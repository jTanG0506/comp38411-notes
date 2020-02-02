---
title: Digital Signature Algorithm
markup: "mmark"
weight: 1
---

### Digital Signature Algorithm

The idea of digital signatures is to prove that the holder of the private key tied to a particular digital signature signed the message and that the signature is authentic. Digital signatures should be **message-dependent** so that signatures are non-reusable (thus ensuring integrity), **signer-dependent** so that signatures are unforgeable (thus ensuring authenticity) and **verifiable** so that others can verify that the signature is indeed from the originator.

Since only the private key holder knows the private key, nobody can compute a signature for some message using their private key, but everyone can verify the signature since everyone has access to their public key. This verified signature can be used to demonstrate that the private key holder did indeed sign a particular message, providing a property of undeniability, called nonrepudiation.

Digital signatures are intended to prevent forgeries rather than provide confidentiality. For example, a scheme that reveals parts of the messages could be a secure signature scheme, but not a secure encryption scheme. Due to the processing overhead required, public-key encryption can only process short messages in practice, which are usually secret keys rather than actual messages. However, a signature scheme can process messages of arbitrary sizes by using their hash values as a proxy, and it can be deterministic yet secure.

A digital signature scheme consists of:
- a key generation algorithm
- a signature generation / signing algorithm
- a signature verification algorithm

-----

## Digital Signature Algorithm (DSA)
Digital Signature Algorithm (DSA) is a signature algorithm based on discrete logarithms which can be used to provide digital signatures. Unlike RSA, it cannot be used for encryption or key exchange.

-----

### Key Generation
We need to generate $$p$$, $$q$$ and $$g$$, which make up the global public key.

- we choose $$p$$ to be a prime number with $$2^{L-1} < p <2^L$$, for $$L \in [512, 1024]$$ and $$64 \; \vert \; L$$, that is, a prime with bit length $$L$$ between 512 and 1024, in increments of 64 bits
- we choose $$q$$ to be a prime divisor of $$(p - 1)$$, where $$2^{N-1} < q < 2^N$$, that is, bit length of $$N$$ bits
- we calculate $$g = h^{(p - 1)/q} \; mod \; p$$, where $$h \in (1, p - 1) \subseteq \mathbb{Z}$$, such that $$h^{(p-1)/q} \; mod \; p > 1$$\\

Next, we need to generate a pair of keys, $$x$$ and $$y$$, which are the user's public and private keys.

- $$x$$ is the user's long term private key, a (psuedo) random integer with $$x \in (0, q)$$
- $$y$$ is the user's long term public key, $$y = g^x \; mod \; p$$

-----

### Signing
Suppose we wish to sign the message $$M$$.

- we choose a random number, $$k$$, known as the ephemeral key, with $$k \in (0, q)$$ and $$k$$ must be destroyed after use and never be reused.
- we compute $$r = (g^k \; mod \; p) \; mod \; q$$
- we compute $$s = [k^{-1} (H(M) + xr)] \; mod \; q$$

Now, $$(r, s)$$ makes up the signature and we send $$M \; \vert \vert \; (r, s)$$ to the recipient.

-----

### Verification
Suppose the recipient has received $$(M', r', s')$$ and knows the global public key $$(p, q, g)$$ as well as the senders public key $$y$$.

- we compute the hash of the received message, $$H(M')$$
- calculate the multiplicative inverse of $$s'$$, $$w = (s')^{-1} \; mod \; q$$
- calculate $$u_1 = (H(M')w) \; mod \; q$$
- calculate $$u_2 = (r')w \; mod \; q$$
- calculate $$v = [g^{u_1}y^{u_2} \; mod \; p] \; mod \; q$$

The recipient then checks if $$v = r'$$, and if that is the case, then the signature is verified.

-----

### Worked Example of DSA

#### Key Generation
First, we need to generate $$p$$, $$q$$ and $$g$$. Suppose we generate $$p = 53$$, $$q = 13$$ and $$g = 16$$. Now we generate a long term private key $$x \in (0, 13)$$, say $$x = 3$$, and compute a long term public key $$y = g^x \; mod \; p = 16^3 \; mod \; 53 = 15$$.

#### Signature Generation
Suppose we want to sign a message $$M$$ such that $$H(M) = 5$$, and we choose $$k = 2$$.
- $$r = (g^k \; mod \; p) \; mod \; q = (16^2 \; mod \; 53) \; mod \; 13 = 5$$
- $$s = [k^{-1} (H(M) + xr)] \; mod \; q = [2^{-1} (5 + 3 \cdot 5)] \; mod \; 13 = 10$$

Note that $$2^{-1}$$ refers to the multiplicative inverse of $$2$$ modulo $$13$$, which is 7.

#### Signature Verification
Suppose we have received $$(M', 5, 10)$$ and we wish to verify the signature.
- calculate the hash of the message, $$H(M') = 5$$
- calculate the multiplicative inverse of $$s'$$, $$w = (s')^{-1} \; mod \; q = (10)^{-1} \; mod \; 13 = 4$$
- calculate $$u_1 = [H(M')w] \; mod \; q = [5 \cdot 4] \; mod \; 13 = 7$$ 
- calculate $$u_2 = (r')w \; mod \; q = [5 \cdot 4] \; mod \; 13 = 7$$
- calculate $$v = [g^{u1}y^{u2} \; mod \; p] \; mod \; q = [(16^7 \cdot 15^7) \; mod \; 53] \; mod \; 13 = 5$$

Observe that we have $$v = r'$$, hence the signature is verified.
