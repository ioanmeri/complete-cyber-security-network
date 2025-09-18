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
- [No-script - HTTP Filters, ad and track blockers](#no-script---http-filters-ad-and-track-blockers)
- [Policeman and others - HTTP Filters, ad and track blockers](#policeman-and-others---http-filters-ad-and-track-blockers)
- [History, Cookies and Super cookies](#history-cookies-and-super-cookies)

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

## No-script - HTTP Filters, ad and track blockers

**NoScript Security Suite**

NoScript FireFox extension allows active content to run only from sites that you trust
- protects you against XSS
- clickjacking attacks
- by default blocks all scripts from all domains

allows you to lock down what scripts can do and where.

Main extension used by the Tor Browser to restrict the functionality of the browser

- Can also disable Java / Flash / Silverlight / Audio / Video / Frames / Fonts
- XSS protection
- Enable Automatic Secure Cookies Management
- Forse HTTPS 

The problem is, most sites to work correctly need JavaScript (active content which is disable by default)
- should only be used when you don't want to be tracked

If you want high levels of security and privacy, you have to block scripts and at the same time accept that most sites you visit
won't work correctly.

Better middleground is the uBlock origin and uMatrix with balance between security and usability

NoScript is best for a browser profile that you want to completely lockdown

---

## Policeman and others - HTTP Filters, ad and track blockers

Policeman addon has a purpose similar to request policy and no script
- it supports rules based on content type
  - e.g. allow img, styles but not scripts / frames

can write advanced rules and also act as a black list

Tool: virustotal.com site to check suspicious websites

Company offering tracking protection: trackoff.com

Addblocker for iOS: Purify Blocker

---

## History, Cookies and Super cookies

All of these can be used to track you online, affect your privacy and can be used as evidence if there is a local forencic examination

Even if you set "Never remember history", the browser plugins and extensions will still retain some information

**List of objects that can be used to track you**

- History
- Searches
- Cookies
- Temporary Files
- Downloads
- Searchbox and findbox text
- Bookmarks
- HTTP auth, SSL state
- OCSP state
- Site-specific content preferences (including HSTS state)
- Content and image cache
- Offline cache
- Offline storage
- Super Cookies
- Crypto tokens
- DOM storage
- The safe crowsing key
- Google wifi geolocation token
- Plugin and extensions data e.g. NoScript's site and temprary permissions, and all other browser site permissions. uBlock origin and uMatrix settings etc.

Full cache: `about:cache`

**Best practices**

- use a non-persistent OS
  - tails, nopix, live, debian, VN snapshot
  - no data is ever permanently stored
- when you accept cookies from a website, that site can track you through cookies
  - clear cookies
- can clear history // only superficial
- Evidence elimination tools: CC cleaner, BleachBit
  - add extra signatures to the cleaners, 2000+ applications evidence

**Mitigation Add-ons**
- Better privacy: extension to find and delete super cookies, local shared objects etc
- Click & Clean: browser history, erase temporary history files, cookies, flash LSOs etc
- QuickJava: enable / disable Java, JS, cookies, images, animations, styles, proxies
- Advanced Cookie Manager: cookie manager with advanced features e.g. export inport
- Self-Destructing Cookies: deletes cookies and local storage
- Decentraleyes: CDNs can track you and this plugin protects you against centralised content
  - works well with uBlock origin and uMatrix

**Mitigation Tools**
 - Hidden Operating System from veracrypt against local forensic examination of history
 - Portable Firefox in encrypted hidden containers
 - JonDoFox portable firefox
 - Tor Browser

Tool to create super-cookie: `samy.pl/evercookie`, stores values in:
- userData
- cookieData
- localData
- sessionData
- pngData
- etagData
- cacheData
- lsoData
- slData

---







