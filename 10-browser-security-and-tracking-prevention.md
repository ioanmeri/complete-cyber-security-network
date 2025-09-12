# Section 10: Browser Security and Tracking Prevention

- [Which Browser](#which-browser)
- [Reducing the Browser Attack Surface](#reducing-the-browser-attack-surface)
- [Browser Hacking Demo](#browser-hacking-demo)
- [Browser Isolation and Compartmentalization](#browser-isolation-and-compartmentalization)
- [Firefox Security, Privacy and Tracking](#firefox-security-privacy-and-tracking)
- [uBlock origin - HTTP Filters, ad and track blockers](#ublock-origin---http-filters-ad-and-track-blockers)
- [uMatrix - HTTP Filters, ad and track blockers](#umatrix---http-filters-ad-and-track-blockers)
- [Disconnect, Ghostery, Request policy - HTTP Filters, ad and track blockers](#disconnect-ghostery-request-policy---http-filters-ad-and-track-blockers)
- [ABP, Privacy badget, WOT - HTTP Filters, ad and track blockers](#abp-privacy-badget-wot---http-filters-ad-and-track-blockers)

How to better reduce the attack surface of your browser and harder it for maximum security and privacy.

How browsers are hacked and methods to mitigate all those attack vectors

---

## Which Browser 

**Browsers**
- Opera
  - less market share
- Safari
  - Apple's browser
- Internet Explorer
- Edge
- Chrome
  - first for quick updates / Safe browsing
  - business model that is relying on tracking users
    - it's a conflict of interest of how they do business and privacy
- Firefox
  - build in malware and phising, auto updates, extra security features with extensions

**Secure Browsers**
- Aviator
- SRWare Iron Browser
- JonDoFox
- Tor
- Epic Privacy Browser
- Comodo Browsers
  - Comodo Ice Dragon (Firefox)
  - Comodo Dragon (Chromium)
  - Comodo Chromium Secure (Chromium)

Debian: Iceweasel Browser / Fork of Firefox 
- Mozilla Firefox Linux binaries contain copies of all Linux binaries
- Iceweasel uses the sytem libraries instead
  - updates to Iceweasel are done using the Debian method of updating the system
  - `apt-get update && apt-get upgrade`

---

## Reducing the Browser Attack Surface

- Disable, remove or uninstall anything that you don't use
  - especially those that use the internet: browsers, browsers plugins and extensions
- Disable or limit Java, Flash, Silverlight
- Disable or limit Javascript, browser applications e.g. PDF reader
  - JS is needed but can be a security problem e.g. malvertising using JS
  - Plugin "QuickJava" to enable disable JS / Java /Flash but not Flash
  - If anonymity is a priority, JS has to be disabled

---

## Browser Hacking Demo

To attack the browser, I need to get something to run in this browser.

Example: Use JavaScript to send my payload

- Send an email with JS embedded in the email, if your email client runs scripts, it will be executed
- Cross site scripting vulnerabilites for sites that you are using, embed scripts in legitimate sites
- Buy an ad from an ad network and embed the script
- Send a link with a script that is running in the website to visit
  - e.g. using BeEF in Kali linux and be able to see the connected browser
    - can use the command "Fake Notification Bar"
    - "Pretty Theft" command to steal credentials
    - "Google Phishing"
  - Reserve shell and run commands

Once even the smallest amount of script is run within your browser, you can do all of those social engineering attacks,
and then if you're not patched or there is exploits available, or zero-day exploits, you can create reverse shells.

---

## Browser Isolation and Compartmentalization

One of the best methods to protect against hacking and tracking is isolation and compartmentalization

- Consider the sandboxing and running the browser in a virtual machine
- if you can use a VM and a sandbox, will provide you isolation from your browser and OS
- cloud browsers will protect you from hackers and malware but they are a privacy concern
- "Browser in the Box" application that running in a VM
- Firefox profiles - completely separate configuration / addons for profiles e.g. testing extension
- Firefox contextual identities
- Priv8 / Multifox addons - manage tabs on sandboxes for increased security

If it really matters to you to not have cross-contamination between contextual identities, you need physical isolation

---

## Firefox Security, Privacy and Tracking

- Firefox has - Do not track - feature
  - adds a `DNT: 1` flag in HTTP headers - voluntarily policy
- Firefox has - Use Tracking Protection in Private Windows
  -  option (disconnect.me): will block requests to tracking sites
- Firefox has noscript extension
- Chrome has better sandboxing
- Google is the biggest coorporate tracker of all Internet users

Testing tool: `itisatrap.org`

- Firefox has "Never remember history" option
  - will prevent much of the tracking e.g will not store local shared objects / flash cookies
  - it will also stop sites from remembering preferences e.g. language
- Firefox has Private Browsing feature
  - means "Never remember history" for that window

**Settings options**

- Warn me when sites try to install add-ons
- Block reported attack sites
- Block reported web forgeries (service provided by Google: `safebrowsing.google.com`)
- Remember logins (use alternative password managers)
- Master password (use alternatives)

Look browsers connections:

```
about:networking
```

---

## uBlock origin - HTTP Filters, ad and track blockers

Methods to block undesirable malware, ads, malvertisement, phishing links and tracking methods with 
- HTTP filtering
  - also speed up your browser
- Script blocking

**Security extensions for Firefox**
- uBlock Origin
  - open source
  - `about:memory` to check memory usage
  - can block e.g. images / 3rd-party / inline-scripts / 2mdn.net domain etc globally or per site
  - provides documentation for easy / medium / hard mode

---

## uMatrix - HTTP Filters, ad and track blockers

- for more technical users, overlapping functionality with uBlock origin
- point and click matrix based firewall with many privacy enhancing tools
- Options for
  - User agent spoofing
  - Referrer spoofing
    - firefox won't tell sites, what browser came from
  - Script HTTPS
- Scope for
  - cookie, css, image, plugin, script, XHR, frame, other
- settings to delete blocked cookies, delete non-blocked session cookies, host files etc

---

## Disconnect, Ghostery, Request policy - HTTP Filters, ad and track blockers

Disconnect is a private browser, similar to uBlock origin, open source and free for non technical users
- only blocks what it considers privacy invasive

Ghostery is similar to disconnect and uBlock origin. Was bought by an ad company. 
They sell user data without any private information.

Request policy is a Firefox addon that blocks all cross-site requests unless you allow them.
This prevents unwanted connections to 3rd parties to protect you against XSS.

---

## ABP, Privacy badget, WOT - HTTP Filters, ad and track blockers

Add Block Plus is a very popular ad blocker which shows acceptable ads by default, more like an install and let it work type of plugin for non technical users (EasyList is used in uBlock anyway)

Privacy Badget is another HTTP filter to block adverts and malware, developed by EFF. Developed by the need for an extension to automatically analyze and block any traffic or ad that violated the principle of user consent, which would work well without settings or knowledge by the user. Primary a privacy tool not an ad blocker.

WOT Web of Trust plugin informs you about what websites are trusted and untrusted based on community distribution

---













