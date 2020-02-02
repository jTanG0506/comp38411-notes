---
title: Galois Counter Mode
markup: "mmark"
weight: 8
---

## Galois Counter Mode
AES-GCM is the most widely used authenticated cipher, which as the name suggests, is based on the AES algorithm, and the Galois counter mode. GCM makes use of two functions: $$GHASH$$, which is a keyed hash function, and $$GCTR$$, which is essentially the CTR mode with the counters determined by a simple increment by one operation.

-----

### GHASH
$$GHASH_H(X)$$ takes two inputs; the hash key $$H$$ and a bit string $$X$$ such that $$len(X) = 128m$$ for some $$m \in \mathbb{Z}, m > 0$$, and produces a 128-bit MAC value. 

![GHASH](/docs/figures/GHASH.png)

The function can be described as follows.
1. Let $$X_1, X_2, \dots, X_m$$ denote the unique sequence of blocks such that $$X = X_1 \; || \; X_2 \; || \; \dots \; || \; X_M$$
1. Let $$Y_0$$ be a block of 128 zeros, denoted $$0^{128}$$
1. For $$i \in [1, m]$$, let $$Y_i = (Y_{i - i} \oplus X_i) \cdot H$$, where $$\cdot$$ is multiplication in $$GF(2^{128})$$
1. Then we have $$GHASH_H(X) = Y^m$$

The $$GHASH_H(X)$$ function can be expressed as

$$Y_m = X_1 \cdot H^m \oplus X_2 \cdot H^{m-1} \oplus \dots \oplus X_m \cdot H = \bigoplus_{i = 1}^m X_i \cdot H^{m - i + 1}$$

This formulation shows us that if we use the same hash key is to be used to authenticate multiple messages; then we can precalculate the values $$H^2, H^3, \dots, H_m$$ for use with each message to be authenticated. Then, the blocks of data to be authenticated $$(X_1, \dots, X_M)$$ can be processed in parallel, as the computations are independent of one another.

-----

### GCTR
$$GCTR_K(ICB, X)$$ is a variation of CTR mode which takes a secret key $$K$$ and a bit string of arbitrary length and returns a ciphertext $$Y$$ of the same bit length as $$X$$. $$ICB$$ is the initial counter block, which is chosen random, similar to the $$IV$$ we have seen before. 

![GCTR](/docs/figures/GCTR.png)

The function can be described as follows.
1. If $$X$$ is the empty string, return the empty string and we are done
1. Let $$n = \lceil len(X) / 128 \rceil$$, or $$ceil(len(X) / 128)$$
1. Let $$X_1, X_2, \dots, X_{n - 1}, X_n^*$$ denote the unique sequence of bit strings such that $$X = X_1 \; || \; X_2 \; || \; \dots \; || \; X_{n - 1} \; || \; X_n^* \quad \text{and} \quad X_1, X_2, \dots, X_{n - 1} \; \text{are complete 128-bit blocks}$$
1. Let $$CB_1 = ICB$$
1. For $$i \in [2, n]$$, let $$CB_i = inc_{32}(CB_{i - 1})$$, where $$inc_{32}(S)$$ is the function which increments the rightmost 32 bits of 1.  by $$1 \; mod \; 2^{32}$$, and the remaining bits are unchanged
1. For $$i \in [1, n - 1]$$, let $$Y_i = X_i \oplus E(K, CB_i)$$
1. Let $$Y_n^* = X_n^* \oplus MSB_{len(X^*)}(E(K, CB_n))$$
1. Let $$Y = Y_1 \; || \; Y_2 \; || \; \dots \; || \; Y_{n - 1} \; || \; Y_n^*$$
1. Return $$Y$$

For a given $$ICB$$, we can precalculate the values $$CB_2, CB_3, \dots, CB_n$$ beforehand. Notice that the encryptions can be done in parallel, $$Y_i = X_i \oplus E(K, CB_i)$$ and $$Y_n^* = X_n^* \oplus MSB_{len(X^*)}(E(K, CB_n))$$, where the encryption function is some block cipher with 128-bits block size.

-----

### Most and Least Significant Bits (MSB and LSB)
$$LSB_S(X) =$$ the bit string consisting of the $$s$$ right-most bits of the bit string $$X$$

$$MSB_S(X) =$$ the bit string consisting of the $$s$$ left-most bits of the bit string $$X$$


**Example**: Let $$X = 00100011$$, then $$LSB_6(X) = 100011$$ and $$MSB_6(X) = 001000$$

Observe that for a bit string $$X$$, and $$s \in \mathbb{Z}, 0 \leq s \leq len(X)$$, we have $$X = MSB_{len(X) - s}(X) \; || \; LSB_s(X)$$.

-----

### Incrementing Function
For a bit string $$X$$ and $$s \in [0, len(X)] \subseteq \mathbb{Z}$$, the $$s$$-bit incrementing function, is defined as

$$inc_s(X) = MSB_{len(X) - s}(X) \; || \; [(int(LSB_s(X)) + 1) \; mod 2^s]_s$$

That is, the $$s$$-bit incrementing functions increments the right-most $$s$$ bits of the string, regarded as the binary representation of an integer, modulo $$2^s$$; and the remaining, left-most $$len(X)-s$$ bits, remain unchanged.

-----

### Galois Counter
For Galois Counter, the input consists of a secret key $$K$$, an initialisation vector $$IV$$, a plaintext $$P$$ and additional authenticated data $$A$$. The notation $$[x]_s$$ means the $$s$$-bit binary representation of the non-negative integer $$x$$.

The overall authenticated encryption function can be defined as follows. 

1. Let $$H = E(K, 0^{128})$$
2. Define a block, $$J_0$$ as\\
$$
J_0 = \begin{cases} 
  IV \; || \; 0^{31} \; || \; 1 & \text{if} \; len(IV) = 96\\
  GHASH_H(IV \; || \; 0^{s+64} \; || \; [len(IV)]_{64}) & \text{if} \; len(IV) \neq 96
\end{cases} 
$$\\
where $$s = 128\lceil len(IV) / 128 \rceil - len(IV)$$

3. Let $$C = GCTR_K(inc_32(J_0), P)$$
4. Let $$u = 128\lceil len(C) / 128 \rceil - len(C)$$
5. Let $$v = 128\lceil len(A) / 128 \rceil - len(A)$$
6. Define a block, $$S = GHASH_H(A \; || \; 0^v \; || \; C \; || \; 0^u \; || \; [len(A)]_{64} \; || \; [len(C)]_{64})$$
7. Let $$T = MSB_t(GCTR_K(J_0, S))$$, where $$t$$ is the supported tag length
8. Return $$(C, T)$$

![Galois Counter](/docs/figures/galois-counter.png)

-----

### Benefits of GCM over CCM
GCM was designed to provide authenticated encryption for high-speed applications and is based on the CTR mode of operation and multiplication in a Galois field.  Encryption is done in a variant of CTR mode, which means that it can be pipelined and parallelised in hardware implementations. Authentication is done using multiplication in a Galois field instead of using a block chain cipher as this operation is much less computationally intensive and is easily implemented in hardware. Compared to CCM mode, we only require half the number of AES operations in GCM.
