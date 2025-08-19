# Section 8: How We Are Tracked Online

The objective of this section is to learn the techniques used by ISP, coorporations, nation states and other agencies to track you online erode your privacy,
from cookies to supercookies, from HTTP ETags to web cache. The object of this sections is to understand how you are tracked and profiled online.

- [Types of Tracking](#types-of-tracking)
- [IP address](#ip-address)
- [3rd Party Connnections](#3rd-party-connnections)
- [HTTP Referer](#http-referer)
- [Cookies and Scripts](#cookies-and-scripts)
- [Super Cookies](#super-cookies)
- [Browser Fingerprinting and Browser Volunteered Information](#browser-fingerprinting-and-browser-volunteered-information)
- [Browser and Browser Functionality](#browser-and-browser-functionality)

---

## Types of Tracking

There are many ways you can be tracked online. Different entities track you in different ways and for different reasons.

- Everything you do in a **website**, in most cases, it will be logged, even if you don't log into.
- **3rd party sites**, those are linked out from a site you visit.
- **Ad Networks** like yahoo and google they track you if you seeing an ad provided by them, or any other type of ad network or ad provider.
- Your **E-mail Provider** will know everyone that you message, have access to all your emails, will be able to track websites you visit.
- **Internet Service Providers** will probably log the websites you visit and DNS queries you make
- **Telco - Cell Provider** will also track you like ISP, but will also log information about the geographic location or movements.
- **Governments - LEAs** cound track you specifically if you are a target
- **Work - University - School** will probably log the websites you visit, DNS queries, and may even log the content. Can even monitor https traffic by placing trusted certificates on your client, or simply run a monitor program on the client or machine.
- While using **Public/Shared Wi-Fi** you should assume that the sites you visit are logged by the Wi-Fi provider
- Any **Man-In-The-Middle** can potentially monitor you, encryption helps, but not a complete panacea
- Anyone with **Access to Machine** can monitor you. For example if it's at work or university or school they can have administration tools for monitoring. If it's hackers or someone malicious it's rats and key loggers can installed remotely and the same for a legitimate owner
- **Local Network** can monitor you
- **Radio - Wi-Fi / 3G / 4G** can be used to geographically locate you and possibly crack that communication to monitor you

**Encrypting** the traffic is usually a strong roadblock to monitoring and cracking, but there are always way around that encryption

---


## IP address

A basic way to identify you is by your IP address. It is registered in a address database which can reveal your city or area. The IP belongs to your router which is directly connected to the Internet.
The IP address dynamically changes and it's assigned when you connect to the Internet. Usually, is kept the same IP until the router disconnects.

Local IP address is different to the routers IP which is the actual IP address that other people will see when you are on the Internet.

People on the Internet, cannot directly connect to you. They connect to the router and then through a process called NAT, Network Address Translation, and then forwarded it to you.

```
netstat -an
```

can see all the various connections out of this machine.

---

## 3rd Party Connnections

**Web Tracking: 3rd Party Connections**

We can enable a proxy (Burp Suite) while connecting to a website, and see the actual request (HTTP request - text).

In the responses we get, we can see that we don't only connect to the target website but also additional websites like `googletagmanager.com`
and `google-analytics.com`

The browser interacts with those 3rd party sites and those might even redirect you to a 4th party etc

---

## HTTP Referer

A common method of tracking is the HTTP Referer, and it is an HTTP header.

It tells a webpage which is the origin website you visited before landing in the current page.

In the Burp Proxy we can see in a Request e.g.

```
Referer: http://www.forbes.com/
```

HTTP Referer is sent whenever you load content on a page from a third party. For example, if the web page includes an ad or a tracking script, the browser tells the advertiser or tracking network what page you are viewing.

Another method they use it's called convertion pixels or audience segmentation pixels, which are tiny one by one pixels, which are essensially invisible images that use HTTP referer to track you without appearing on a website.

HTTP referers are also used to track your emails, and if you email provider processes pictures automatically they can put in those little images pixels that reach out and connect to the relevant website with the HTTP referer, and therefore they've tracked your loading and use of that email.

---

## Cookies and Scripts

Another way of tracking is cookies and scripts

**Cookies**

Cookies have plenty of legitimate uses, for example the session cookie to remember who you are so you don't have to refresh on every page.
- It is persistent across all pages
- All cookies from the target website are consider first party cookies
- From another website are referred to as third party cookies
  - include third party advertisements
  - tracking and scripts
  - google analytics are very common
  - double click ads

If two websites contain the same tracking network e.g. double click, then the tracking network knows that you are visiting both e.g. bbc, forbes and any other site with double click cookies

Add-on: Cookies Manager

Cookies are also used to hijack your session, via e.g. MITM attack can relay that cookie in the HTTP header and login as that user. That is why encryption is important.


**Scripts**

Scripts on social network can also function as tracking scripts, e.g. facebook like button in another website, facebook knows you visited that website

Most websites contain google analytics scripts. So google will recording the websites you visit against your account (along with PII if you have provided or the SessionID which is tied to your browser)

---

## Super Cookies

Super cookies is a general term for methods designed to permanently track a user by placing within the browser something.

They are more difficult for the user to detect and remove

**Examples**

- Evercookie
- Local Shared Objects (Flash Cookies)
- Silverlight Isolated Storage
- Storing cookies in RGB values of auto-generated, force-cached PNGs using HTML5 Canvas tag to read pixels (cookies) back out
- Storing cookies in Web History
- Storing cookies in HTTP Etags
- Storing cookies in Web cache
- window.name caching
- Internet Explorer userData storage
- HTML5 Session Storage
- HTML5 Local Storage
- HTML5 Global Storage
- HTML5 Database Storage via SQLite
- HTML5 IndexedDB
- Java JNLP PersistenceService
- Java CVE-2013-0422 exploit (applet sandbox escaping)

You might clear your browser cookies and not your flash cookies, so the website will copy the value of the flash cookie to your browser cookie

Super Cookies are very resilient, cookies that are recreated after deleting from backup, stored outside the browser's dedicated cookie store are generally referred as **zombie cookie**

**Injected Super Cookies**

Everybody in the middle of a telco or HTTP request, like your ISP, they can insert cookies into your requests and into your responses and they can't be stripped out by the user.

These super cookies allow ad networks and media publishers to follow people across the internet. It allows the networks to build up profiles on users habits and pitch them targeted advertising, while the telcos take a cut.

**Book suggestion: The Rise of Mobile Tracking Headers: How Telcos Around the World Are threatening Your Privacy**

The only way to stop the header from reporting back is to limit your web browser to HTTPs sites only. With end-to-end encryption is impossible to inject anything into it without breaking the crypto. But over HTTP is easy to insert things into your traffic like in the Burp Proxy.

---

## Browser Fingerprinting and Browser Volunteered Information

When you visit a webpage, your browser voluntarily sends information about its configuration such as 
- the user agents
- the fonts that you use
- your browser types
- add-ons

If this combination of information is unique enough, it may be possible to identify and track you.

Ironically, plug-ins that you install to make you more secure, actually make you more unique

Tool: Panopticlick, test your browser how unique it is

The trick to **not** be fingerprinted is to be less unique, to be more default, to be more average

**Other information browser leaks out**

- IP Address
- DNS
- General location
- Browser / Browser type
- Accepting cookies
- Screen size
- Plugins
- Headers
- Mime Types
  
Tool: ipleak.net

But if you remove the ability of your browser to give out information, you're going to remove functionality from your browser

---

## Browser and Browser Functionality

Your browser functionality can also give information to track you. For example
- Geo location
- Security features
  - Safe Browsing
  - Phishing protection
- browser.send_pings
- WebRTC
  - can be used for tracking and IP discovery
- Extensions and plugins
  - WOTWeb of Trust
  - HTML5 Canvas Fingerprinting (Using JS)
- Browser history

They can find info that is more relevant to you or rather advertising to you

Tool: `www.browserleaks.com/geo`

**Security Features**

Browser have security features such as safe browsing and phishing protection. These beacon out every time you visit a website to check whether or not that site is known to have problems in relation to malware and phishing

This is good for security but is a privacy problem

---
