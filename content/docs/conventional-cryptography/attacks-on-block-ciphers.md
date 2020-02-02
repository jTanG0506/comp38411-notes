---
title: Attacks of Block Ciphers
markup: "mmark"
weight: 7
---

## Attacks on block ciphers
Meet-in-the-middle attacks (not to be confused with man-in-the-middle attacks) and padding oracle attacks are two common techniques used to attack block ciphers.

-----

### Meet-in-the-middle attacks
Let us take double DES for example. Suppose we have $$P$$ and $$C=E(K_2, E(K_1, P))$$ with two unknown 56-bit keys, $$K_1$$ and $$K_2$$.

1. Build a key-value table with $$2^{56}$$ entries of $$E(K_1, P)$$, where $$E$$ is the DES encryption function, and $$K_1$$ is the value stored.
2. For all $$2^{56}$$ values of $$K_2$$, compute $$D(K_2, C)$$ and check whether the resulting value appears in the table as an index.
3. If this value is found as an index of the table, you take the corresponding $$K_1$$ from the table and verify that the $$(K_1, K_2)$$ found is indeed the correct one by checking it against other pairs of $$(P, C)$$.

![Meet in the middle](/docs/figures/meet-in-the-middle.png)

This attack allows us to recover $$K_1$$ and $$K_2$$ by performing at most $$2^{57}$$ instead of $$2^{112}$$ operations: step 1 encrypts $$2^{56}$$ blocks and then step 2 encrypts at most $$2^{56}$$ blocks, hence $$2^{56} + 2^{56} = 2^{57}$$ operations at most in total. 

Likewise, you can apply the man-in-the-middle attack to triple DES similarly, except that the third stage will to through all $$2^{112}$$ values of $$K_2$$ and $$K_3$$. Therefore, the whole attack succeeds after performing at most $$2^{112}$$ operations, meaning that triple DES only has 112-bit security despite having a 168-bit key.

-----

### Padding oracle attacks
Recall that padding fills the plaintext with extra bytes in order to fill a block. A *padding oracle* is a system that essentially returns a *success* or an *error* value depending on whether the padding in a CBC-encrypted message is valid. A padding oracle can be found in services on a remote host sending error messages when it receives malformed ciphertexts. A padding oracle attack makes use of this information by recording which inputs have valid padding and which do not, in order to decrypt chosen-ciphertext values.