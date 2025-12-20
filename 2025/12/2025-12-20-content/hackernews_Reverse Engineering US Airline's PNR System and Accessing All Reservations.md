---
source: hackernews
title: Reverse Engineering US Airline's PNR System and Accessing All Reservations
url: https://alexschapiro.com/security/vulnerability/2025/11/20/avelo-airline-reservation-api-vulnerability
date: 2025-12-20
---

[Alex Schapiro](/)
[ ]

[About](/about/)

# Brute-Forceable Airline Reservation API Left Millions of Passenger Records Vulnerable

## A 6-hour brute-force attack could have downloaded every Avelo Airline passenger's PII, Known Traveler Number, and payment data.

Nov 20, 2025

*Timeline & Responsible Disclosure*

***Initial Contact:** Upon discovering this vulnerability on **October 15, 2025**, I immediately reached out to security contacts at Avelo Airlines via email.*

***October 16, 2025:** The Avelo cybersecurity team responded quickly and professionally. We had productive email exchanges where I detailed the vulnerability, including the lack of last name verification and rate limiting on reservation endpoints.*

***November 13, 2025:** Avelo pushed a fix to production and notified me that the vulnerabilities were patched. I independently verified the fixes were in place before publication, and informed the Avelo team of my intention to write a technical blog post about this vulnerability, highlighting their cooperative and responsive approach to security disclosure.*

***Publication:** November 20, 2025.*

*The Avelo team was responsive, professional, and took the findings seriously throughout the disclosure process. They acknowledged the severity, worked quickly to remediate the issues, and maintained clear communication. This is a model example of how organizations should handle security disclosures.*

---

