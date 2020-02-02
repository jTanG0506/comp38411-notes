---
title: Virtual Private Networks
markup: "mmark"
weight: 3
---

## Virtual Private Networks
A **virtual private network** (VPN) is a security solution which allows the provisioning of private network services over a public or shared infrastructure such as the Internet or service provider network. To achieve these private network services, VPNs make use of tunnelling, encryption, authentication, and access control technologies.

-----

### VPN Terminology

A **VPN tunnel** encapsulates data of one protocol inside the data field of another protocol; headers are added to data to provide routing information to allow transmission across the Internet to reach its destination, to emulate a dedicated point-to-point link. For **IP-in-IP encapsulation**, IP packets are encapsulated in another IP packet.

![VPN Header](/docs/figures/vpn-header.png)

A **VPN router** or **VPN gateway** is located at the corporate network perimeter, which is responsible for performing tunnelling, authentication and the encryption / decryption of data. They can be either **standalone* devices or **integrated* implementations into existing devices such as routers and firewalls. Router-based VPNs add encryption support to existing routers while keeping the upgrade costs of a VPN low. Firewall based VPNs are a workable solution for small networks with low traffic volume.

![VPN Internet](/docs/figures/vpn-internet.png)

A **VPN client** is a piece of software that is used for remote VPN access. The purpose of a VPN client is to create a secure path from the remote client computer to a VPN router or gateway. VPN clients can be loaded onto an individuals computer that requires remote access or onto a router that establishes a peer-to-peer VPN connection.

During tunnel setup, the devices on each side of the tunnel agree on the details of authentication and encryption. Here, authentication refers to identifying VPN users and devices for ensuring the authenticity of data; encryption refers to protecting the confidentiality of data in transit across the Internet.

------

### VPN Protocols
VPNs can be implemented in a variety of ways:
1. **PPTP** (point-to-point tunnelling protocol), which is based upon PPP
1. **L2TP** (Layer 2 tunnelling protocol), which requires pre-arrange paths between devices or to and from a secure server
1. **IPSec**, which is our focus for this chapter
