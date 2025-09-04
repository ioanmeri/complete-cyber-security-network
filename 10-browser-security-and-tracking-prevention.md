# Section 10: Browser Security and Tracking Prevention

- [Which Browser](#which-browser)
- [Reducing the Browser Attack Surface](#reducing-the-browser-attack-surface)
- [Browser Hacking Demo](#browser-hacking-demo)

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



