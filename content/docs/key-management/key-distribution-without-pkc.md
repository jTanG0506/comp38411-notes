---
title: Key Distribution without using PKC
markup: "mmark"
weight: 3
---

## Key Distribution without using PKC
Suppose a group of $$n$$ parties wish to establish pair-wise secure communication using symmetric-key cryptography. A naive solution would consist of each party exchanging a symmetric key with each of the other parties, so that we would require $$\frac{1}{2}n(n-1)$$ keys. Clearly, this solution does not scale for a large and distributed group, as the number of keys becomes untenable for everyone.

-----

### Key Distribution Centre (KDC)
Suppose that two parties $$A$$ and $$B$$ each have an encrypted connection to a third party $$C$$, then $$C$$ can deliver a key on the encrypted links to $$A$$ and $$B$$. Based on this idea, $$C$$ can be used a key distribution centre (KDC) who is responsible for distributing keys to pairs of users, and each party must share a unique master key with the KDC for purposes of key distribution. That is, if $$A$$ wishes to establish an encrypted connection to $$B$$, $$A$$ will request a session key from the KDC for communication with $$B$$.

![Key Distribution Centre](/docs/figures/kdc.png)

Each party shares a unique master key with the key distribution centre, which must be distributed securely. If we have $$n$$ parties which wish to communicate in pairs, then we need at most $$\frac{1}{2}n(n-1)$$ keys, as before. However, only $$n$$ master keys are required, one for each entity, and thus can be distributed in some non-cryptographic way, such as physical delivery.

Although a key distribution centre reduces the scale of the problem from $$n^2$$ to $$n$$, it has some major security flaws, by nature. The KDC is a single point of failure, for example, if the server containing the KDC crashes, then all types of secure communications become impossible to have. Also, as all the end-users will be using the KDC at peak times, the KDC could be a performance bottleneck, leading to very slow communications or a complete breakdown in communications. Also, the KDC has enough information to impersonate anyone to anyone, and anyone who has access to the KDC can decrypt any communication between any pair of parties.
