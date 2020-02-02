---
title: HMAC
markup: "mmark"
weight: 4
---

## HMAC
The hash-based MAC (HMAC) construction allows us to build a MAC from a hash function. HMAC uses a hash function, $$H$$, to compute a MAC tag as follows:

$$HMAC(K, M) = H((K^+ \oplus opad) \; || \; H((K^+ \oplus ipad) \; || \; M)$$

where:
- $$opad$$ (outer padding) is the string $$(5c5c \dots5c)$$ that is as long as the block size of $$H$$
- $$ipad$$ (inner padding) is the string $$(36636\dots 36)$$ that is as long as the block size of $$H$$
- $$K^+$$ is the key padded out with 00 bytes to the block size of $$H$$

![HMAC](/docs/figures/HMAC.png)

In the diagram above, $$b$$ is the number of bits in a block, $$M_i$$ is the $$i$$-th block of the message $$M$$, $$L$$ is the number of blocks in $$M$$, $$IV$$ is the initial value input to the hash function and $$n$$ is the length of the digest produced by the hash function.

We can describe the algorithm as follows:
1. Expand zeros to the left of the key $$K$$ to create a $$b$$-bit string $$K^+$$
2. Concatenate $$K^+ \oplus ipad$$ and the message $$M$$
3. Apply the hash function to the result of Step 3
4. Concatenate $$K^+ \oplus opad$$ and the result of Step 4
5. Apply the hash function to the result of Step 4 to get the output

Notice that $$K^+ \oplus ipad$$ and $$K^+ \oplus opad$$ can be precomputed as they do not depend on the message $$M$$. Therefore these values only need to be computed initially and every time the key changes.
