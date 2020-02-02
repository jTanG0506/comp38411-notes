---
title: Modes of Operation
markup: "mmark"
weight: 5
---

## Modes of operation
In this section, we will discuss the main modes of operations used by block ciphers, their security and function properties.

-----

### Electronic Codebook (ECB) Mode
Electronic codebook mode is the simplest of block cipher encryption modes. ECB takes plaintext blocks $$P_1, P_2, \dots, P_N$$ and processes each one independently by $$C_1 = E(K, P_1), C_2 = E(K, P_2)$$, and so on. Although a simple operation, it is **extremely insecure**.

![ECB Mode](/docs/figures/ecb-mode.png)

As plaintext blocks are encrypted independently of other blocks, reordering plaintext blocks results in correspondingly reordered ciphertext blocks. This also means that ciphertext blocks can be cut from one message and pasted in another, so block replay, insert or deletion attacks may go undetected. If the same key is used, the same block of plaintext always produces the same ciphertext, meaning patterns of plaintext will show up in the ciphertext.

There is a famous illustration of ECB's security that uses an image of the Linux's mascot, where it is easy to see the penguin's shape because all the blocks of one shade of grey in the original message have been encrypted into the same new shade of grey in the new image, so ECB encryption simply gives the same image but with different colours.

-----

### Cipher Block Chaining (CBC) Mode
Cipher block chaining (CBC) is like ECB but with a small change that makes a big difference. Instead of encrypting the $$i$$-th block, $$P_i$$, as $$C_i = E(K, P_i)$$, we set $$C_i = E(K, P_i \oplus C_{i - 1})$$, where $$C_{i - 1}$$ is the previous ciphertext block, *chaining* the blocks $$C_{i - 1}$$ and $$C_i$$. However, when we encrypt the first block, $$P_1$$, there is no previous ciphertext block to use, so CBC takes a random initial value (IV). This IV is sent in the clear with the ciphertext, as it is required for decryption.

![CBC Mode](/docs/figures/cbc-mode.png)

In CBC, using two different values for $$IV$$ to encrypt the same plaintext will produce different ciphertexts. Unlike ECB mode, any repeated patterns in the plaintext are concealed by the feedback as ciphertext block $$C_j$$ depends on $$M_j$$ and all preceding plaintext blocks.

Decryption can be much faster than encryption due to parallelism. While encrypting a new block, $$P_i$$ needs to wait for the previous block, $$C_{i - 1}$$, decryption of a block computes $$P_i = D(K, C_i) \oplus C_{i - 1}$$, where there is no need for the previous plaintext block, $$P_{i - 1}$$.

-----

### Counter (CTR) Mode
In counter (CTR) mode, the block cipher algorithm does not transform the plaintext data. Instead, it will encrypt blocks composed of a **counter** and a **nonce**. A counter is an integer that is incremented by 1 for each subsequent block (modulo $$2^b$$, where $$b$$ is the block size) and is initially equal to the plaintext block size used. No two blocks should use the same counter within a message, but different messages can use the same sequence of counter values. A nonce is a number that is only used once. It is the same for all blocks in a single message, but no two messages should use the same nonce.

![CTR Mode](/docs/figures/ctr-mode.png)