After my [9 AM Akkadian class](https://coursetable.com/catalog?selectSeasons=202503&searchText=akkad&course-modal=202503-13154), I sat down to change my flight out of New Haven with Avelo Airlines, and noticed that my computer was making some unusual requests. After digging a little further, I stepped into a landmine of customer information exposure. In the wrong hands, this critical vulnerability could allow an attacker to access full reservation details, including PII, government ID numbers, and partial payment info, for every Avelo passenger, past and present.

Before I walk you through my work on that Tuesday morning, let’s establish how airlines generally manage their reservations.

## How Airline Logins *Should* Work

Normally, to access a flight reservation (which often contains sensitive information like passport numbers, Known Traveler Numbers, and partial credit card data), you need at least two pieces of information: a **confirmation code** and the **passenger’s last name**.

This two-factor system is generally secure. The space of all 6-character alphanumeric confirmation codes combined with all possible last names is astronomically large, making it impossible to “guess” a valid pair.

But what if the last name check was missing?

Suddenly, the problem becomes much simpler. The *entire* keyspace an attacker needs to guess is just the confirmation code. In Avelo’s case, their codes are 6-character alphanumeric strings (`[A-Z0-9]`).

Let’s do the math:

* **Keyspace:** 36 characters (26 letters + 10 digits)
* **Length:** 6
* **Total Combinations:** 36^6 = **2,176,782,336** (~2.18 billion)

That’s a big number, but it’s not “astronomically large.” It’s well within the reach of a modern brute-force attack.

### The Attack Timeline

How long would it take to try all 2.18 billion combinations? The time is just `2.18 billion / (requests per second)`.

* At **1,000 req/s** (a modest script): 2.18 million seconds, or **~25 days**.
* At **10,000 req/s** (a decent server): 218,000 seconds, or **~2.5 days**.
* At **100,000 req/s** (a small cluster of servers, costing $400-$700)[1](#fn:1): 21,800 seconds, or **~6 hours**.

**Bottom line:** If Avelo’s flight system has no rate limiting and doesn’t require a last name, an adversary could extract all passenger data in about 6 hours for less than a thousand dollars.

### Even Faster Than 6 Hours

Even worse, they don’t need to run for 6 hours. With an estimated 8 million tickets sold, the “hit rate” is roughly **1 in every 270 guesses** (2.18B / 8M). An attacker would start getting valid PII back in *seconds*.

## Back to the Story: Finding the Flaw

This was all just theory until I looked at my network traffic. As I was changing my reservation, I saw a GET request to an API endpoint:

```
https://www.aveloair.com/payment/services/reservation/{code}
```

![Screenshot of the reservation API endpoint](/assets/images/avelo-security/endpoint.png)

The parameter at the end didn’t seem like a reservation code, but the response contained all relevant reservation data, so I decided to probe further. On a hunch, I swapped that token for my *actual* 6-character code and re-sent the request.

**Voila.** The server responded with a massive JSON object containing my entire reservation.

This endpoint wasn’t asking for my last name. The only other security was a standard authentication cookie… but was that cookie tied to *my* reservation?

I quickly texted a friend for their old Avelo confirmation code. I plugged it into the URL, kept *my own cookie*, and hit send. But there was no way it could poss-

**It worked.**

I was looking at their full reservation. **Any** valid authentication cookie could be used to query **any** reservation, using only the 6-character code. The theoretical flaw was real.

## Executing the Attack: No Rate Limiting

The only remaining (partial) defense was rate-limiting. I wrote a quick multi-threaded Python script to generate random 6-character codes and hit the endpoint.

The requests flew. There was no WAF, no IP blocking, no CAPTCHA.

![Valid reservation codes being discovered](/assets/images/avelo-security/validcodes.png)
*The script quickly finding valid reservation codes*

Within minutes, my script was logging hundreds of valid reservations. Troves of data were being returned, including from passengers flying on government business with `@dot.gov` and `@faa.gov` email addresses.

A successful hit returned the *entire* reservation object. This was a complete data breach for each passenger – including myself!

(Note: During further testing, I discovered a similar vulnerability on a different reservation endpoint. I promptly notified the Avelo team, and they patched that endpoint as well before publication.)

## What Data Was Leaked?

For every valid code, the API returned:

* **Full Passenger PII:** `FullName`, `DateOfBirth`, `Gender`
* **Government IDs:** `IDDocuments.IDNumber` (this field contained Known Traveler Numbers (KNTs) and, in other cases, Passport Numbers)
* **Contact Info:** phone numbers, email addresses
* **Full Itinerary:** Flight numbers, dates, times, and `SeatLocation`
* **Payment Details:** `CardNumber` (masked: `************8`), `DateTimeExpiration`, and billing `Address.PostalCode`
* **Vouchers:** `PaymentInternals.AccountNumber` and `Amount.Value`
* **PCI Data:** `PaymentCards.TrackData` — This field seemed to contain partial magnetic-stripe data

![Payment card data in API response](/assets/images/avelo-security/payment.png)

Example of exposed payment card data returned by the API

![Known Traveler Number in API response](/assets/images/avelo-security/ktn.png)

Example of exposed Known Traveler Number (KNT) and other PII in API response

## The Fallout

This flaw was critical. An attacker could:

1. Run the 6-hour brute-force attack to enumerate millions of valid passenger reservation codes (PNRs) — or simply run the script for a few minutes and start harvesting valid passenger data immediately
2. Extract comprehensive PII including full names, dates of birth, contact information, flight itineraries, and government ID numbers (Known Traveler Numbers and passport numbers) for identity theft and fraud
3. Access partial payment card data including last 4 digits, expiration dates, and billing zip codes
4. View complete travel history and passenger boarding status
5. Modify or cancel all Avelo passengers’ reservations, causing widespread travel disruption

I immediately disclosed this to the Avelo team. They were responsive, professional, and took the findings seriously, patching the issues promptly.

## Key Takeaways

This incident is a stark reminder of how critical simple security checks are. A single missing `lastName` check and an absent rate-limit configuration exposed millions of sensitive passenger records to trivial enumeration.

**For developers:**

* Always require multiple factors for accessing sensitive data (e.g., confirmation code + last name)
* Implement rate limiting on all enumerable endpoints
* Ensure authentication cookies are properly scoped to user sessions

I’m glad we could get this fixed, and I hope this write-up helps other developers avoid similar pitfalls.

1. AWS Lambda: requests billed at $0.20 per million plus compute billed per GB‑second; at 2.18B requests, request charges are about 2,176.8 million × $0.20 ≈ $435 [↩](#fnref:1)

## Alex Schapiro

* Alex Schapiro
* [About](/about/)
* [alex\_dot\_schapiro\_at\_yale\_dot\_edu](/cdn-cgi/l/email-protection#21404d44597e454e557e524249405148534e7e40557e58404d447e454e557e444554)

* [bearsyankees](https://github.com/bearsyankees)
* [aschap](https://www.linkedin.com/in/aschap)

Security research, ethical hacking, and building stuff.
