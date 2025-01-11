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
