# Section 11: Passwords and Authentication Methods

- [Goals and Learning Objectives](#goals-and-learning-objectives)
- [Passwords](#passwords)
- [How Passwords are Cracked - Hashes](#how-passwords-are-cracked---hashes)
- [How Passwords are Cracked - Hashcat](#how-passwords-are-cracked---hashcat)

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

