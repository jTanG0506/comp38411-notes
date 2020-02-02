---
title: Authentication Header
markup: "mmark"
weight: 4
---

## Authentication Header

The figure below shows the structure of the Authentication Header (AH).

![Authenticated Header](/docs/figures/authenticated-header.png)

- **Next Header**: the type of the header immediately following the AH. IP being 4, TCP being 6, UDP being 17, etc.
- **Payload Length**: the length of the AH in 32-bit word, minus the integer 2.
- **Reserved**: not used, and is defaulted to 0.
- **SPI**: a 32-bit value which establishes the Security Association for this packet.
- **Sequence Number**: a monotonically increasing number for each packet sent to prevent replay attacks.
- **Authentication Data**: the MAC of the packet calculated with either SHA-1 or HMAC.

-----

### Comparison of IPv4 Packet

![AH IPv4](/docs/figures/ah-ipv4.png)

-----

### Computation of MAC
The Authentication Data field in the AH of the packet is a MAC calculated over the header fields that do not change in transit; the source and destination IP addresses, the AH header without the Authentication Data, and the inner IP packet for establishing authentication in tunnel model. That is, all mutable IP header fields, such as TOS, flags, fragment offset, TTL and header checksums, are zeroed prior to MAC calculation.
