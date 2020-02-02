---
title: Keyed Hashes
markup: "mmark"
weight: 3
---

## Keyed Hashes
One obvious way of producing a keyed hash function would be to use an *unkeyed* hash function with both a key and a message. One way to do this is to append the key to the message and apply the normal hash function, that is, $$Hash(M \; || \; K)$$. This is known as **secret-suffix** construction. 

However, secret-suffix construction is vulnerable. Suppose we have a collision of the form $$Hash(M_1) = Hash(M_2)$$, where $$M_1$$ and $$M_2$$ are two distinct messages, os possible different sizes. This implies that $$Hash(M_1 \; || \; K)$$ and $$Hash(M_2 \; || \; K)$$ will be equal too, as internally $$K$$ will be processed based on the data hashed previously, namely $$Hash(M_1)$$, equal to $$Hash(M_2)$$.

To exploit this property, an attack would merely:
- Find two colliding messages, $$M_1$$ and $$M_2$$
- Request the MAC tag of $$M_1$$, $$Hash(M_1 \; || \; K)$$
- Guess that $$Hash(M_2 \; || \; K)$$ is the same, thus forging a valid tag for $$M_2$$

Alternatively, we could use **secret-prefix** construction, which prepends the key to the start of the message; however, this approach is vulnerable to the length-extension attack.

---

### Comparison of Secure Hash Algorithm

Secure Hash Algorithm (SHA) is a family of NIST-approved cryptographic hash functions. SHA-1 is a successor to MD5, which is still used in many legacy applications but has been cracked in 2017, and as a result, NIST official withdrew its approval for applications that need to be compliant with U.S. Government standards.

|Algorithm| Max Message Size (bits) | Block Size (bits) | Word Size (bits) | Digest Size (bits) | Security (bits) |
|:-:|:-:|:-:|:-:|:-:|:-:|
|SHA-1  | $$2^{64}$$  | 512  | 32 | 160 | 80  |
SHA-256 | $$2^{64}$$  | 512  | 32 | 256 | 128 |
SHA-384 | $$2^{128}$$ | 1024 | 64 | 384 | 192 |
SHA-512 | $$2^{128}$$ | 1024 | 64 | 512 | 25  |

- The **maximum message size** is the upper bound on the size of the message that the algorithm can handle.
- The **block size** is the size of each bit block that the message is divided into.
- The **word size** is the size of the integral unit of operation for internal transformations.
- The **digest size** is the size of the digest produced.
- The **security** refers to the number of messages that would need to be generated before two can be found with the same digest with a probability of 0.5.

We have seen that, for a secure digest function that produces an $$n$$-bit digest, one would need to generate $$2^{n/2}$$ messages in order to discover a collision with a probability of 0.5. As a result, the entries in the **Security** column is half of the corresponding entry in the **Digest Size** column.
