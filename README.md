# The Complete Cyber Security Course: Network Security

| Layer | No. | Org | Transmition | Devices | Protocols |
| --- | --- | ---|---| ---| ---|
| Internet | 3 | IP | Packets | Router Brouter | IP, IPSec, ICMP, IGMP, RIP, OSPF, BGP, IPX, SKIP, SWIPE, NAT, IGRP, EIGRP, BOOTP, DHCP, ISIS, ZIP, DDP |
| Network access | 2 | IEEE 802 | Data Link LLC MAC | Frames | Switch, Bridge, NIC | Ethernet, 802.3, Token Rin5 802.5, ATM, FDDI, 802.4, Wi-Fi 802.11, PPP, L2TP, SLIP, ARP, RARP, 802.1AE MACSec, HDLC |
| Network access | 1 | Physical | Bits | Repeater, Hub, NIC, Cables, MAU, Multiplexer | ISDN, DSL, SONET, 10BASE-T, 10BASE2, 10BASES, 100BASE-TX, 100BASE-FX, 100BASE-T, 1000BASE-T, 1000BASE-SX, EIA-x, RS-x |

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


