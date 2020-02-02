---
title: The RSA Algorithm
markup: "mmark"
weight: 3
---

## The RSA algorithm
The RSA scheme is a block cipher in which the plaintext and ciphertext are represented as integers in the range $$[0, n - 1]$$ for some large $$n$$, typically of 1024 bits (or 309 decimal digits). The RSA algorithm can be broken down into three steps: key generation, encryption, and decryption.

-----

### Key Generation
In RSA, the key pairs are generated as follows.
1. **Choose** two large prime numbers, $$p$$, $$q$$ with $$p \neq q$$
2. **Calculate** $$n = pq$$ and $$\varphi(n) = (p - 1)(q - 1)$$
3. **Choose** $$e \in \mathbb{Z}$$, $$gcd(\phi(n), e) = 1$$, $$e \in (1, \phi(n))$$
4. **Calculate** $$d \equiv e^{-1} \; (\text{mod} \; \varphi(n))$$

The private key consists of $$\{d, n\}$$ and the public key consists of $$\{e, n\}$$. We say that $$d$$ and $$e$$ are multiplicative inverses mod $$\varphi(n)$$, and we know that $$ed \equiv 1 \; (\text{mod} \; \varphi(n))$$. By our choice of $$e$$ being coprime to $$\varphi(n)$$, we must also have that $$d$$ is coprime to $$\varphi(n)$$.

Notice that finding $$\varphi(n)$$ for an RSA modulus $$n$$ is equivalent to breaking RSA, as the secret exponent $$d$$ can be derived from $$\varphi(n)$$ and $$e$$, by computing $$e$$'s inverse. Therefore $$p$$ and $$q$$ should also be kept secret, since knowing $$p$$ or $$q$$ allows us to compute $$\varphi(n) = (p - 1)(q - 1)$$.

-----

### Encryption
To encrypt a plaintext, we represent it as an integer $$M \in [0, n - 1]$$, then encrypt as follows:

$$C = M^e \; \text{mod} \; n$$

-----

### Decryption
Decryption is done as follows:

$$M = C^d \; \text{mod} \; n$$

-----

### Example of RSA
We will do an example of the RSA algorithm, using small prime numbers, for convenience.

- Suppose we choose $$p = 17$$ and $$q = 11$$
- We calculate $$n = pq = 17 \times 11 = 187$$ and $$\varphi(n) = (p - 1)(q - 1) = 16 \times 10 = 160$$
- We choose $$e = 7$$ and it is clear that $$gcd(7, 160) = 1$$
- We calculate $$d$$ such that $$de \equiv 1 \; \text{mod} \; n$$, so $$d = 23$$ as we have $$23 \times 7 = 161 = (1 \times 160) + 1$$
- Suppose we want to encrypt $$M = 88$$, then $$C = 88^7 \; \text{mod} \; 187$$. $$88^7 \equiv 11 \; \text{mod} \; 187$$ so $$C = 11$$
- Now do decrypt $$C = 11$$, we need to compute $$11^{23} \; \text{mod} \; 187$$, which as $$88$$, as expected

-----

### Proof of RSA
Euler's Theorem states that if $$a$$ and $$n$$ are coprime positive integers, then $$a^{\varphi(n)} \equiv 1 \; (\text{mod} \: n)$$.

Recall that in RSA, we have:
1. $$n = pq$$, where $$p$$ and $$q$$ are large primes
2. $$\varphi(n) = (p - 1)(q - 1)$$
3. $$e, d \in \mathbb{Z}$$ such that $$e \equiv d^{-1} \; (\text{mod} \: n)$$ and so $$ed = 1 + k\varphi(n)$$ for some $$k \in \mathbb{Z}$$

It follows that:

$$C^d \equiv (M^e)^d \equiv M^{1 + k\varphi(n)} \equiv M^1 \cdot (M^{\varphi(n)})^k \equiv M^1 \cdot 1^k \equiv M \; (\text{mod} \: n)$$
