# Section 6: Wireless and Wi-Fi Security

- [Wi-Fi Weaknesses - WEP](#wi-fi-weaknesses---wep)
- [Wi-Fi Weaknesses - WPA, WPA2, TKIP and CCMP](#wi-fi-weaknesses---wpa-wpa2-tkip-and-ccmp)
- [Wi-Fi Weaknesses - Wi-Fi Protected Setup WPS, Evil Twin and Rouge AP](#wi-fi-weaknesses---wi-fi-protected-setup-wps-evil-twin-and-rouge-ap)
- [Wi-Fi Security Testing](#wi-fi-security-testing)
- [Wireless Security - Secure Configuration and Network Isolation](#wireless-security---secure-configuration-and-network-isolation)

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

## Wi-Fi Security Testing

With Kali Linux, a compatible USB network adapter or doggle network device is needed to be able to put it into monitor mode and to do packet injection

- Atheros AR9271
- Ralink RT3070
- Ralink RT3572
- Realtek 8187L
- Alfa AWUS036NHA


**Kali Pre-shared key cracking program**

```
aircrack-ng | more
```

**Implementation of an offline dictionary attack against WPA / WPA2 / Personal**

```
cowpatty
```

**Brute force attack against Wifi protected setup or WPS**

```
reaver
```
in combination with aircrack to know MAC address

also 
- Fern WIFI Cracker
  - to recover WEP, WPA, WPS keys
- securitystartshere.org


---

## Wireless Security - Secure Configuration and Network Isolation

Wireless networks should be considered less trusted than psysical networks, those both should be isolated from each other where possible.
Traffic from wireless devices, ideally should not be able to reach your wired devices

**Example**

SSID 1: Untrusted guest Wi-Fi
- 192.168.4.0/24

SSID 2: Semi-Trusted Wi-FI
- 192.168.5.0/24

these two networks are running from the same Wireless Access Point

They are separate Wi-Fi networks even though they are connected to the same access point. They would have
- different passwords
- traffic could be allowed to go inbound (e.g. administration to untrusted device, best to isolate it)

**Custom firmware**

To facilitate the ability to be able to add equipment, consider the installation of custom firmware.

**Best practices**

- switch off Wi-Fi in not used
- change default SSID to something random / or not the default
- WPA2-Enterprise (WPA-802. IX mode)
- RADIUS Authentication Server
- Extensible Authentication Protocol (EAP)
- WPA2-Personal (WPA-PSK Pre-shared key)
- CCMP/AES Encryption
- CBC-MAC
- Never use WEP or WPA
- Avoid TKIP
- Use complex passwords
- Always encrypt Network Traffic when possible (TLS, VPNs)
- Disable WPS!
- arpwatch (ARP spoffing protection)
- software to discover rogue or evil twin wireless access point
- change default password
- close admin interfaces you don't use
- turn off Wi-Fi if no used

**More optional**

- Disable Wireless SSID Broadcast
  - any wifi scanner software will be able to see it
- Consider implementating IEEE 802.1X
- Set max associated clients to the devices you have
- RF Isolation and reduction methods

 ---







