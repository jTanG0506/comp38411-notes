---
title: Encapsulating Security Payload
markup: "mmark"
weight: 5
---

## Encapsulating Security Payload
The **Encapsulating Security Payload** (ESP) protocol is used for providing encryption services in IPSec. While ESP may be used to provide the same services as the AH header, its main purpose is to provide confidentiality through encryption.

The figure below shows the structure of the Encapsulating Security Payload (ESP).

![Encapsulating Security Payload](/docs/figures/encapsulating-security-payload.png)

- **SPI**: a 32-bit value which establishes the Security Association for this packet.
- **Sequence Number**: a monotonically increasing number for each packet sent to prevent replay attacks.
- **Encrypted Payload Data**: a transport-level segment (transport mode) or IP packet (tunnel mode) that is protected by encryption.
- **Padding**: up to 255 bytes of padding
- **Pad Length**: the number of pad bytes immediately preceding this field.
- **Next Header**: the type of the header immediately following the AH. IP being 4, TCP being 6, UDP being 17, etc.
- **Authentication Data**: the MAC of the packet calculated with either SHA-1 or HMAC.

-----

### Padding
The padding field in the ESP serves several purposes:
- Takes care of the fact that the length of the encrypted payload would ordinarily be a multiple of the block size used for encryption with symmetric key cryptography.
- Ensures the right alignment of the Pad Length and Next Header fields, as required by the ESP format.
- Provides partial traffic-flow confidentiality by concealing the actual length of the payload.

-----

### Comparison of IPv4 Packet

![ESP IPv4](/docs/figures/esp-ipv4.png)
