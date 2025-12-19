---
source: hackernews
title: We pwned X, Vercel, Cursor, and Discord through a supply-chain attack
url: https://gist.github.com/hackermondev/5e2cdc32849405fff6b46957747a2d28
date: 2025-12-19
---

[Skip to content](#start-of-content)

Search Gists

Search Gists

[All gists](/discover)
[Back to GitHub](https://github.com)
[Sign in](https://gist.github.com/auth/github?return_to=https%3A%2F%2Fgist.github.com%2Fhackermondev%2F5e2cdc32849405fff6b46957747a2d28)
[Sign up](/join?return_to=https%3A%2F%2Fgist.github.com%2Fhackermondev%2F5e2cdc32849405fff6b46957747a2d28&source=header-gist)

[Sign in](https://gist.github.com/auth/github?return_to=https%3A%2F%2Fgist.github.com%2Fhackermondev%2F5e2cdc32849405fff6b46957747a2d28) [Sign up](/join?return_to=https%3A%2F%2Fgist.github.com%2Fhackermondev%2F5e2cdc32849405fff6b46957747a2d28&source=header-gist)

You signed in with another tab or window. Reload to refresh your session.
You signed out in another tab or window. Reload to refresh your session.
You switched accounts on another tab or window. Reload to refresh your session.

Dismiss alert

{{ message }}

Instantly share code, notes, and snippets.

[![@hackermondev](https://avatars.githubusercontent.com/u/60828015?s=64&v=4)](/hackermondev)

# [hackermondev](/hackermondev)/**[writeup.md](/hackermondev/5e2cdc32849405fff6b46957747a2d28)**

Last active
December 18, 2025 23:44

Show Gist options

* [Download ZIP](/hackermondev/5e2cdc32849405fff6b46957747a2d28/archive/89c4c075920764b4665c0b5e4b25269c707d4eb9.zip)

* [Star

  23
  (23)](/login?return_to=https%3A%2F%2Fgist.github.com%2Fhackermondev%2F5e2cdc32849405fff6b46957747a2d28)You must be signed in to star a gist
* [Fork

  0
  (0)](/login?return_to=https%3A%2F%2Fgist.github.com%2Fhackermondev%2F5e2cdc32849405fff6b46957747a2d28)You must be signed in to fork a gist

* Embed

  # Select an option

  + Embed
     Embed this gist in your website.
  + Share
     Copy sharable link for this gist.
  + Clone via HTTPS
     Clone using the web URL.

  ## No results found

  [Learn more about clone URLs](https://docs.github.com/articles/which-remote-url-should-i-use)

  Clone this repository at &lt;script src=&quot;https://gist.github.com/hackermondev/5e2cdc32849405fff6b46957747a2d28.js&quot;&gt;&lt;/script&gt;
* Save hackermondev/5e2cdc32849405fff6b46957747a2d28 to your computer and use it in GitHub Desktop.

[Code](/hackermondev/5e2cdc32849405fff6b46957747a2d28)
[Revisions
4](/hackermondev/5e2cdc32849405fff6b46957747a2d28/revisions)
[Stars
23](/hackermondev/5e2cdc32849405fff6b46957747a2d28/stargazers)

Embed

# Select an option

* Embed
   Embed this gist in your website.
* Share
   Copy sharable link for this gist.
* Clone via HTTPS
   Clone using the web URL.

## No results found

[Learn more about clone URLs](https://docs.github.com/articles/which-remote-url-should-i-use)

Clone this repository at &lt;script src=&quot;https://gist.github.com/hackermondev/5e2cdc32849405fff6b46957747a2d28.js&quot;&gt;&lt;/script&gt;

Save hackermondev/5e2cdc32849405fff6b46957747a2d28 to your computer and use it in GitHub Desktop.

[Download ZIP](/hackermondev/5e2cdc32849405fff6b46957747a2d28/archive/89c4c075920764b4665c0b5e4b25269c707d4eb9.zip)

How we pwned X (Twitter), Vercel, Cursor, Discord, and hundreds of companies through a supply-chain attack

[Raw](/hackermondev/5e2cdc32849405fff6b46957747a2d28/raw/89c4c075920764b4665c0b5e4b25269c707d4eb9/writeup.md)

[**writeup.md**](#file-writeup-md)

hi, i'm daniel. i'm a 16-year-old high school senior. in my free time, i [hack billion dollar companies](https://hackerone.com/daniel) and build cool stuff.

about a month ago, a couple of friends and I found serious critical vulnerabilities on Mintlify, an AI documentation platform used by some of the top companies in the world.

i found a critical cross-site scripting vulnerability that, if abused, would let an attacker to inject malicious scripts into the documentation of numerous companies and steal credentials from users with a single link open.

(go read my friends' writeups (after this one))
[how to hack discord, vercel, and more with one easy trick (eva)](https://kibty.town/blog/mintlify/)
[Redacted by Counsel: A supply chain postmortem (MDL)](https://heartbreak.ing/)

here's my story...

# Discord

My story begins on Friday, November 7, 2025, when Discord announced a brand new update to their developer documentation platform. They were previously using a custom built documentation platform, but were switching to an AI-powered documentation platform.

[![image](https://private-user-images.githubusercontent.com/60828015/523279902-58ed99c4-ba37-4e6a-a782-3004ac12ab96.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjYxMDI4MDksIm5iZiI6MTc2NjEwMjUwOSwicGF0aCI6Ii82MDgyODAxNS81MjMyNzk5MDItNThlZDk5YzQtYmEzNy00ZTZhLWE3ODItMzAwNGFjMTJhYjk2LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTEyMTklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMjE5VDAwMDE0OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWUwYjIzNzZjMTIyMDNlZjdkNDk4MjFiMjBhMWYwY2M0MzM5NWFlYWZjMzNhZWUwYjA1YjhiN2FmMGQ0M2RmY2ImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.iyR68njGMUsUSeW0swxVDNFszb6icf4fdSXvOrk2jAw)](https://private-user-images.githubusercontent.com/60828015/523279902-58ed99c4-ba37-4e6a-a782-3004ac12ab96.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjYxMDI4MDksIm5iZiI6MTc2NjEwMjUwOSwicGF0aCI6Ii82MDgyODAxNS81MjMyNzk5MDItNThlZDk5YzQtYmEzNy00ZTZhLWE3ODItMzAwNGFjMTJhYjk2LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTEyMTklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMjE5VDAwMDE0OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWUwYjIzNzZjMTIyMDNlZjdkNDk4MjFiMjBhMWYwY2M0MzM5NWFlYWZjMzNhZWUwYjA1YjhiN2FmMGQ0M2RmY2ImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.iyR68njGMUsUSeW0swxVDNFszb6icf4fdSXvOrk2jAw)

Discord is one of my favorite places to hunt for vulnerabilities since I'm very familiar with their API and platform. I'm at the top of their bug bounty leaderboard having reported nearly 100 vulnerabilities over the last few years. After you've gone through every feature at least 10 times, it gets boring.

I found this new update exciting, and as soon as I saw the announcement, I started looking through how they implemented this new documentation platform.

# Mintlify

Mintlify is an AI-powered documentation platform. You write your documentation as markdown and Mintlify turns it into a beautiful documentation platform with all the modern features a documentation platform needs. (Despite the vulnerabilities we found, I would highly recommend them. They make it really easy to create beautiful docs that work.)

Mintlify-hosted documentation sites are on the \*.mintlify.app domains, with support for custom domains. In Discord's case, they were just proxying certain routes to their Mintlify documentation at `discord.mintlify.app`.

Every Mintlify subdomain has a `/_mintlify/*` path that is used internally on the platform to power certain features. Regardless of whether it's hosted through the `mintlify.app` domain or a custom domain, the `/_mintlify` path must be accessible to power the documentation.
[![image](https://private-user-images.githubusercontent.com/60828015/523285391-4cca3f2c-52ce-4d9c-a713-3d6a4d8e1b9b.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjYxMDI4MDksIm5iZiI6MTc2NjEwMjUwOSwicGF0aCI6Ii82MDgyODAxNS81MjMyODUzOTEtNGNjYTNmMmMtNTJjZS00ZDljLWE3MTMtM2Q2YTRkOGUxYjliLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTEyMTklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMjE5VDAwMDE0OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTU0NWFmZjg4OTMwNDRhNzFkMGEwZmIyMjc5MzU2ZjY5YTAzMWE2NzRmMzUzMDA0ZWI5NTZhM2RiMTdhNzFmNDMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.5LQZXtl1KysNsQvCLmtR3lVZSimNvtKthE05odt2sQg)](https://private-user-images.githubusercontent.com/60828015/523285391-4cca3f2c-52ce-4d9c-a713-3d6a4d8e1b9b.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjYxMDI4MDksIm5iZiI6MTc2NjEwMjUwOSwicGF0aCI6Ii82MDgyODAxNS81MjMyODUzOTEtNGNjYTNmMmMtNTJjZS00ZDljLWE3MTMtM2Q2YTRkOGUxYjliLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTEyMTklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMjE5VDAwMDE0OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTU0NWFmZjg4OTMwNDRhNzFkMGEwZmIyMjc5MzU2ZjY5YTAzMWE2NzRmMzUzMDA0ZWI5NTZhM2RiMTdhNzFmNDMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.5LQZXtl1KysNsQvCLmtR3lVZSimNvtKthE05odt2sQg)

(For example, the `/api/user` path for authentication: <https://docs.x.com/_mintlify/api/user>, <https://discord.com/_mintlify/api/user>, etc)

# `/_mintlify/markdown/`

After Discord switched to Mintlify and when I started looking for bugs on the platform, from the get-go, my plan was to find a way to render another Mintlify documentation through Discord's domain.

At first, I tried path traversal attacks, but they didn't work. Then, I started looking through the `/_mintlify` API endpoints.

Using Chrome DevTools to search the assets, I found the endpoint `/_mintlify/_markdown/_sites/[subdomain]/[...route]`. It accepted any Mintlify documentation (`[subdomain]`) and it returned a file from that specific documentation (`[...route]`). The endpoint didn't check to make sure the `[subdomain]` matched with the current host, which means you could fetch files from any Mintlify documentation on an host with the `/_mintlify/` route.

Unfortunately, this endpoint only returned raw markdown text. The markdown wasn't rendered as HTML, meaning it was impossible to run code. I spent the rest of the time trying different ways to bypass this, but nothing worked.

# `/_mintlify/static/`

Fast forward 2 days to Sunday, November 9, 2025, I went back to hunting.

I was confident there was another endpoint, like the markdown one, which could fetch and return cross-site data, but I couldn't find one. I tried searching web assets and some other techniques, but I couldn't find the endpoint I was looking for.

Finally, I decided to look through the Mintlify CLI. Mintlify lets you run your documentation site locally via their npm package (@mintlify/cli). I realized that this probably meant the code powering the documentation platform was somewhat public.

After digging through the package and downloading tarballs linked in the code, I found myself at exactly what I was looking for.

[![image](https://private-user-images.githubusercontent.com/60828015/523295339-700c1c7d-69da-440e-8022-4a9f59902871.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjYxMDI4MDksIm5iZiI6MTc2NjEwMjUwOSwicGF0aCI6Ii82MDgyODAxNS81MjMyOTUzMzktNzAwYzFjN2QtNjlkYS00NDBlLTgwMjItNGE5ZjU5OTAyODcxLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTEyMTklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMjE5VDAwMDE0OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTUxODEzNDg4ZTJjM2Q5NWVjYTc2ZWM5YjA2OTQ0ZDVlNTM4ZDRjM2IwOGMzYWRkZTgzYjU3Nzg0MmE5MzhjMmQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.E0_mcR3rVXC1pXwX6tWDG8bhfr4CoGKdh0u5rDsuFVw)](https://private-user-images.githubusercontent.com/60828015/523295339-700c1c7d-69da-440e-8022-4a9f59902871.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjYxMDI4MDksIm5iZiI6MTc2NjEwMjUwOSwicGF0aCI6Ii82MDgyODAxNS81MjMyOTUzMzktNzAwYzFjN2QtNjlkYS00NDBlLTgwMjItNGE5ZjU5OTAyODcxLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTEyMTklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMjE5VDAwMDE0OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTUxODEzNDg4ZTJjM2Q5NWVjYTc2ZWM5YjA2OTQ0ZDVlNTM4ZDRjM2IwOGMzYWRkZTgzYjU3Nzg0MmE5MzhjMmQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.E0_mcR3rVXC1pXwX6tWDG8bhfr4CoGKdh0u5rDsuFVw)

Jackpot!

This was a list of application endpoints (compiled by Nextjs), and in the middle, there's the endpoint `/_mintlify/static/[subdomain]/[...route]`.

Like the markdown endpoint, this endpoint accepted any Mintlify documentation (`[subdomain]`). The only difference was this endpoint returned static files from the documentation repo.

First, I tried accessing HTML and JavaScript files but it didn't work; I realized there was some sort of whitelist of file extensions. Then, I tried an SVG file, and it worked.

If you didn't know, you can embed JavaScript into an SVG file. The script doesn't run unless the file is directly opened (you can't run scripts from (`<img src="/image.svg">`). This is very common knowledge for security researchers.

I created an SVG file with an embedded script, uploaded it to my Mintlify documentation, and opened the endpoint through Discord (<https://discord.com/_mintlify/_static/hackerone-a00f3c6c/lmao.svg>). It worked!
[![image](https://private-user-images.githubusercontent.com/60828015/523299352-256777fb-9d84-4a4b-b847-3261e00d398d.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjYxMDI4MDksIm5iZiI6MTc2NjEwMjUwOSwicGF0aCI6Ii82MDgyODAxNS81MjMyOTkzNTItMjU2Nzc3ZmItOWQ4NC00YTRiLWI4NDctMzI2MWUwMGQzOThkLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTEyMTklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMjE5VDAwMDE0OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTkwNDNiYmM5YjJkMGJhNTVkZTU5NWE5OWQ0MjZmYjA3Y2M3MDQ3NzdmNmFkNTc4YjViZDY0Y2M2NmE2M2NiZmImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.TE8YGLzUxAj8E-kVvgi877EjKqrAubIsq3LNkdALDuc)](https://private-user-images.githubusercontent.com/60828015/523299352-256777fb-9d84-4a4b-b847-3261e00d398d.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjYxMDI4MDksIm5iZiI6MTc2NjEwMjUwOSwicGF0aCI6Ii82MDgyODAxNS81MjMyOTkzNTItMjU2Nzc3ZmItOWQ4NC00YTRiLWI4NDctMzI2MWUwMGQzOThkLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTEyMTklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMjE5VDAwMDE0OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTkwNDNiYmM5YjJkMGJhNTVkZTU5NWE5OWQ0MjZmYjA3Y2M3MDQ3NzdmNmFkNTc4YjViZDY0Y2M2NmE2M2NiZmImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.TE8YGLzUxAj8E-kVvgi877EjKqrAubIsq3LNkdALDuc)

# Collaboration

XSS attacks are incredibly rare on Discord, so I shared it with a couple friends.

I sent a screenshot to xyzeva, only to find out she had also been looking into Mintlify after the Discord switch. She had previously discovered other vulnerabilities on the Mintlify platform, and had found more that she was preparing to disclose ([go read her writeup!](https://kibty.town/blog/mintlify/)). I find it funny we had both separately been looking into Mintlify and found very different, but very critical bugs.

[![image](https://private-user-images.githubusercontent.com/60828015/527263164-783c899c-5e85-4aad-8303-c0fccc3c7b4e.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjYxMDI4MDksIm5iZiI6MTc2NjEwMjUwOSwicGF0aCI6Ii82MDgyODAxNS81MjcyNjMxNjQtNzgzYzg5OWMtNWU4NS00YWFkLTgzMDMtYzBmY2NjM2M3YjRlLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTEyMTklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMjE5VDAwMDE0OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWZmZWE1YjM5NWVjMWI4OTRjNzQ3ZGRlNzM1NTRiN2E2OTAzZDU0NTk5NjE1NmY3ZDA2ZWQxODMxODczMmJmNDcmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.NTGMjWKnbTQwX-uXHuoXwhQKLfLy7RJVjJA8PdK9KuM)](https://private-user-images.githubusercontent.com/60828015/527263164-783c899c-5e85-4aad-8303-c0fccc3c7b4e.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjYxMDI4MDksIm5iZiI6MTc2NjEwMjUwOSwicGF0aCI6Ii82MDgyODAxNS81MjcyNjMxNjQtNzgzYzg5OWMtNWU4NS00YWFkLTgzMDMtYzBmY2NjM2M3YjRlLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTEyMTklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMjE5VDAwMDE0OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWZmZWE1YjM5NWVjMWI4OTRjNzQ3ZGRlNzM1NTRiN2E2OTAzZDU0NTk5NjE1NmY3ZDA2ZWQxODMxODczMmJmNDcmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.NTGMjWKnbTQwX-uXHuoXwhQKLfLy7RJVjJA8PdK9KuM)

Another friend joined, and we created a group chat.

[![image](https://private-user-images.githubusercontent.com/60828015/525738216-ce48ea02-e88b-4c90-b121-554fd9590f6a.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjYxMDI4MDksIm5iZiI6MTc2NjEwMjUwOSwicGF0aCI6Ii82MDgyODAxNS81MjU3MzgyMTYtY2U0OGVhMDItZTg4Yi00YzkwLWIxMjEtNTU0ZmQ5NTkwZjZhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTEyMTklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMjE5VDAwMDE0OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTk5YzU1NDYzMGYwNWQzYjc3NmYxZTlkMTI5NWU1NTBkMzY2ZTc4Mjc5NTkxYWNkMzNkMmRlMDcxNjlkYzIxYmYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.-jhaDpIIYP3HyUhEmpVC-DTibRUIdOUCeWTNmuCojII)](https://private-user-images.githubusercontent.com/60828015/525738216-ce48ea02-e88b-4c90-b121-554fd9590f6a.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjYxMDI4MDksIm5iZiI6MTc2NjEwMjUwOSwicGF0aCI6Ii82MDgyODAxNS81MjU3MzgyMTYtY2U0OGVhMDItZTg4Yi00YzkwLWIxMjEtNTU0ZmQ5NTkwZjZhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTEyMTklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMjE5VDAwMDE0OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTk5YzU1NDYzMGYwNWQzYjc3NmYxZTlkMTI5NWU1NTBkMzY2ZTc4Mjc5NTkxYWNkMzNkMmRlMDcxNjlkYzIxYmYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.-jhaDpIIYP3HyUhEmpVC-DTibRUIdOUCeWTNmuCojII)

# Reporting

We reported the vulnerability to Discord and attempted to contact Mintlify through an employee.

Discord took this very seriously, and closed off its entire developer documentation for 2 hours while investigating the impact of this vulnerability. Then, they reverted to their old documentation platform and removed all the Mintlify routes.
<https://discordstatus.com/incidents/by04x5gnnng3>

[![image](https://private-user-images.githubusercontent.com/60828015/523303432-780d56bc-7493-48bf-9b0e-4005bd3bac11.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjYxMDI4MDksIm5iZiI6MTc2NjEwMjUwOSwicGF0aCI6Ii82MDgyODAxNS81MjMzMDM0MzItNzgwZDU2YmMtNzQ5My00OGJmLTliMGUtNDAwNWJkM2JhYzExLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTEyMTklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMjE5VDAwMDE0OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWI0YjBjZTgwZDllNjYxODk2NTIwZDQ1YjhhZGZhNGYzODAxOGNmMjU1YmU0OTFlMmY3MzViYTEzNmFiZjM3ODQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.KKADdOowSo3QIQG7CC_x3KVW_x1cEeiPBoxAkAnM78o)](https://private-user-images.githubusercontent.com/60828015/523303432-780d56bc-7493-48bf-9b0e-4005bd3bac11.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjYxMDI4MDksIm5iZiI6MTc2NjEwMjUwOSwicGF0aCI6Ii82MDgyODAxNS81MjMzMDM0MzItNzgwZDU2YmMtNzQ5My00OGJmLTliMGUtNDAwNWJkM2JhYzExLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTEyMTklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMjE5VDAwMDE0OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWI0YjBjZTgwZDllNjYxODk2NTIwZDQ1YjhhZGZhNGYzODAxOGNmMjU1YmU0OTFlMmY3MzViYTEzNmFiZjM3ODQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.KKADdOowSo3QIQG7CC_x3KVW_x1cEeiPBoxAkAnM78o)

Mintlify contacted us directly very shortly after hearing about the vulnerability through Discord. We set up a Slack channel with Mintlify's engineering team and got to work. Personally, this cross-site scripting attack was the only thing I had the time to find; eva and MDL worked with Mintlify's engineering team to quickly remediate this and other vulnerabilities they found on the platform.

# Impact

In total, the cross-site scripting attack affected almost every Mintlify customer. To name a few: X (Twitter), Vercel, Cursor, Discord, [and more](https://mintlify.com/customers).

These customers host their documentation on their primary domains and were vulnerable to account takeovers with a single malicious link.

# Conclusion

Fortunately, we responsibly found and disclosed this vulnerability but this is an example of how compromising a single supply chain can lead to a multitude of problems.

In total, we collectively recieved ~$11,000 in bounties. Discord paid $4,000 and Mintlify individually gave us bounties for the impact of the bugs we individually found.

[![@joekrejci](https://avatars.githubusercontent.com/u/484893?s=80&v=4)](/joekrejci)

Copy link

### **[joekrejci](/joekrejci)** commented [Dec 18, 2025](/hackermondev/5e2cdc32849405fff6b46957747a2d28?permalink_comment_id=5910922#gistcomment-5910922)

[![](https://camo.githubusercontent.com/becca88a3770f921b86efae55e6c69f146781cc527dd327c71383937f3a34297/68747470733a2f2f6d65646961312e67697068792e636f6d2f6d656469612f76312e59326c6b50574a6b4d3256684e54646c5a335a326233427962475a766233686e633370785a47307a623230344e7a4a715a544131624845315a48426d4e48566e5a3246754f435a6c634431324d56396e61575a7a58334e6c59584a6a61435a6a6444316e2f6d43496a436773336e575157664a5a7650412f67697068792d646f776e73697a65642d6d656469756d2e676966)](https://camo.githubusercontent.com/becca88a3770f921b86efae55e6c69f146781cc527dd327c71383937f3a34297/68747470733a2f2f6d65646961312e67697068792e636f6d2f6d656469612f76312e59326c6b50574a6b4d3256684e54646c5a335a326233427962475a766233686e633370785a47307a623230344e7a4a715a544131624845315a48426d4e48566e5a3246754f435a6c634431324d56396e61575a7a58334e6c59584a6a61435a6a6444316e2f6d43496a436773336e575157664a5a7650412f67697068792d646f776e73697a65642d6d656469756d2e676966)

Sorry, something went wrong.

### Uh oh!

There was an error while loading. Please reload this page.

[![@atbe](https://avatars.githubusercontent.com/u/987031?s=80&v=4)](/atbe)

Copy link

### **[atbe](/atbe)** commented [Dec 18, 2025](/hackermondev/5e2cdc32849405fff6b46957747a2d28?permalink_comment_id=5910957#gistcomment-5910957)

*nice*

Sorry, something went wrong.

### Uh oh!

There was an error while loading. Please reload this page.

[![@yard2010](https://avatars.githubusercontent.com/u/6155611?s=80&v=4)](/yard2010)

Copy link

### **[yard2010](/yard2010)** commented [Dec 18, 2025](/hackermondev/5e2cdc32849405fff6b46957747a2d28?permalink_comment_id=5910964#gistcomment-5910964) • edited Loading Uh oh! There was an error while loading. Please reload this page.

Very nice!
Why does Eva have a bot tag?

Daniel I think you deserve more money for this.

Sorry, something went wrong.

### Uh oh!

There was an error while loading. Please reload this page.

[![@aelithron](https://avatars.githubusercontent.com/u/187228556?s=80&v=4)](/aelithron)

Copy link

### **[aelithron](/aelithron)** commented [Dec 18, 2025](/hackermondev/5e2cdc32849405fff6b46957747a2d28?permalink_comment_id=5910966#gistcomment-5910966)

quite an interesting bug, good work! :3

Sorry, something went wrong.

### Uh oh!

There was an error while loading. Please reload this page.

[![@jayzalowitz](https://avatars.githubusercontent.com/u/598844?s=80&v=4)](/jayzalowitz)

Copy link

### **[jayzalowitz](/jayzalowitz)** commented [Dec 18, 2025](/hackermondev/5e2cdc32849405fff6b46957747a2d28?permalink_comment_id=5910976#gistcomment-5910976)

Looks like you...
*puts on glasses*
Made a Mint.

Sorry, something went wrong.

### Uh oh!

There was an error while loading. Please reload this page.

[![@pauljones0](https://avatars.githubusercontent.com/u/32859666?s=80&v=4)](/pauljones0)

Copy link

### **[pauljones0](/pauljones0)** commented [Dec 18, 2025](/hackermondev/5e2cdc32849405fff6b46957747a2d28?permalink_comment_id=5910990#gistcomment-5910990)

11k is a paltry sum... Nation based hacking groups would've paid more. If I'm reading it right, this is a zero day for many companies

Sorry, something went wrong.

### Uh oh!

There was an error while loading. Please reload this page.

[![@zeluspudding](https://avatars.githubusercontent.com/u/8431341?s=80&v=4)](/zeluspudding)

Copy link

### **[zeluspudding](/zeluspudding)** commented [Dec 18, 2025](/hackermondev/5e2cdc32849405fff6b46957747a2d28?permalink_comment_id=5911008#gistcomment-5911008)

Deserve's way more money for this

Sorry, something went wrong.

### Uh oh!

There was an error while loading. Please reload this page.

[![@et0and](https://avatars.githubusercontent.com/u/42124348?s=80&v=4)](/et0and)

Copy link

### **[et0and](/et0and)** commented [Dec 18, 2025](/hackermondev/5e2cdc32849405fff6b46957747a2d28?permalink_comment_id=5911016#gistcomment-5911016)

Very good write up and great work, agree with others here in saying that you should’ve been paid more given the severity of the vulnerability.

Sorry, something went wrong.

### Uh oh!

There was an error while loading. Please reload this page.

[![@dietofworms](https://avatars.githubusercontent.com/u/28944297?s=80&v=4)](/dietofworms)

Copy link

### **[dietofworms](/dietofworms)** commented [Dec 18, 2025](/hackermondev/5e2cdc32849405fff6b46957747a2d28?permalink_comment_id=5911017#gistcomment-5911017)

Nice work. I feel like this is worth more than ~$11K total.

Sorry, something went wrong.

### Uh oh!

There was an error while loading. Please reload this page.

[![@NeilHanlon](https://avatars.githubusercontent.com/u/680198?s=80&v=4)](/NeilHanlon)

Copy link

### **[NeilHanlon](/NeilHanlon)** commented [Dec 18, 2025](/hackermondev/5e2cdc32849405fff6b46957747a2d28?permalink_comment_id=5911032#gistcomment-5911032)

Discord owes you 10x what they paid. Goodness.

Great work dude.

Sorry, something went wrong.

### Uh oh!

There was an error while loading. Please reload this page.

[![@ijsbol](https://avatars.githubusercontent.com/u/57773965?s=80&v=4)](/ijsbol)

Copy link

### **[ijsbol](/ijsbol)** commented [Dec 18, 2025](/hackermondev/5e2cdc32849405fff6b46957747a2d28?permalink_comment_id=5911050#gistcomment-5911050)

i have been waiting for this writeup for like 2 weeks finally

Sorry, something went wrong.

### Uh oh!

There was an error while loading. Please reload this page.

[![@IkariuPrime](https://avatars.githubusercontent.com/u/214130757?s=80&v=4)](/IkariuPrime)

Copy link

### **[IkariuPrime](/IkariuPrime)** commented [Dec 18, 2025](/hackermondev/5e2cdc32849405fff6b46957747a2d28?permalink_comment_id=5911085#gistcomment-5911085)

$11K for bugs that could have cost billions is crazy

Sorry, something went wrong.

### Uh oh!

There was an error while loading. Please reload this page.

[Sign up for free](/join?source=comment-gist)
**to join this conversation on GitHub**.
Already have an account?
[Sign in to comment](/login?return_to=https%3A%2F%2Fgist.github.com%2Fhackermondev%2F5e2cdc32849405fff6b46957747a2d28)

## Footer

© 2025 GitHub, Inc.

### Footer navigation

* [Terms](https://docs.github.com/site-policy/github-terms/github-terms-of-service)
* [Privacy](https://docs.github.com/site-policy/privacy-policies/github-privacy-statement)
* [Security](https://github.com/security)
* [Status](https://www.githubstatus.com/)
* [Community](https://github.community/)
* [Docs](https://docs.github.com/)
* [Contact](https://support.github.com?tags=dotcom-footer)
* Manage cookies
* Do not share my personal information

You can’t perform that action at this time.
