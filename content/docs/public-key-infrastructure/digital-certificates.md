---
title: Digital Certificates
markup: "mmark"
weight: 2
---

## Digital Certificates
Digital certificates are electronic documents which can be used to prove the ownership of a public key. Digital certificates are digitally signed by a Certificate Authority, an entity which verifies the contents of the certificate.

-----

### Trust Models
One model of PKI is called the *web of trust*, in which the identity of a public key is attested to by someone you trust, such as a friend or a friend of a friend. This model is used in applications such as Pretty Good Privacy (PGP), where we usually know whom we are communicating with. However, it would not work as well for automated network applications and business processes.

Alternatively, we can use a more centralised trust model, such as X.509 certificates, which generate a strict hierarchy of trust rather than rely on directly trusting peers. This hierarchy is known as a *chain of trust* between certificates.
