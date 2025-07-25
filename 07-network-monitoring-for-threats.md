# Section 7: Network Monitoring for Threats

- [Syslog](#syslog)

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


