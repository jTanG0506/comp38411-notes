---
title: IPSec
markup: "mmark"
weight: 2
---

## IPSec
**IPSec** is a specification for the IP level security features that are defined in the IPv6 internet protocol. IPSec is below the transport layer and so is transparent to applications, meaning there is no need to make changes to software  when IPSec is implemented in the firewall or router. IP level authentication means that the source of the packet is as stated in the packet header, and that the packet was not altered during transmission. IP level confidentiality means that a perpetrator cannot eavesdrop on the communications during transit of data.

IP level authentication can be provided by inserting an **Authentication Header** (AH) into the packets, which is simply a hash value for the portions of a packet that are expected to stay invariant during its transmission from the source to the destination. IP level confidentiality can be provided by inserting an **Encapsulating Security Payload** (ESP) header into  the packets, which can do the job of the AH header by providing authentication in addition to confidentiality.

-----

### Security Association
A **Security Association** (SA) is a one-way logical connection between a sender and a receiver that affords security services to the traffic carried on it. That is, a security association is an establishment of a set of security attributes between entities for the protection of IP traffic. An SA may include attributes such as the authentication mechanism (HMAC being the default), the encryption algorithm and mode (DES-CBC being the default), a shared session key, and the initialisation vector. Since an SA is unidirectional, two security associations are required if we wish to have a secure two-way exchange.

A security association is uniquely identified by:

1. **Security Parameters Index (SPI)**: A 32-bit unsigned integer assigned to this SA and having local significance only.
1. **IP Destination Address**: The address of the destination endpoint of the SA, which may be an end-user system or a network such as a firewall or a router.
1. **Security Protocol Identifier**: Identifier for whether the association is an AH or ESP security association.

-----

### Key Management
There are two types of key management, as described by the IPSec Architecture document.

- **Manual**: The keying material and SA data are manually configured for each system. This is practical for small, relatively static environments but does not scale well.
- **Automated**: An automated system enables the on-demand creation of keys for SAs and facilitates the use of keys in a large distributed system.

------

### Establishing an SA and Session Key

![IKE Phase 1](/docs/figures/ike-phase-1.png)

1. The initiator $$I$$ sends a SA negotiation payload $$SA_I$$ with one or more proposals.
1. The responder $$R$$ chooses a single SA from the proposals and responds with $$SA_R$$.
1. The initiator sends the key exchange payload $$KE_I$$ containing the public information exchanged in a Diffie-Hellman exchange and a nonce payload $$N_I$$.
1. The responder also sends a Diffie-Hellman key exchange payload $$KE_R$$ and a nonce payload $$N_R$$.
1. The initiator sends their certificate $$Cert_I$$ along with their signature on $$R$$'s nonce, $$I\{N_R\}$$.
1. The responder sends their certificate $$Cert_R$$ along with their signature on $$I$$'s nonce, $$R\{N_I\}$$.

-----

### Authentication Methods
The **Internet Security Association and Key Management Protocol** (ISAKMP) is a protocol for establishing Security Associations and cryptographic keys, which supports multiple authentication methods:

1. **Symmetric Key Cryptography**: Each host has the same pre-installed keys, and the peers authenticate one another by computing and sending a keyed hash of data which includes the pre-shared keys.
1. **Public Key Cryptography**: Each party generates a nonce and encrypts it and their ID using the other party's public key. The ability to decrypt the data with the local private key authenticates the parties to each other. This method requires the ability to generate random numbers and perform public-key encryption and decryption. Currently, only the RSA algorithm is supported.
1. **Digital Signatures**: Each device signs some data contributed by the other entity, providing non-repudiation. This scheme supports both RSA and DSS.

-----

### Transport Mode
The **Transport Mode** is the *regular* mode for packets to travel from a source to its destination in a network and is only applicable to host implementations. The two endpoints must carry out the security checks on the packets based on the information contained in the authentication header. In Transport Mode, the original source and destination addresses are visible, thus enabling technology to ensure Quality of Service (QoS), but makes traffic analysis possible.

![Transport Mode](/docs/figures/transport-mode.png)

-----

### Tunnel Mode
Suppose the source and destination endpoints for a given packet stream do not have the ability or resources to carry on the security checks on the packets. Therefore, the source must route the packets to an IPSec Server $$X$$ whose role is to insert the authentication and / or ESP headers. If the intended destination also is not able to carry out such security checks on the packets, $$X$$ may need to send the packets to another IPSec Server $$Y$$, which is in the *vicinity* of the actual destination of the packet stream. The host at $$Y$$ can then carry out the security verification based on the information provided in the security headers inserted b $$X$$ and send the packets to their true destination.

![Tunnel Mode](/docs/figures/tunnel-mode.png)

$$X$$ is sometimes referred to as the **encapsulator** and $$Y$$ as the **decapsulator**. The points $$X$$ and $$Y$$ define the two endpoints of what's referred to as a **tunnel**.

-----

### Transport Mode vs Tunnel Mode
If we wish to use IPSec for just authentication of sender and receiver information that is placed in the IP headers, and if the two endpoints can do authentication processing, then we can use IPSec in Transport Mode with AH headers. Conversely, if the endpoints cannot do their own authentication, we need to use IPSec in Tunnel Mode.

If we wish to use IPSec for confidentiality via the means of encryption, we will need to use ESP headers, with or without AH headers since the ESP headers can carry out authentication. Likewise, if the two endpoints are capable of security processing, then we can use IPSec in the Transport Mode, else Tunnel Mode.
