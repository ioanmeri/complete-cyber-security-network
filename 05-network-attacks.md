# 5. Network Attacks, Architecture and Isolation

- [Network Attacks and Network Isolation](#network-attacks-and-network-isolation)
- [Arp Spoofing and Switches](#arp-spoofing-and-switches)
- [Effective Network Isolation](#effective-network-isolation)
- [Software to protect against attacks](#software-to-protect-against-attacks)

---

## Network Attacks and Network Isolation

Network isolation is physically or logically putting devices on separate networks and restricting how or if devices can communicate with each other.

**Threats related to such isolation**

- attacker / malware on your network they can attempt to propagate an attack onto the rest of the network
- untrusted devices / guests may be infected and can be used to propagate attacks
- IoT are not well maintained generally

network isolation will help mitigate those attacks

Once an attacker is on the network they can
- local traffic sniffing or snooping
- MITM attacks / SSL striping / insert malicious code via HTTP
- attacking devices directly via open services
  - network share
  - password protected (smart TV with vulnerable SSH)

---

## Arp Spoofing and Switches

### Switch MAC Table

Switches are where the network cables are plugged in.

Switches keep a table of ethernet MAC addresses

| MAC Adress | Port |
| --- | --- |
| 00:1C:14:80:59:02 | 3 |
| 00:16:3E:C3:EC:C5 | 2 |
| 00:16:3E:58:C4:4C | 1 |

and uses these unique MAC addresses for your devices to send traffic to it's destination and works on the Data Link Layer 2

- Inside the network, IP addresses are not used anymore, instead MAC addresses are used for traffic to find it's destination
- IP addresses are only used on the Internet when you have a routing device

When you have a switch or a hub then you only using MAC addresses. Swtiches offer more security over hubs, because switches have **Isolated Collision Domains**. Meaning, you can't sniff the traffic on the network with a switch, because traffic only gets forwarded to the correct physical LAN port based on MAC address.

When traffic goes into the switch, or the router, the switch knows what the MAC address is, so instead of sending that data to all the devices, it is just send it to that one physical port and down that wire. For everyone else who is plugged in to that switch, won't receive that data (Isslated collision domain)

---

### ARP attack

On a Ethernet network, connected via switch, it is relatively easy to perform MITM attacks.

An attacker fools other devices on the network into believing that the attacker is the default gateway or the router by abusing the Address Resolution Protocol (ARP)

The attacker can then observe, record, inject into and manipulate traffic.

In a Wi-Fi network, traffic can also be manipulated in this way.

If you have a hub instead of switch, you can observer all the traffic anyway, but for injection manipulate you will need ARP spoofing

---

### Address Resolution Protocol

This is IP to MAC resolution, it works at the Data link layer

It resolves the network layer IP address into Data link layer MAC address

```
sudo arp -a
```

If a device doesn't have an entry in it's ARP table to connect to a given IP address, then it needs to issue a ARP request to resolve the IP to MAC and gain the MAC address

- ARP is used to find out the MAC address
- ➡️ ARP broadcast a frame requesting the MAC address for the IP it has
- ➡️ The device with the correct IP replies this correct MAC address
- ➡️ This is then added to the ARP table cache

With tools like `arpspoof`, `ethercap` (kali), `cain and able` (windows) an attacker in a network could fool the devices on a network that they are the correct MAC address for the router's IP address, by simply responding to say me I am the router. This is ARP spoofing, ARP poisoning

This is possible because ARP doesn't have authentication, anyone can say they are anyone. It was developed in time when devices on a local network where all trusted

There is absolutely zero security around it. When the ARP spoofing it's been done, an attacker can see all the traffic of the victim, instead of the router and can perform manipulation, injection, SSL striping etc

The attacker needs to be internal


More: https://www.irongeek.com/

---

## Effective Network Isolation

First thing is to have separate routable networks for different devices of different level of trust

![Network Isolation](/assets/images/01.png)

- DMZ
  - this is a network used in a business implementation as well
  - for devices that need port forwarding
    - inbound connections from the Internet
    - more at risk, e.g. web server / IoT device / CCTV or web cam
  - e.g. connect it directly via an ethernet cable psychically into your router / firewall and assign it it's own network
    - devices on the DMZ would be blocked from making outbound connections to the other internal networks / Internet
    - attacker connecting inwards would not be able to make a connection outwards
  - can be implmented with firewall rules
    - IP tables, NAT
    - Routing, making this firewall not rootable to other networks
  - Trusted VLAN device can make a connection to a DMZ device for administration
  - needs a router and seperate ports
- VLAN
  - is the logical seperation of a network instead of psysical
  - have a tag in each frame for which VLAN it is
    - tagging mechanisms ISL or 802.1q
  - work at layer 2 / Data Link layer (same as Mac) / sometimes layer 3
  - devices can be placed on separate VLAN
    - a MITM attack can only be performed within this VLAN
    - ARP spoffing would be limited in scope to this VLAN
    - default gateway for each VLAN would be a different IP
  - each VLAN have no idea, other networks exist or their IP addresses
  - untrusted VLAN would not be able to connect outbound to the rest of the network
  - we can even have separate VLANs for each device
    - but a technique called VLAN hoping has been effective to go from one VLAN to another
    - CAM flooding, STP, hot standby route of protocol HSRT
    - Yersinia (Kali) ➡️ VLAN hoping
    - VLAN hoping but has been patched
  - easier than DMZ, simply in the switch and / or router firewall configure VLAN 1, VLAN 2


---

## Software to protect against attacks

Netcut, sniffdet, xarp, arpwatch (available in pfSense)

- Prefer static, dynamic IPs can be spoofed

Advanced options: Catalyst (Option: Port Security), IEEE 802.1AE, IEEE 802.1X (devices need to authenticate to connect to network), DHCP Snooping, VPNs on local network to connect to local devices, pfSense and dd-wrt support VLANs and IPtables and switches support VLANs

For Wi-Fi a router is needed with AP isolation and support multiple SSIDs on the same Access Point

---
