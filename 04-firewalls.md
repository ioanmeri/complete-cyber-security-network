# 4. Firewalls

Firewalls allow and deny traffic based on a set of rules or Access Control List. Most work on the network and transport layers to accept or reject traffic based on port protocol and address.

**Example**

Allow out of your network only HTTP traffic on TCP port 80 also HTTPS traffic on port 443, DNS or UDP on port 53, and deny everything else.

> TCP:HTTP:80

> TCP:HTTPS:442

> UDP:DNS:53

Also, firewalls can be configured to allow or restrict access to specific IP addresses or IP ranges

> 192.168.1.2 -> TCP:HTTPS:442 <- 177.1.2.3


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

There is little need for firewall to block inbound traffic in most cases in a home network because internet traffic from real IPs cannot communicate to private IP addresses on an internat network

OS have their own firewalls, these are called host based firewalls

On all firewalls, whether they are host based or network based there should be an implicit **deny-all** rule to external traffic connecting inbound, unless it is specifically required, on home routers that isn't required because of NAT, on a host based firewall that may be to be configured

---

## Egress / Outbound filtering

A firewall can be used to filter outbound connections, e.g. stopping your windows machine communicating to Microsoft, or DNS licks from VPN, or malware communicating to it's commanding control server, can be blocked at the firewall with egress or outbound filtering

Outbound filtering is a strong use case for using a network based NAT router firewall on a home network

A reverse shell, or reverse connection in case of a malware, is needed for this outbound connection, so that the advesary can control that device (the code that runs the reverse connections is called shell code)

---

## Network isolation

There is a growing use case for using a firewall to block traffic between devices on the internat home network, to prevent a propagation of an attack from an internal device to another internal device (also a router and a switch can be used to create network isolation)

IoT devices increasingly should be considered untrusted and isolated from devices that you care about

It is much more likely that an IoT device have bugs, unpached OS and more likely to be backdoored

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

Input Chain // Forward Chain // Output Chain

Input controls the inbound connections

Forward for connections destined to be forward on to another device (like a router)

Ouput chain controls outbound connections (e.g. the laptop to server the web)

Default Policy **DROP**: When non of the existing rules apply the connection will be dropped

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

**Demonestration of laptop that I want to lock down to only browse the web**

```
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT DROP
```

**-A append, -i interface, lo local, -j Jump switch**
**Allow all incoming packets destined for the local host interface to be accepted**

```
iptables -A INPUT -i lo -j ACCEPT
```

==

ACCEPT  all  ---  anywhere  anywhere

**Verbose information**
```
iptables -L -v --line-number -n
```

**Next rule**
```
iptables -A INPUT -m state --state RELATED, ESTABLISHED -j ACCEPT
```
--state imports a state module that can examine the state of a packet and determine if it is new (new connections that weren't initiated by the host system), established or related (incoming packets of already established connections)

This is already dynamic packet filtering

**Next rule**
```
iptables -A OUTPUT -o lo -j ACCEPT
```

**With dynamic packet filtering**
```
iptables -A OUTPUT -m state --state RELATED, ESTABLISHED -j ACCEPT
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


---


