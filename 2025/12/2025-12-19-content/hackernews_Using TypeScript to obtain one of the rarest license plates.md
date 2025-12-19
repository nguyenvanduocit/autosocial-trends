---
source: hackernews
title: Using TypeScript to obtain one of the rarest license plates
url: https://www.jack.bio/blog/licenseplate
date: 2025-12-19
---

![logo](/_next/image?url=%2Fmonogram-light.png&w=48&q=75)[home](/)[blog](/blog)

# Using TypeScript to Obtain One of the Rarest License Plates

# Using TypeScript to Obtain One of the Rarest License Plates

December 17, 2025 · [#programming](/blog/tags/programming) · 1,486 views

Most people never think twice about the random mix of letters and numbers the DMV assigns them.

I'm not one of those people.

Online, I've always chased having a clean and memorable digital identity. Over the years, I've been able to pick up handles like my first + last name on Instagram (@jlaf) and full words across platforms (@explain, @discontinue). So when the DMV mailed me my third reminder to renew my registration, that same instinct kicked in: why *hadn't* I considered getting a distinctive plate combination of my own?

In the world of license plates exists a rarity hierarchy:

* Single number license plates (10 possible)
* Repeating number license plates (10 possible)
* Single letter license plates (26 possible)
* Repeating letter combinations (??? possible)
* Two letter plate combinations (676 possible)

After some research about the history these rare plates, my curiosity got the best of me. How rare could you really go? And how far can you push a state's public lookup tools to find out?

## PlateRadar & the Monopoly

As it stands right now, there's a single resource to find mass information on license plate availability: PlateRadar. PlateRadar, like any smart website, recognizes that this data is definitely worth something to someone - and as a result, hides any information that might be deemed rare behind a 20 dollar a month paywall. The site also refreshes every 24 hours, and from my history with rare usernames I know that time is of the absolute essence when snagging something rare. 24 hours wasn't going to cut it.

![Image.png](/images/licenseplate/one.png)

![Image.png](/images/licenseplate/two.png)

Unfortunately for PlateRadar, I'm an engineer and not a normal human being, so I decided to dig in on how vanity plates are deemed available or unavailable.

## Florida's Vanity Plate Checker

Florida, unlike some states (!), provides a website that allows you to check a license plate configuration (meaning the custom sequence of letters/numbers that you want printed on your plate) before you waste your time sitting in line at the tax collector's office. The tool also provides the plate types that support that combination, as different plates also allow different character limits (for example, some only permit 5 characters while allow others up to 7 characters).

![Image.png](/images/licenseplate/three.png)

Thankfully, the site had the nifty feature to check more than a single combination at a time, with no additional delay in the request. I was submitting some combinations manually before realizing that I was able to make requests pretty fast manually - so what if I just automated this whole process?

### The Rate is Limitless

I fired up Burp Suite and proxied a request to the service. What came through looked like this:

```
POST https://services.flhsmv.gov/mvcheckpersonalplate/ HTTP/1.1

__VIEWSTATE=/wEPDwULLTE2Nzg2NjE0NDgPZBYCZg9kFgICAw9kFgICAQ9kFgwCBQ8PFgIeBFRleHQFCUFWQUlMQUJMRWRkAgcPDxYCHgdWaXNpYmxlZ2RkAgsPDxYCHwAFASBkZAIRDw8WAh8ABQEgZGQCFw8PFgIfAAUBIGRkAh0PDxYCHwAFASBkZGQZj5Nowpt7uQW4i5K8gYM8k2+WSv9Zz0wpvFKj57zF0w==

__VIEWSTATEGENERATOR=0719FE0A

__EVENTVALIDATION=/wEdAAlM0TkirL0XIlY9Dw0k/5tSphigSR1TLsx/PgGne7pkToFkrQPgalhmo+FySJy6U4iQeyzYgJga2PpZFeMkYbpKuFA0Lbs4tsi+aCEe29qpNhTkiCU5GKYk9WuPyhuiSM5sZFBTNc+Q1lCok0SfYOt8+CHI2KGhrgOke/DbhB4LDccabLrTZbd0ckqhWOrhQ2MjwxuXnk/njUGbYQbYHdP4Ds+OFyUVKVe45DGbH/0quQ==

ctl00$MainContent$txtInputRowOne=MYPLATE

ctl00$MainContent$txtInputRowTwo

ctl00$MainContent$txtInputRowThree

ctl00$MainContent$txtInputRowFour

ctl00$MainContent$txtInputRowFive

ctl00$MainContent$btnSubmit=Submit
```

`__VIEWSTATE`, `__VIEWSTATEGENERATOR`, and `__EVENTVALIDATION` immediately tipped me off that this was an ASP.NET Web Form. Granted, this is a government website, so honestly, what else was I expecting?

EVENTVALIDATION is (was?) a novel security measure implemented in 2006 by the ASP.NET team to *"prevents unauthorized requests sent by potentially malicious users from the client [..] to ensure that each and every postback and callback event originates from the expected user interface elements, the page adds an extra layer of validation on events".*

In practice, it's meant to stop forged form submissions, which theoretically sounds like a scraping killer. If I had to fetch a fresh set of these variables before making any form of a request, I'd quickly overwhelm the system with round-trips and get rate-limited almost immediately.

... except there was no ratelimiting. At all.

See, the website had absolutely zero CAPTCHA, IP ratelimiting, or web application firewall stopping an influx of requests from coming in. I quickly verified this by using Burp Repeater to make a number of null payload requests, which all returned a status code of 200 Successful.

Once I realized this, I quickly threw a script together to automate the entire process. The workflow looks something like this:

1. Fetch the page once using real browser headers, which loads the ASP.NET form and gives me `__VIEWSTATE`, `__VIEWSTATEGENERATOR` and `__EVENTVALIDATION` - and the power to make a legitimate POST request.
2. Extract the values from the form using a Regex helper.

```
function extractFormFields(html: string): {

viewState: string;

viewStateGenerator: string;

eventValidation: string;

} {

const viewStateMatch = html.match(/id="__VIEWSTATE"\s+value="([^"]+)"/);

const viewStateGeneratorMatch = html.match(

/id="__VIEWSTATEGENERATOR"\s+value="([^"]+)"/

);

const eventValidationMatch = html.match(

/id="__EVENTVALIDATION"\s+value="([^"]+)"/

);

if (!viewStateMatch || !viewStateGeneratorMatch || !eventValidationMatch) {

throw new Error("Failed to extract required form fields from page");

}

return {

viewState: viewStateMatch[1],

viewStateGenerator: viewStateGeneratorMatch[1],

eventValidation: eventValidationMatch[1],

};

}
```

3. Build the POST request with all necessary fields. The actual plate combinations were submitted through `ctl00$MainContent$txtInputRowXXX`, where XXX was `one` through `five`. Using this let me check plate availability 5x faster - and when checking thousands of license plate combinations at a time, it definitely matters.

```
function buildFormData(

plates: string[],

viewState: string,

viewStateGenerator: string,

eventValidation: string

): string {

const params = new URLSearchParams();

params.append("__VIEWSTATE", viewState);

params.append("__VIEWSTATEGENERATOR", viewStateGenerator);

params.append("__EVENTVALIDATION", eventValidation);

const fieldNames = [

"ctl00$MainContent$txtInputRowOne",

"ctl00$MainContent$txtInputRowTwo",

"ctl00$MainContent$txtInputRowThree",

"ctl00$MainContent$txtInputRowFour",

"ctl00$MainContent$txtInputRowFive",

];

for (let i = 0; i < 5; i++) {

params.append(

fieldNames[i],

i < plates.length ? plates[i].toUpperCase() : ""

);

}

params.append("ctl00$MainContent$btnSubmit", "Submit");

return params.toString();

}
```

4. Submit the POST request and parse the body! Thankfully, the site returned a big ol' `AVAILABLE` or `NOT AVAILABLE` for each plate combo, so that was easy enough to check in code:

```
function extractPlateStatuses(

html: string,

plates: string[]

): PlateCheckResult[] {

const results: PlateCheckResult[] = [];

const labelIds = [

"MainContent_lblOutPutRowOne",

"MainContent_lblOutPutRowTwo",

"MainContent_lblOutputRowThree",

"MainContent_lblOutputRowFour",

"MainContent_lblOutputRowFive",

];

for (let i = 0; i < plates.length; i++) {

const labelId = labelIds[i];

const regex = new RegExp(`id="${labelId}"[^>]*>([^<]*)<`, "i");

const match = html.match(regex);

const status = match ? match[1].trim() : "";

const available = status.toUpperCase() === "AVAILABLE";

results.push({

plate: plates[i],

available,

status: status || "UNKNOWN",

});

}

return results;

}
```

## The Plate War of '25

Once the script was running smoothly, I created a small microservice that added the results to a Postgres database with the plate combination, along with the last time it was checked. For smaller, high-value combinations (eg, any of the single letter / double letter combinations), I constantly polled every hour or two to check availability. What I *didn't* realize at the time was the system updated in real time. The moment someone reserved a plate, the Florida DMV's backend reflected the change on the next lookup.

To visualize the data I had scraped, I built a quick Next.js frontend that let me browse through results, filter combinations, and batch-upload plate lists from a text file for quick checking.

![Image.png](/images/licenseplate/four.png)

I found some really cool plate combinations, like `WEBSITE`, `SITE`, and `CAPTCHA` . But nothing compared to the spotting one of the only remaining two-letter combination I had seen during my search: `EO`.

I saw that `EO` was available on November 26th. With Thanksgiving, Black Friday, and the entire weekend shutting down state offices, I assumed I had plenty of time to stroll into the Tax Collector's office and grab it.

December 1st rolled around and I hopped in my car at 9:30am to head towards the tax collector's office. While driving, I got a notification from my service that `EO` was no longer available. Someone had the same idea as me, and clearly must have arrived when their doors opened right at 8am. I turned the car around, defeated, and went home.

When I had gotten home, out of spite (and curiosity) I decided to re-run a full check on all two letter license plates.

![Image.png](/images/licenseplate/five.png)

Just like that, by some weird divine timing alignment, another two-letter combination had popped back into availability.

My wallowing quickly ended, and I got right back in my car and drove straight to the office. After almost an hour long wait (and a conversation with a slightly confused but very patient office clerk listening to my explanation), I was able to make the reservation. HY was officially my license plate.

![Image.png](/images/licenseplate/six.png)

I'd show you a picture, but unfortunately Florida runs on a 60-day delivery timeline for custom plates. Still: it exists, it's paid for, and it's proof that with a little TypeScript and an unreasonable amount of determination, you can claim just about anything.

You might also like...

[Getting a Cease and Desist from Waffle House](/blog/wafflehouse)

Copy Link[GitHub](https://github.com/jacc)[Twitter](https://www.x.com/1afond)[LinkedIn](https://www.linkedin.com/in/jacklafond/)
