# Section 7: Network Monitoring for Threats

- [Syslog](#syslog)
- [Network Monitoring - Wireshark, tcpdump, tshark, iptables part 1](#network-monitoring---wireshark-tcpdump-tshark-iptables-part-1)
- [Network Monitoring - Wireshark, tcpdump, tshark, iptables part 2](#network-monitoring---wireshark-tcpdump-tshark-iptables-part-2)
- [Wireshark - Finding malware and hackers - Part 1](#wireshark---finding-malware-and-hackers---part-1)
- [Wireshark - Finding malware and hackers - Part 2](#wireshark---finding-malware-and-hackers---part-2)
- [Network Monitoring - Wincap, NST, Netminer and NetWorx](#network-monitoring---wincap-nst-netminer-and-networx)

---

## Syslog

A syslog process can run on a network device and send event messages to a logging server, usually known as a syslog server or syslog viewer.
This will provide information about the device and the network to a central location.

Tools used to root, process, transmit and store log messages such as rsyslog and syslog-ng can also be called syslog too.

The syslog protocol is supported by many network devices, especially rooters, swithers and firewalls.

The syslog server usually listens to port UDP 514 (or TCP 514, UDP over TLS 6514)

It has 8 severity errors:

- 0: emergency
- 1: Alert
- 2: Critical
- 3: Error
- 4: Warning
- 5: Notice
- 6: Informational
- 7: Debug

**Different facilities**

- 0: kernel messages
- 1: user-level messages
- 2: main system
- 3: system deamons
- 4: security / authorization messages
- 5: messages generated internally by syslogd
- 6: line printer subsystem
- 7: network news subsystem
- 8: UUCP subsystem
- 9: clock daemon
- 10: security / authorization messages
- 11: FTP daemon
- 12: NTP subsystem
- 13: log audit
- 14: log alert

**Syslog config file**

```
nano /etc/rsyslog.conf
```

can configure log files, server location etc


**DD-WRT as a syslog**

Services > System Log

Can configure more in ssh 

also locally in dd-wrt

```
192.168.1.1/Syslog.asp
tail -f /tmp/var/log/messages
ln -s /tmp/var/log/messages /tmp/www/log.html # be able see the syslog file via web server
syslogd -h
ps | grep syslog # uses the service, instead of the config file
```

also similar in pfSense

```
tail -f /var/log/syslog
tail -f /var/log/user.log
```

**Syslog tools**

- Graylog 2
- Lire
- Logcheck
- Loggly
- Logstash
- Logsurf
- Logwatch
- SEC - Simple Event Correlator
- Snare Agent for Windows
- Splunk
- Swatch
- Syslog-NG

also need a log analyzer

---

## Network Monitoring - Wireshark, tcpdump, tshark, iptables part 1

**Protocol Analysers for Network Traffic**

`tshark` is the commant line version of the wireshark, only available if wireshark is installed. Same options as tshark and more.

```
tshark -h
```

tcpdump is another command line protocol analyzer that is often avaiable natively

---

**Runtime**

We can run them on each of the devices of the network (limited in the traffic that you can see, if you are using a switch), we can run it on the router or firewall.

You can try ssh into the router or the firewall and run tcpdump

```
tcpdump -h
tcpdump -D
tcpdump -i eth0 # capture traffic
tcpdump -i any
tcpdump -n -i any # display ip addresses and port numbers
tcpdump -n -i any dst port 80
tcpdump -n -i any dst port 53
tcpdump -n -i any port 53
tcpdump -i ppp0 -vv ip6 # modem interface for ipv6
tcpdump -i any port 53 or port 80 or port 443
tcpdump -i any host 192.168.1.81 and not src net 192.168.1.0/24 # can see if anything did connect into it
tcpdump -i any -s 65535 -w capturefile.cap # capture to a file with storage capability
```

---

## Network Monitoring - Wireshark, tcpdump, tshark, iptables part 2

**Wireshark GUI**

```
apt-get install wireshark
```

**Can use port mirroring functionality of `iptables`**

```
iptables -t mangle -A PREROUTING -s 192.168.ip.to.monitor -j TEE -gateway 192.168.1.wireshark
```

**SSH and capture the packets on the fly (no iptables configuration)**

- `ssh root@192.168.1.1 -- "tcpdump -w - -s 65535 'not port 22' " > capture.cap`
  - `s 65535`: full size packet capture
  - `'not port 22'`: no capturing of the traffic that is relying back the tcp dump
- import in wireshark

**Pipe tcpdump to wireshark over SSH**

- `ssh root@192.168.1.1 tcpdump -U -s 65535 -w - 'not part 22' | wireshark -k -i -`
  - `-k -i`: send tcpdump into wireshark

---

## Wireshark - Finding malware and hackers - Part 1

Choose interface and click on start capturing, for each packet there is:
- frame number
  - In data link the units are kwown as frames
  - ethernet frames, token ring frames, wi-fi, arp
  - there is also hex for the data frame in here
- time
- source IP
- destination IP
- protocol
- actual commands

**Change IP to Domain Name**

Edit > Preferences > Name Resolution > Resolve network (IP) adresses

**Start a new live capture**

By clicking on - shark - icon

**Capture Options**

Capture Filter: exactly the same as the `tcpdump`
- IP address 192.168.0.1
- TCP only
- UDP only
- DNS
- Any from outside connecting to 192.168.1.40
- Ports 80 443 53

It's green if it's correct systax

**Filter Expressions**

Built in protocols that it understands

- HTTP - Hypertext Transfer Protocol
  - or just the frames that have cookies on them
  - `http.set_cookie`

**Look for suspicious traffic**

- Start removing the traffic that you do understand, and you do know is valid
  - valid pattern example
  -  `not(tcp.port==80) and not(tcp.port==443) and not (udp.port==53)`
-  can add more filters from frame
  - right click on source > Apply as Filter > ... and not Selected
  - it will add that destination on the filter
- can remove only protocols
  - `not dns || http || ssl`
  - `not smb || nbns || dcerpc ||  nbss`
  - `tcp.srcport==80`
  - `ip.src==192.168.1.0/24 and ip.dst==192.168.1.0/24`
  - `ip.dst==192.168.1.39 and !ip.src==192.168.1.0/24`
    - from external to the internal

---

## Wireshark - Finding malware and hackers - Part 2

If not HTTPS is used we can capture names and passwords and cookies
- filter by `http` or `http.cookie`
- look for POST: `http.request.method == POST`

**Follow Stream**

Right Click on a frame > Follow TCP Stream > We can see the conversation 

**Statistics**

Menu Statistics > Conversations > IPv4:19

We can see the traffic and see if anything is getting out

Right Click > Apply Filter

**Show all the domains, domains that is enforced to resolve**

Statistics > Show address resolution

**Show all endpoints**

Statistics > Endpoints > Right click > Selected > will filter all frames for that source

---

## Network Monitoring - Wincap, NST, Netminer and NetWorx

Various tools for traffic analysis
