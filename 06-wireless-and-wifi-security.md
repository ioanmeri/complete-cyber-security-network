# Section 6: Wireless and Wi-Fi Security

- [Wi-Fi Weaknesses - WEP](#wi-fi-weaknesses---wep)
- [Wi-Fi Weaknesses - WPA, WPA2, TKIP and CCMP](#wi-fi-weaknesses---wpa-wpa2-tkip-and-ccmp)
- [Wi-Fi Weaknesses - Wi-Fi Protected Setup WPS, Evil Twin and Rouge AP](#wi-fi-weaknesses---wi-fi-protected-setup-wps-evil-twin-and-rouge-ap)

---

**Scope**

- Understand possible attack vectors on a wireless network
- Weaknesses in wireless network
- Best architecture and configuration for maximum wireless network

---

## Wi-Fi Weaknesses - WEP

also known as IEEE802.11, password can be cracked within seconds

**Deficiencies**

1. Use of static encryption keys - No session keys, weak RC4 used, same password
2. Lack of packet integrity assurance - Bits can be changed
3. The ineffective use of initialization vectors (IVs) - IVs not random enough, only 24-bits, IVs with the same key, chrackable in seconds


---

## Wi-Fi Weaknesses - WPA, WPA2, TKIP and CCMP

- WPA introduced in 2003 uses the TKIP (Temporal Key Integrity Protocol) protocol
- RC4 stream cypher is used with 128 bits per packet key
  - dynamically generates a unique key for each packet
  - WEP key + IV value + MAC address = New encryption key
- TKIP is susceptible to packet injection attacks
  - avoid it if you can
 
the full **WPA2** IEEE 802.1 I came out officially in 2003
- support fo CCMP (Counter Mode Cipher Block Chaining Message Authentication Code Protocol)
  - intented to replace TKIP encryption protocol
  - mandatory for Wi-Fi certified devices in 2006
- always choose AES (CCMP) encryption algorithm
  - CCMP is an AES based encryption

**WPA2-Personal**

Security mode WPA2 Personal with pre shared key, is designed for home and small business network and
it doesn't require authentication

**WPA2-Enterprise**

The other option for access control method (WPA-802.1 X mod)
- RADIUS Authentication Server
- Extensible Authentication Protocol (EAP)

WPA/WPA2 Handshake are susceptible to dictionary and brute force password guessing of the pre-shared key

Pre-shared key needs to be complex and long with special characters

**Cowpatty** tool available in kali used for this attack

**Weakness in WPA and WPA2 in salt**

both use the SSID in salt but should be a random value that is added to the encryption process to add more complexity
- hackers can pre compute common passwords against or with common SSIDs to make password hacking faster
- rainbow tables
- Church of Wifi WPA-PSK Lookup Tables
  - 1 million common passwords for 1000 common SSIDs
- default or common SSIDs should not be used because of this attack

---

## Wi-Fi Weaknesses - Wi-Fi Protected Setup WPS, Evil Twin and Rouge AP


**WPS**

- The symbol of WPS protected Wi-Fi looks like recycle and has an 8-Digit WPS PIN
- can be brute forced within hours
  - then can get WPA, WPA2, preshared key, password, access to the network
  - revel tool in kali
- must be turned off

**Evil Twin**

- attacker setup access point with SSID the same as a real Wi-Fi network
  - effectivily a man in the middle attack
  - can do SSL stripping, browser attacks
- pinapple hardware can be bought

Nation state and agencies have exploits against the core components of Wi-Fi and wireless technology
- Wi-Fi Drivers
- Protocol stack DHCP
- WPA / AES that community is not aware of
- NIGHTSTAND Hardware
- Drones are equipped with Wi-Fi crackers attached to them

**Rouge AP**
- if you are connected to an untrusted access point, you need to take more precautions
- they are a man in the middle
- it's called Rouge AP
- can strip the SSL
- can see all of the things you are doing, sites you are visiting
- VPNs are required

---
