# 4. Firewalls

[Introduction](#introduction)

[Categories of Firewalls](#categories-of-firewalls)

[Ingress / Inbound filtering](#ingress--inbound-filtering)

[Egress / Outbound filtering](#egress--outbound-filtering)

[MITM mitigation](#mitm-mitigation)

[Network isolation](#network-isolation)

[Home Network](#home-network)

[Virtual Firewall](#virtual-firewall)

[IP Tables](#ip-tables)

[Linux - Host Based Firewalls - UFW, gufw & nftables](#linux---host-based-firewalls---ufw-gufw--nftables)

---

## Introduction

Firewalls allow and deny traffic based on a set of rules or Access Control List. Most work on the network and transport layers to accept or reject traffic based on port protocol and address.

**Example**

Allow out of your network only HTTP traffic on TCP port 80 also HTTPS traffic on port 443, DNS or UDP on port 53, and deny everything else.

```
TCP:HTTP:80
TCP:HTTPS:442
UDP:DNS:53
```

Also, firewalls can be configured to allow or restrict access to specific IP addresses or IP ranges

192.168.1.2 ➡️ TCP:HTTPS:442 ⬅️ 177.1.2.3


## Categories of Firewalls

More advanced firewalls work at the application layer to do deep packet inpection (DPI) and are generally dedicated hardware firewalls

These have protocol and port based rules like they can allow HTTPS traffic on port 443 **plus** they inspect the traffic content to make sure that they are disarable and it fits to the rules that have been specified

E.g. they could look at the TLS handshake to verify it is HTTPS and not SSH or tor trying to sniff through the port, just because it happens to be the port 443

Firewalls that are not application level firewalls cannot do deep inspection inspection and therefore they will let it through. This is the difference between those that are network based (layer 3,4) and those that are based on layer 7 or application or proxy firewalls that do DPI.


- Host based
- Network based
- Virtual

---

## Ingress / Inbound filtering

On a home network, because of NAT, no network traffic can connect into the network without an explicit **port forwarding** rule or **DMZ** being setup

There is little need for firewall to block inbound traffic in most cases in a home network because internet traffic from real IPs cannot communicate to private IP addresses on an internet network

OS have their own firewalls, these are called host based firewalls

On all firewalls, whether they are host based or network based there should be an implicit **deny-all** rule to external traffic connecting inbound, unless it is specifically required, on home routers that isn't required because of NAT, on a host based firewall that may be to be configured

**Example**

When Device sends outbound traffic, firewall logs this in the state table:

```
Connection:
  Source IP: 192.168.1.10
  Source Port: 52345
  Destination IP: 93.184.216.34
  Destination Port: 443
  Protocol: TCP
  State: ESTABLISHED
  Timeout: [some duration]
```

**Considerations**

- Every TCP/UDP Packet Contains Source and Destination Ports
- The firewall expects
  - Replies to match exact values (IP, port, protocol, sequence number)
  - Packets to arrive in order

---

## Egress / Outbound filtering

A firewall can be used to filter outbound connections, e.g. stopping your windows machine communicating to Microsoft, or DNS licks from VPN, or malware communicating to it's commanding control server, can be blocked at the firewall with egress or outbound filtering

Outbound filtering is a strong use case for using a network based NAT router firewall on a home network

A reverse shell, or reverse connection in case of a malware, is needed for this outbound connection, so that the advesary can control that device (the code that runs the reverse connections is called shell code)

---

## MITM mitigation

**TCP sequence numbers** play a major role in preventing spoofing, especially for connection-based protocols like TCP. 

what else do firewalls do to prevent **Man-in-the-Middle (MITM)** attacks?

Let’s look at it in layers — because firewalls, while powerful, are just one layer of defense. Here's how firewalls and related systems help mitigate MITM or spoofing attacks:


### 🔐 1. **Stateful Packet Inspection (SPI)**

As you already understand:

* The firewall **tracks connection state** (3-way handshake, IPs, ports, sequence numbers).
* It **only allows packets that match a known, established connection**.
* Any out-of-place packet is dropped — this stops spoofed replies or hijacked TCP streams.

📌 **Spoofing fails** because the attacker would need to guess or hijack exact IP/port/sequence info **mid-connection**, which is very hard.

---

### 🧱 2. **Reverse Path Filtering (RPF)**

This technique helps prevent **IP spoofing**:

* When a packet arrives, the firewall/router checks if it would **send a response to that source IP via the same interface** it came in on.
* If not, the packet is dropped — assuming it’s spoofed.

✅ **Blocks** attackers trying to spoof a fake source IP that doesn’t match valid routes.

---

### 🚫 3. **Ingress and Egress Filtering**

Firewalls can apply rules such as:

* **Ingress filtering**: Only allow incoming traffic from **valid, expected source IP ranges**.
* **Egress filtering**: Only allow internal devices to send traffic with **their assigned IPs**.

✅ **Stops MITM attackers** from injecting traffic with forged IPs.

---

### 🔐 4. **TLS Termination / Deep Packet Inspection (DPI)**

Some advanced firewalls (e.g., next-gen firewalls or enterprise proxies):

* Can perform **TLS interception** (with the right trust setup).
* Inspect encrypted HTTPS traffic to **detect MITM attempts**, malware, or unauthorized certificates.

🔸 **Downside**: This breaks true end-to-end encryption unless carefully deployed (e.g., in enterprise environments with trusted root CA).

---

### 🧠 5. **Application-Layer Protections**

Firewalls can be configured to monitor:

* **Unexpected DNS, HTTP headers, or SSL certificates**.
* **Changes in TLS fingerprints** (JA3 hashes) — a sudden change can indicate MITM.

✅ Example: If a device suddenly starts talking to a server using a self-signed or untrusted certificate, the firewall can block it.

---

### 🌐 6. **Detection of ARP/DNS Spoofing (Local MITM)**

Some firewalls include or integrate with **intrusion detection systems (IDS)** to watch for:

* **ARP poisoning** (common in local MITM)
* **DNS spoofing** (redirecting traffic to fake servers)
* **MAC spoofing**

✅ If detected, the firewall can alert or isolate the offending device.

---

### 🚧 7. **Connection Rate Limiting / SYN Flood Protection**

A MITM trying to hijack or inject traffic might:

* Send lots of forged SYN or ACK packets.
* Attempt to guess sequence numbers.

Firewalls can:

* Rate-limit SYNs from a source.
* Use **SYN cookies** to protect against spoofed TCP handshakes.
* Detect **anomalies** in TCP flags or sequences.

---

### 🧰 8. **Logging + Alerting + Behavioral Analysis**

Firewalls can log patterns like:

* IPs using multiple ports rapidly (port scanning).
* Repeated handshake failures.
* Certificate anomalies.

This allows:

* **Early detection** of active MITM or spoofing attempts.
* **Automated blocking or isolation** of suspected sources.

---

### ✅ Summary: How Firewalls Help Stop MITM

| Technique                       | What It Prevents             |
| ------------------------------- | ---------------------------- |
| TCP state tracking              | Packet injection / hijacking |
| Sequence number validation      | Spoofing + session hijack    |
| Reverse path filtering          | IP spoofing                  |
| Ingress/egress filtering        | Forged packets, invalid IPs  |
| TLS fingerprint/inspection      | Rogue TLS certs or sessions  |
| ARP/DNS spoof detection         | Local MITM attempts          |
| SYN rate limiting / SYN cookies | DoS, spoof floods            |


> 🔐 **Important:** Firewalls help **prevent MITM attempts**, but the best defense against MITM is **end-to-end encryption** like **HTTPS (TLS)** — because even if someone manages to sit in the middle, they can’t decrypt or alter traffic without being detected.

---


## Network isolation

There is a growing use case for using a firewall to block traffic between devices on the internat home network, to prevent a propagation of an attack from an internal device to another internal device (also a router and a switch can be used to create network isolation)

IoT devices increasingly should be considered untrusted and isolated from devices that you care about

It is much more likely that an IoT device have bugs, unpached OS and more likely to be backdoored

Absolutely — you're touching on one of the most important shifts in modern home network security: treating **internal traffic** with the same suspicion as **external** traffic. Let's go deep into how this works in **practice** and why it's essential, especially in homes with **IoT devices**.

---

### 🧠 The Old Mental Model: "Trusted LAN"

Traditionally, home networks were designed like this:

* Everyone inside the LAN is "trusted."
* The firewall only blocks incoming traffic **from the internet**.
* Devices like laptops, phones, printers, TVs, and smart bulbs all share the same **flat network** (e.g., `192.168.1.x`).

> 🔥 Problem: If one device is compromised (e.g., your smart camera), **it can scan, talk to, or attack** every other device.

---

### 🚨 Why IoT Devices Are a Threat

IoT devices (smart bulbs, cameras, thermostats, fridges) are:

* Often cheaply made with **minimal security**.
* Rarely updated (or updates stop after 1–2 years).
* Sometimes intentionally collect data or expose backdoors.
* May use **outdated software stacks** with known vulnerabilities.

So they represent **soft entry points** into your network.

---

### ✅ The New Model: **Zero Trust Inside the LAN**

This is where **firewalls, VLANs, and segmentation** come in.

Instead of:

> "If it's on the LAN, it's trusted"

We shift to:

> "Everything is untrusted until explicitly allowed — even inside the LAN"

---

### 🔧 How to Block or Isolate Internal Devices (In Practice)

### 🔹 1. **Firewall Rules on Home Router**

Many advanced routers (e.g., OpenWRT, pfSense, Ubiquiti, ASUS Merlin) allow internal firewall rules.

**Example rule:**

* **Block traffic from `192.168.1.100` (IoT camera) to `192.168.1.200` (your laptop)**.
* Only allow it to talk to the internet on port 443.

This stops lateral movement.

### 🔹 2. **Use Guest Networks or VLANs**

Most modern routers support:

* **Guest networks**: Isolated Wi-Fi for guests or IoT devices.
* **VLANs** (Virtual LANs): Logical network separation at the switch/router level.

**Example VLAN setup:**

| VLAN | Devices            | Internet Access | LAN Access |
| ---- | ------------------ | --------------- | ---------- |
| 10   | Laptops, phones    | ✅ Yes           | ✅ Yes      |
| 20   | Smart TVs, cameras | ✅ Yes           | ❌ No       |
| 30   | Guest Wi-Fi        | ✅ Yes           | ❌ No       |

### 🔹 3. **Layer 2 Isolation (Switch-level)**

Managed switches can be configured so that:

* **Ports can’t talk to each other**, only to the internet gateway.
* This is common in **port isolation** or **private VLANs**.

Great for cases like:

* Multiple smart devices plugged into a switch.
* Preventing them from scanning or attacking each other.

---

### 🔐 What Firewall Rules Might Look Like

If using a firewall like **pfSense**, rules could look like:

* **Allow IoT VLAN → Internet (ports 80/443)**
* **Block IoT VLAN → Home VLAN**
* **Block IoT VLAN → DNS servers except router**
* **Log and alert on any internal scan attempt**

---

### 🛡️ Benefits of Internal Isolation

| Benefit                    | Why It Matters                                       |
| -------------------------- | ---------------------------------------------------- |
| **Limits damage**          | If one device is hacked, it can’t infect others      |
| **Reduces attack surface** | IoT can't probe laptops, NAS, or printers            |
| **Improves privacy**       | Stops smart TVs and devices from snooping on your PC |
| **Adds visibility**        | Firewall logs show any unexpected internal activity  |
| **Future-proofs**          | New devices default into the untrusted zone          |

---

### 🧪 Real-World Example

Say you have:

* A smart camera (`192.168.20.50`)
* Your laptop (`192.168.10.100`)

You configure:

* VLAN 20: IoT (can access internet only)
* VLAN 10: Private LAN (full access)
* Firewall rule: Block all `VLAN 20` to `VLAN 10`

Now:

* Even if the smart cam is compromised, it **can’t ping, port scan, or attack** your laptop.
* You’ll see logs if it tries — and know something’s up.

---

### 🧠 Summary: Key Takeaways

* 🔐 **LAN != Safe anymore** — devices inside the home can be threats.
* 🔧 Use **firewalls, VLANs, and guest networks** to isolate traffic inside your home.
* 📉 Reduce IoT permissions: **Only let them access what they need**.
* 📊 Monitor and log internal traffic for unusual behavior.

---

Would you like a visual layout or a sample pfSense or OpenWRT rule set for this kind of home setup?


---

## Home Network

- Network-based Firewall
  - For LAN to Internet filtering
  - could be on the router with IPTables
  - no outbound protection // which isn't good
  - inbound connection allowed via NAT

A disadvantage of network based firewalls is that malware can use the same ports to create a shell connection e.g. TCP:HTTP:80, TCP:HTTPS:443, UDP:DNS:53

Major disadvantage of home based firewalls: Inability to block outbound connections because we cannot distinguish between good and bad traffic

- Host-based Firewall
  - e.g. Comodo on Windows, IPTables on debian (default in linux)
 
Because host-based firewalls are local to the system, they have the disadvantage on being on the same platform as an a malware that might installed on the device trying to attack it

Because of this, malware that takes over your devices, could just disable the firewall

Clever malware could also use trusted software processes that are allowed through the firewall to bypass the firewall

For example, the firewall could you the browser process to communicate out, and some malware does just that

Network firewalls cannot be disabled by a malware that is running on a device

But host based firewalls have an advantage. They can understand what applications are running on a device
and can blocked traffic based on white / blacklist. e.g. **allow only the trusted software to communicate out**

So untrusted malware could not run a reverse shell (but a malware can bypass this via an untrusted process, like the browser)

---

## Virtual Firewall

You could have a network or host-based virtual firewall
- Zen Server on your network that has one of it's virtual machines acting as a firewall
- Laptop with virtual box with a pfSense VM to protect guest virtual machines

Virtual firewall means you don't need a dedicated hardware firewall. Usefull when:
-  nesting VPN services
-  prevent VPN leaks
-  filtering traffic on VM

Main rule: all traffic should be denied unless implicitly allowed
- Block IPv6 (if not used)
- Block UPnP 1900
- Block IGMP
- Block any Windows, Max OS X, Linux services that are not used

---

## Dynamic Packet Filtering or Stateful Packet Inspection (SPI)

Modern firewalls have SPI and you only need to allow outbound connections, you don't need a inbound rule for traffic to return to you

e.g. you don't need to allow web traffic to come back // only need to allow it out of network

When a connection is made, the source port is sent (higher than 1023) and the firewall remembers and automatically allows through Dynamic ACL the inbound connection while the session is running.

---

## IP Tables

All linux based firewalls use the **netfilter** system for kernel packet filtering

The firewalls use port, protocol and IP and operate at network / transport layer

IPTables is the database of firewall rules

**Install IPTables**
```
apt-get install iptables
```


**List of chains and rules**

```
iptables -L
```

- Input Chain
  - Input controls the inbound connections
  - e.g ping this laptop / files share
- Forward Chain
  - Forward for connections **destined to be forward on to another device** (like a router)
  - also for inbound connections
  - only for special devices like routers, NAT addressing
- Output Chain
  - Ouput chain controls outbound connections (e.g. allow the laptop to serf the web)

Chains have default behavior
- Default Policy **DROP**: When non of the existing rules apply the connection will be dropped
  - requests will be timed out
- Reject: don't allow the connection and send back a response to inform the source that has been rejected
  - ping message: destination port unreachable
- Accept

### Usufull commands

**Delete all the rules**

```
iptables -F
```

**Set default policy**
```
iptables -P INPUT DROP
```

or

```
iptables -P INPUT ACCEPT
iptables -P OUTPUT ACCEPT
```

### Demonestration of laptop that I want to lock down to only browse the web

```
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT DROP
```

**Allow all incoming packets destined for the local host interface to be accepted**

```
iptables -A INPUT -i lo -j ACCEPT
```
- -A append
- -i interface
- lo local
- -j Jump switch
  - jump to the target action for packets maching that rule

---

ACCEPT  all  ---  anywhere  anywhere

**Verbose information**
```
iptables -L -v --line-number -n
```

**Next rule**
```
iptables -A INPUT -m state --state RELATED, ESTABLISHED -j ACCEPT
```
- -m state imports a state module that can examine the state of a packet and determine if it is
  - new (new connections that weren't initiated by the host system),
  - established
  - related (incoming packets of already established connections)
  - this is enabling dynamic packet filtering

**Next rule**
```
iptables -A OUTPUT -o lo -j ACCEPT
```

**With dynamic packet filtering**
```
iptables -A OUTPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

**To serf the web, will need port 53 UDP // DNS queries**
```
iptables -A OUTPUT -o eth0 -p udp -m udp --dport 53 -j ACCEPT
```

**Also add port 80 HTTP**
```
iptables -A OUTPUT -o eth0 -p tcp -m tcp --dport 80 -m state --state NEW -j ACCEPT
```

**Also for port 443 HTTPS**
```
iptables -A OUTPUT -o eth0 -p tcp -m tcp --dport 443 -m state --state NEW -j ACCEPT
```

**Ouput of IPTables after adding the rules**

```
iptables -L --line-number -n
```

| Chain Num | Output target | prot | opt | source | destination | desc |
|----------| ------------- | ---- | -----| -------| ----------- | ---- |
| 1 | ACCEPT | all | -- | 0.0.0.0/0 | 0.0.0.0/0 | _ |
| 2 | ACCEPT | all | -- | 0.0.0.0/0 | 0.0.0.0/0 | state RELATED, ESTABLISH |
| 3 | ACCEPT |udp | -- | 0.0.0.0/0 | 0.0.0.0/0 | udp dpt:53 |
| 4 | ACCEPT | tcp | -- | 0.0.0.0/0 | 0.0.0.0/0 | tcp dpt:80 state NEW |
| 5 | ACCEPT | tcp | -- | 0.0.0.0/0 | 0.0.0.0/0 | tcp dpt:443 state NEW |


**Show commands entered to create the rules**
```
iptables - S
```

**Save rules**

Kali / Debian
```
/sbin/iptables-save
```

**Delete rules (use line number and chain)**
```
iptables -D OUTPUT 5
```
**Remove rules (firewall switched off)**

```
iptables -t nat -F
iptables -t mangle -F
iptables -F //flush command
iptables -X //delete non default chains
```

**V6 IPs have different IP table**

needs to be configured separately

```
ip6tables -L
```

```
ip6tables -P INPUT DROP
ip6tables -P FORWARD DROP
ip6tables -P OUTPUT DROP
```

### Resources

- https://www.frozentux.net/iptables-tutorial/iptables-tutorial.html
- https://github.com/meetrp/personalfirewall

---

## Linux - Host Based Firewalls - UFW, gufw & nftables

### UFW, UncomplicatedFirewall

Frontend for IP tables, provides a framework for managing `netfitler` and CLI for manipulating the firewall
- Simplifies IP tables commands
- upstream for other distribution e.g. gufw

**Commands**
```
apt-get install ufw
iptables -L --line-number -n
iptables -F
iptables -L --line-number -n
ufw status
ufw enable
ufw status
iptables -L --line-number -n
ufw status verbose
```

**Change default policy and more**
```
ufw default deny incoming
ufw default deny outgoing
ufw default allow outgoing
```

**Allow / delete ports**
```
ufw allow out 22
ufw delete allow out 22
ufw status numbered
ufw delete 2
ufw deny in 22 // unnecessary because of default policy
ufw allow out 67:68/udp // range of ports, dhcp rule
```

**Only write change on IPV4. not IPV6**
```
nano /etc/defualt/ufw
IPV6=no // restart firewall
iptables -L -n
```

### Shorewall

Alternative to IP tables, more flexible and powerfull but not similer

---

### Gufw

Graphical interface of UFW

```
apt-get install gufw
```

---

### GUIs & Frontends

http://www.iptables.info/en/iptables-gui.html
http://www.fwbuilder.org/4.0/hot_it_works.shtml
http://www.turtlefirewall.com/
http://configserver.com/cp/csf.html
http://www.ipcop.org/
https://www.vuurmuur.org/trac/wiki/ScreenShots
https://help.ubuntu.com/community/firewall/ipkungfu
http://www.linuxguruz.com/forum/viewforum.php?f-35

---

### Linux-firewall

Only this firewall is application aware, block / allow based on application

---

### nftables

nftables is the project that aims to replace the existing {ip,ip6,arp,eb} tables framework

