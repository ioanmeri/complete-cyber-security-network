# Section 11: Passwords and Authentication Methods

- [Goals and Learning Objectives](#goals-and-learning-objectives)
- [Passwords](#passwords)
- [How Passwords are Cracked - Hashes](#how-passwords-are-cracked---hashes)
- [How Passwords are Cracked - Hashcat](#how-passwords-are-cracked---hashcat)
- [Operating System Passwords](#operating-system-passwords)
- [Password Managers](#password-managers)
  - [Master Password](#master-password)
  - [KeePass, KeePassX and KeyPassXC](#keepass-keepassx-and-keypassxc)
  - [LastPass](#lastpass)
  - [Hardening Lastpass](#hardening-lastpass)
  - [Creating a Strong Password That You Can Remember](#creating-a-strong-password-that-you-can-remember)
- Multi-Factor Authentication
  - [Soft Tokens - Google Authenticator and Authy](#soft-tokens---google-authenticator-and-authy)
  - [Hard Tokens - 2FA Dongles](#hard-tokens---2fa-dongles)
  - [Choosing a Method of Multi-Factor Authentication](#choosing-a-method-of-multi-factor-authentication)
  - [Strengths and Weaknesses](#strengths-and-weaknesses)
  - [The Future of Passwords and Authentication](#the-future-of-passwords-and-authentication)
 
---

## Goals and Learning Objectives

How to use methods of authentication, including:

- Passwords
- MFA
  - Soft tokens
  - Hard tokens
- Use of Password managers
- How passwords are cracked
- How to mitigate the cracking through the use of better hashing
- Key stressing technology
- Choice of good passwords
- Future of passwords and what that might look like

---

## Passwords

Passwords are one of the weakest links in security
- Need to be using different and strong passwords
- most probable scenario of password leak is from a website hacked
- phishing attacks are another very common method of compromising your passwords
- password could be captured in transit if not encrypted
- stolen by a malware / keyloggers / spyware / shoulder surfing / email account

Tool: haveibeenpwned.com

Search to see if any of your previous or current passwords associated with any of your email addresses or usernames have been compromised

A password is as secure, as the least secure place it is being used

---

## How Passwords are Cracked - Hashes

**3 ways to crack passwords**

- Dictionary attacks
  - common passwords from words lists
- Brute force attacks
  - try every single password possibility
- Hybrid attacks
  - psychology of human behavior combined with a dictionary

Tools:

- hashcat.net
  - combinator attacks
  - rule-based attacks
- thesprawl.org
  - analyze password patterns
- Leet
- Markov chain
- hydra kali linux

Password attack can be:
- Online
  - hacker interacts with the system
  - e.g. a dictionary attack in a network based service
  - extremely slow because of the response of the crypto system (e.g. 1 sec per second)
  - can block the IP of attacker once a certain number of attempts have been made / can inform user, admin
- Offline
  - Attackers have access to the password hashes
  - No interaction with the crypto system
  - cannot use rainbow tables 

Tools:
- sha1-online.com
- PBKDF2 hash generator

Use of key streching / derivation functions results in better cryptographic results
- passwords will take too long to crack even if someone steals the hashes

**Other way to strengthen the passwords hashes**
 - Use another secret key to encrypt the hash
 - Include the secret key in the hash using a keyed hash algorithm

Ideally the second key should be in another location separate from when you store the first key

Best way: Hardware security module

---

## How Passwords are Cracked - Hashcat

Password hashes can be stolen by:
- SQL attacks
- extracting from OS
  - live mount CD
  - pwdump7 tool
 
Can be much faster than online attacks: 100 B guesses / second

Kali linux tool to analyze hash: `hashid`:

- Can use a dictionary attack
- `hashcat -m 0 hashes.txt wordlist.txt -o crackedhashes.txt` - MD5
- `hashcat -m 0 hashes.txt wordlist.txt -o crackedhashes.txt -a 1` - 1 for combination attack

Rules: hashcat.net

Online cracking tools:
- raymond.cc

If the right hash, key derivation function, key length and password has been chosen then it's unlikely to be cracked even with all the power.

If you are a MITM, instead of cracking it you just forward it on after you have captured it.
- NTLM hashes can be relayed with metasploit

---

## Operating System Passwords

- OS passwords provides zero security against someone with physical access to the device
- password can be bypassed simply by booting your device with another OS
  - live CD, connect hard drive to another machine
- the hash can be simply replaced creating a new working password
  - with linux you boot it up
  - edit the group parameters from recovery mode to a bash shell
  - remount the root partition
  - use the passwd command to change the password
- to slow down the process you can add 2FA login, YubiKey
- what does work is whole disk encryption
  - password is used as a key

---

## Password Managers

Password managers are secure stores for sensitive data
- username
- password
- credit card data
- PIN numbers
- access codes

and enable you to have strong and unique passwords

- you choose a random generated password
- is locked using a master password (you need to remember / must be very strong)

**Categories of Password Managers**
- Local
- Cloud
  - can share the information between devices

---

## Master Password

Tool: masterpasswordapp.com

Stateless password generator
- passwords are generated on demand
  - from name, site, master password
  - autogenerates passwords
- No need to do backups, internet access in not needed
  - lowest attack surface / stateless ➡️ don't even need to store the passwords
  - simply nothing stored
- Cannot use usernames, form filling, secure notes, credit cards

---

## KeePass, KeePassX and KeyPassXC

Recommendation for Local Password Manager for Linux and Mac
- Keypass
- KeypassX
- KeypassXC

You passwords are in a local encrypted database
- need a master password (very strong) in order to decrypt the database
- 2FA login option
  - supports yubikey authentication
- autogenerates passwords with settings
- uses AES 128-bit using 256-bit key
- has plugins e.g. cloud syncing
- should do separate backups apart from the syncing

A malware keylogger could compromise the master password
- can install it in a separate VM
- store DB in encrypted USB as extra layer of isolation

---

## LastPass

Recommended Cloud-based password manager

- has the greatest attack surface
  - integrates with the web browser and syncs across devices using LastPass servers
- encrypted passwords and data are stored in LastPass servers
- can use credit cards, emails, usernames, pins
- cross platform including portable browsers
- can auto change password for auto login sites
- can auto login / auto populate
- last pass cannot find your master password, because data is encrypted client side
  - zero knowledge system
- PBKDF2 SHA-1 / 5000 password iterations
- supports 2FA

LastPass and any tool that integrates with the browser are simply more vulnerable to attack:

- Phising were you enter the Master password
- Attacker can compromise LastPass servers
- MITM can defeat 2FA
- Attacker in your device using keylogger, master password can be compromised

---

## Hardening Lastpass

- better to use the binary / plugin version instead of the web ➡️ safer
- never enable remember password option
  - if you store it it's an attack vector
- don't use a password reminder / remember it yourself / write the password down
- Re-prompt for Master Password for e.g. Access a Secure Note
- Use a security email for critical account notifications
- Add Access restriction for countries
- Disallow Tor logins
- Password Iterations: 10000 - 20000
- use MFA to mitigate password cracking
- Disable password recovery

---

## Creating a Strong Password That You Can Remember

You should use a password manager to generate and store your password
- long passwords that you don't need to remember and cannot be reused
- minimum 12 characters, recommended 43+ randomly generated

Very sensitive data
- 128-bit encryption use 22 character passwords + (Not future proof)
- 256-bit encryption use 43 character passwords + (Good)
- 512-bit encryption use 86 characters passwords + (Very good but slower)

Properties to consider when creating a password
- The difficulty to crack
- The difficulty to remember
- The difficulty to type

The difficulty to crack a password is based on each entropy and randomness away from human patterns

What you should **AVOID** using in passwords (Especially with passwords <= 20 characters)
- Patterns
- Avoid combinations or words or common patterns
- Avoid common phrases (patterns)
- Avoid dates, names (patterns)
- Avoid common pattern combinations
- Capital letters at the beginning
- Lower-case letters in the middle
- Symbols and numbers at the end
- l33t speak

What to **DO** to create strong passwords that are memorable and strong
- use pass phrases or sentences - increases length, entropy and randomness
- foreign words are better because they are not in dictionaries
- add paddings e.g. 11111111111 at the end - doesn't matter the pattern because of the length
  - e.g. Bruce L33 - "Be like water."Amazon.com
- better to create long passwords that you can remember than keep changing passwords
- don't tell anyone your passwords or padding, or don't right down unless they are locked
- don't use security questions
- can split up the password in a password manager
- change usernames as passwords like admin or root
- prioritize the accounts that are most important and have multi factor authentication
- setup notifications and alerts when account is accessed or funds are transfered
- keep a backup of the password manager database

---

## Soft Tokens - Google Authenticator and Authy

Multi-factor authentication is a method of assigning access to a system based on presenting more than one piece of evidence to an authenticating system
- Card - Chip + Pin number

**3 types of possible authentication factors**

- Something you **know** - Authenticatin by **knowledge**.
  - e.g. Passwords, PINs, passphrases etc
- Something you **have** - Authentication by **ownership**
  - e.g. ID badge, smart card, credit card,
  - soft token like google authenticator
  - hardware token like yubikey or nitrokey
  - an SMS or email sent to your phone
  - a phone call verification et.c
- Something a **person is** - Authentication by **characteristic**
  - Retine scan
  - Thumb scan
  - Biometrics
  - The iphone thumb scanner etc.

> Multifactor authentication / Two-factor authentication (2FA) or 2 step verification

**Time-based One-time Password (TOTP) soft token tools**

- Google Authenticator which supports OATH OTP
  - also called Disconnected token
- Authy available in phones and desktop
  - uses the cloud but they are encrypted
  - recovery method determines the strength of the authentication
  - you must have multiple devices with your second token on or you could forever be locked out of your accounts

**OTP soft tokens**
- Android
  - Google Authenticator
  - Duo Mobile
  - Authy
  - Authenticator Plus
- iOS
  - Google Authenticator
  - Duo Mobile
  - HDE OTP Generator
  - Authy
- Windows Phone
  - Authenticator
- BlackBerry
  - 2 Steps Authenticator
  - Authomator

You can also have
- SMS
- email sent to your phone

as a method of authentication

---

## Hard Tokens - 2FA Dongles

**Hard Tokens Manufacturers / Two Factor Auth (2FA) Dongles**

- DigiFlak
- Nitrokey
- VASCO Data Security
- WWPass
- Yubico

also implementing OTP but in hardware.

they support the open authentication standard: Universal 2nd Factor (U2F) / hard token or connected token

Businesses use RSA servers for remote access into the office

---

## Choosing a Method of Multi-Factor Authentication

Tools: twofactorauth.org, dongleauth.info

Depends on each website or application since they have their own MFA methods:

- SMS
- Phone call
- Email
- Hardware token
- Software token

can use yubikey-luks for hard disk encryption, also Yubico can work with veracrypt 

---

## Strengths and Weaknesses

**Strengths**

An attacker needs access to a second or even third factor and not just a single password
- increases the difficulty

**Weaknesses**

- An attacker may access another factor by hacking a phone
- An attacker may hack the phone network to receive the SMS
  - SMS should be seem as inferior but better than nothing
- Hard tokens may have malware on it
- MFA still vulnerable in phising, MITM attack even using OTP
  - a MITM can receive the OTP and rely them to the destination

---

## The Future of Passwords and Authentication

Passwords will slowly fade away and be replaced

SQRL is one possible future for passwords 
- Scan it and login to the system

CLEF is another possibility
  
---


