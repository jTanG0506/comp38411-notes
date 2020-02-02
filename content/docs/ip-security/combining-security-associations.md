---
title: Combining Security Associations
markup: "mmark"
weight: 7
---

## Combining Security Associations
An individual SA can implement either the AH or ESP protocol but not both. Sometimes, we may wish to employ multiple SAs on the same traffic flow to achieve some desired IPSec services. We refer to a sequence of SAs through which traffic must be processed to provide the desired set of IPSec services as a **security association bundle** (SA bundle).

Security Associations can be combined into bundles in two ways:
1. **Transport Adjacency**: Applying more than one SA to a single IP packet without invoking tunnelling. This approach only allows one level of combination, as further nesting yields no added benefit since the processing is performed at one IPSec instance, namely the end destination.
1. **Iterated Tunnelling**: Applying multiple layers of security protocols through IP tunnelling. This approach allows for multiple layers of nesting since each tunnel can originate and terminate at a different point along the path.

-----

### ESP with Authentication
As we have seen earlier, ESP can be applied to the data to be protected, and then we append the authentication data field. In transport mode, ESP provides authentication and encryption to the IP payload delivered to the host, but the IP header is not protected. In tunnel mode, ESP provides authentication to the entire IP packet delivered to the outer IP destination address, where the authentication is performed.

-----

### Transport Adjacency
For Transport Adjacency, we use two bundled transport SAs; the inner one being an ESP SA without authentication, and the outer being an AH SA. The advantage of this approach over using ESP with authentication is that the authentication covers more fields, namely the source and destination IP addresses. However, it brings an overhead of using two SAs instead of a single SA.

![Transport Adjacency](/docs/figures/transport-adjacency.png)

-----

### Transport-Tunnel Bundle
For a Transport-Tunnel Bundle, the idea is to apply authentication before encryption, which can be desirable as the authentication data is protected by encryption, meaning an attack cannot intercept the message and alter the authentication data without detection. In this approach, we use a bundle consists of an inner AH transport SA and an outer ESP tunnel SA.

![Transport Tunnel Bundle](/docs/figures/transport-tunnel-bundle.png)
