---
title: Block Ciphers
markup: "mmark"
weight: 1
---

## Block ciphers
A block cipher consists of an algorithm and a decryption algorithm.
1. the **encryption algorithm** $$E$$, which takes a key $$K$$ and a plaintext block $$P$$ and produces a ciphertext block, $$C$$. We write this as $$C = E(K, P)$$.
2. the **decryption algorithm** $$D$$ is the inverse of the encryption algorithm and decrypts a ciphertext $$C$$ to the origin plaintext $$P$$. We write this as $$P = D(K, C)$$.

![Block Ciphers](/docs/figures/block-cipher.png)

-----

### Security goals
There are three main properties that an encryption algorithm should aim to satisfy:
1. completeness: every bit of the output should depend on every bit of the input and every bit of the key.
2. the avalanche effect: a desirable property of any encryption algorithm is that a small change (one bit, for example) in either the plaintext or the key should produce a significant change in the ciphertext.
3. statistical independence: attackers should be unable to discover any pattern in the input and output values of a block cipher, i.e. they appear to be statistically independent.

-----

### Block size
The block size and the key size are the two values which characterise a block cipher and security depends on both these values. Most block ciphers have either 64-bit or 128-bit blocks. For one thing, blocks must not be too large in order to minimise the length of the ciphertext and the memory footprint. With regard to the length of ciphertext, block ciphers process blocks, not bits. This means that in order to encrypt a 32-bit message when blocks are 128 bits, you will first need to convert the message into a 128-bit block, and only then will the block cipher be able to process it and return a 128-bit ciphertext. The wider the blocks, the longer this overhead.

As for the **memory footprint**, in order to process a 128-bit block, you need at least 128 bits of memory. This is small enough to fit into the registers of most CPUs; however, larger blocks can have a noticeable impact on the cost and performance of implementations.

-----

### Constructing block ciphers
A block cipher consists of a repetition of **rounds**, a short sequence of operations, known as **round functions**, that are weak on its own but strong in number. Two common techniques used to construct a round are substitution-permutation networks (as in AES) and Feistel schemes (as in DES).

Computing a block cipher boils down to computing a sequence of rounds. For example, a block cipher with three rounds encrypts a plaintext by computing $$C = R_3(R_2(R_1(P)))$$, where $$R_1, R_2$$ and $$R_3$$ are the rounds and $$P$$ is the plaintext. The round functions $$R_1, R_2$$, and so on - are usually identical algorithms but a value called the **round key** parameterises them. Two round functions with two distinct round keys will behave differently, and therefore will produce distinct outputs if fed with the same input.

Round keys are keys derived from the main key, $$K$$, using an algorithm called the **key schedule**. Note that round keys should always be different from each other in every round.

Recall that **confusion** refers to the input (plaintext and encryption key) undergoing complex transformations and that *diffusion* means that these transformations depend equally on all bits of input. For a block cipher, confusion and diffusion take the form of substitution and permutation operations. Substitution often appears in the form of **S-boxes**, or **substitution boxes**, which are small lookup tables that transform chunks of 4 or 8 bits.