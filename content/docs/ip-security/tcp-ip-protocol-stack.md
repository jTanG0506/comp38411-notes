---
title: TCP / IP Protocol Stack
markup: "mmark"
weight: 1
---

## TCP / IP Protocol Stack
**Internet Protocol** (IP) deals with routing packets of data from one computer to another. **Transmission Control Protocol** (TCP) deals with ensuring that data packets are delivered reliably from one computer to another. We could say that TCP sits on top of IP, in the sense that TCP asks IP to send a packet to its destination and ensures that the packet was indeed received at the destination. 

| Layer | Protocols |
|----------------------|----------------------|
|**Application Layer** | HTTP, FTP, SMTP, etc |
|**Transport Layer**   | TCP, UDP             |
|**Network Layer**     | IP                   |
|**Link Layer**        | Ethernet, WiFi, etc  |

Note that there is an equivalent 7-layer model of the protocols referred to as the *Open Systems Interconnection* (OSI) model. Although TCP and IP are just two of the protocols that reside in the stack, the entire stack is commonly referred to as TCP / IP protocol stack as the main roles are played by the TCP and IP protocols. 

-----

### Providing security across the layers

In the TCP / IP protocol stack, end-to-end information security can be provided at different layers.
1. **Network Layer**: We can provide security in the Network Layer by using, say the IPSec protocol. This approach can reduce the need for higher-level protocols to provide security and also take away the management of security from the application developer.
1. **Transport Layer**: We can provide security in the Transport Layer in a manner that is agnostic with regard to specific applications, by adding security-related features to TCP packets. This can be done with a Session Layer protocol such as Secure Sockets Layer (SSL / TLS).
1. **Application Layer**: We can provide security at the highest level of the stack by embedding security directly in the application itself. Examples of these are PGP and S/MIME.
