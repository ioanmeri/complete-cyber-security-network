# The Complete Cyber Security Course: Network Security

| Layer | No. | Org | Transmition | Devices | Protocols |
| --- | --- | ---|---| ---| ---|
| Internet | 3 | IP | Packets | Router Brouter | IP, IPSec, ICMP, IGMP, RIP, OSPF, BGP, IPX, SKIP, SWIPE, NAT, IGRP, EIGRP, BOOTP, DHCP, ISIS, ZIP, DDP |
| Network access | 2 | IEEE 802 | Data Link LLC MAC | Frames | Switch, Bridge, NIC | Ethernet, 802.3, Token Rin5 802.5, ATM, FDDI, 802.4, Wi-Fi 802.11, PPP, L2TP, SLIP, ARP, RARP, 802.1AE MACSec, HDLC |
| Network access | 1 | Physical | Bits | Repeater, Hub, NIC, Cables, MAU, Multiplexer | ISDN, DSL, SONET, 10BASE-T, 10BASE2, 10BASES, 100BASE-TX, 100BASE-FX, 100BASE-T, 1000BASE-T, 1000BASE-SX, EIA-x, RS-x |

---

# 2. Routers - Port and Vulnerability scanning

To find the IP of your router, you need to know the default gateway on your devices

```
sudo route -n
```

You access a device via a port

The router will have an external IP address (194.187.250.204 - ISP assigned) and it also has an internal IP address (192.168.1.1)

In almost all cases, nonone will be able to make direct connection to your Internet devices via the Internet. This is because of a service called NAT

## NAT - Network Address Translation

Because of NAT, you'd have to specifically configure within the router to forward traffic to a specific device on a specific port, in order for Internet devices to connect to your internal devices

This is often called port forwarding or DMZ

They can communicate back to you because the connection maintain state, as long you have initiate the connection

NAT operates as deny all rule to any attempt to connect to internal devices unless port forwarding is specifically enabled to allow access


## Router

What we call router these days is technically several devices together in one device, although you can have those devices seperate:
- Modem
  - modulate and demodulate a signal so it can be passed on to the local loop of your network carrier
  - works at the physical layer 1, sends photons and electrons on to the Internet
- Router
  - routes traffic from one network to another based on the IP routing table within the router
  - LAN to LAN or LAN to Internet
  - works at Layer 3 and routes IP packages between networks
  - something is a different network when it has a different subnet
- Firewall
  - used to allow and deny traffic based on a set of tools
  - IPTables
  - Works in Transport / Network layers 4 / 3
  - accept and reject traffic based on port protocol and IP address
- Switch
  - where network cables are plugged in
  - keeps a MAC table
  - works on Data Link layer 2
  - has issolated collision domains (cannot sniff)
- Wireless Access Point
  - works similar to the switch but for wireless devices
  - uses the unique MAC addresses of your devices to send traffic
  - works at Data Link layer 2
- (optional) Bridge
    - forwards packets and filters based on MAC address
    - forwards broadcast traffic but not collisions
    - works at data link layer 2

Routers also offer services like 
- DHCP which assigns your IP addresses to your devices
- DNS to resolve domain names into IP addresses
- Port forwarding
- NAT

---

# 5. Network Attacks

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

Inside the network, IP addresses are not used anymore, instead MAC addresses are used for traffic to find it's destination

IP addresses are only used on the Internet when you have a routing device

When you have a switch or a hub then you only using MAC addresses. Swtiches offer more security over hubs, because switches have Isolated Collision Domains. Meaning, you can't sniff the traffic on the network with a switch, because traffic only gets forwarded to the correct physical LAN port based on MAC address.

When traffic goes into the switch, or the router, the switch knows what the MAC address is, so instead of sending that data to all the devices, it is just send it to that one physical port and down that wire. For everyone else who is plugged in to that switch, won't receive that data (Isslated collision domain)

### ARP attack

On a Ethernet network, connected via switch, it is relatively easy to perform MITM attacks.

An attacker fools other devices on the ntetwork into believing that the attacker is the default gateway or the router by abusing the Address Resolution Protocol (ARP)

The attacker can then observe, record, inject into and manipulate traffic.

In a Wi-Fi network, traffic can also be manipulated in this way.

If you have a hub instead of switch, you can observer all the traffic anyway, but for injection manipulate you will need ARP spoofing

### Address Resolution Protocol

This is IP to MAC resolution, it works at the Data link layer

It resolves the network layer IP address into Data link layer MAC address

```
sudo arp -a
```

If a device doesn't have an entry in it's ARP table to connect to a given IP address, then it needs to issue a ARP request to resolve the IP to MAC and gain the MAC address

ARP is used to find out the MAC address ➡️ ARP broadcast a frame requesting the MAC address for the IP it has ➡️ The device with the correct IP replies this correct MAC address ➡️ This is then added to the ARP table cache

With tools like `arpspoof`, `ethercap` (kali), `cain and able` (windows) an attacker in a network could fool the devices on a network that they are the correct MAC address for the router's IP address, by simply responding to say me I am the router. This is ARP spoofing, ARP poisoning

This is possible because ARP doesn't have authentication, anyone can say they are anyone. It was developed in time when devices on a local network where all trusted

There is absolutely zero security around it. When the ARP spoofing it's beed done, an attacker can see all the traffic of the victim, instead of the router and can perform manipulation, injection, SSL striping etc

The attacker needs to be internal


More: https://www.irongeek.com/

---
