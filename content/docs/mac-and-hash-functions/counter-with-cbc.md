---
title: Counter with CBC Mode
markup: "mmark"
weight: 7
---

## Cipher-Based Message Authentication Code
Cipher-Based Message Authentication Code (CMAC) is a MAC based on block ciphers. We first define the operation of CMAC when the message is an integer multiple $$n$$ of the cipher block length $$b$$. For AES, $$b = 128$$, and for triple DES, $$b = 64$$. First, the message is divided into $$n$$ blocks, $$(M_1, M_2, \dots, M_n)$$. The algorithm makes use of a $$k$$-bit encryption key $$K$$ and a $$b$$-bit constant, $$K_1$$. For AES, the key size $$k$$ is 128, 192, or 256 bits, and for triple DES, the key size is 112 bits or 168 bits.

![CMAC](/docs/figures/cmac.png)

CMAC is calculated as follows:

$$
\begin{aligned}
C_1 &= E(K, M_1)\\
C_2 &= E(K, M_2 \oplus C_1)\\
C_3 &= E(K, M_3 \oplus C_2)\\
& \vdots \\
C_n &= E(K, M_n \oplus C_{n-1} \oplus K_1)\\
T &= MSB_{Tlen}(C_n)
\end{aligned}
$$

where $$T$$ is the message authentication code, also known as the tag, $$Tlen$$ is the bit length of $$T$$, and $$MSB_s(X)$$ is the $$s$$ leftmost bits of the bit string $$X$$.

Suppose the message is not an integer multiple of the cipher block length, then the final block is padded to the right with a 1 and as many 0s as necessary to pad the final block to length $$b$$. The CMAC operation then proceeds as before, except we use a a different $$b$$-bit key $$K_2$$ instead of $$K_1$$.

-----

### Keys
The two $$b$$-bit keys derived from the $$k$$-bit encryption key as follows.

$$
\begin{aligned}
L &= E(K, 0^b)\\
K_1 &= L \cdot x\\
K_2 &= L \cdot x^2 = (L \cdot x) \cdot x\\
\end{aligned}
$$

where $$\cdot$$ is multiplication in the finite field $$GF(2^b)$$, and $$x$$ and $$x^2$$ are first- and second-order polynomials which are elements of $$GF(2^b)$$.

-----

### Authentication and Encryption
For authentication, the input, namely the nonce, associated data and plaintext, is formatted into a sequence of blocks $$B_0, B_1, \dots, B_r$$. $$B_0$$ contains the nonce along with some formatting bits which indicate the length of the elements $$N$$, $$A$$ and $$P$$. This is followed by zero or more blocks that contain $$A$$, followed by zero or more blocks that contain $$P$$. This sequence is then used as input to the CMAC algorithm, which produces a mac value with length $$Tlen \leq$$ block length.

For encryption, a sequence of counters $$Ctr_0, Ctr_1, \dots, Ctr_m$$ is generated, independent of the nonce. The authentication tag is encrypted in CTR using the counter $$Ctr_0$$, and the $$Tlen$$ most significant bits of the output is XORed with the tag to produce an encrypted tag. The remaining counters $$Ctr_1, \dots, Ctr_m$$ are used for the CTR mode encryption of the plaintext. The encrypted plaintext is concatenated with the encrypted tag to form the ciphertext output.

The authentication and encryption process is as follows:
1. Applying the formatting function to $$(N, A, P)$$ produce the blocks $$B_0, B_1, \dots, B_r$$
2. Let $$Y_0 = E(K, B_0)$$
3. For $$i \in [1, r]$$, let $$Y_i = E(K,  B_i \oplus Y_{i - 1})$$
4. Let $$T = MSB_{Tlen}(Y_r)$$
5. Apply the counter generation function to generate the counter blocks $$Ctr_0, Ctr_1, \dots, Ctr_m$$, where $$m = \lceil Plen / 128 \rceil$$
6. For $$j \in [0, m]$$, let $$S_j = E(K, Ctr_j)$$
7. Let $$S = S_1 \; || \; S_2 \; || \; \dots \; || S_m$$
8. Return $$C = (P \oplus MSB_{Plen}(S)) \; || \; (T \oplus MSB_{Tlen}(S_0))$$

![CCM](/docs/figures/ccm.png)

-----

### Decryption and Verification
For decryption and verification, the recipient requires the ciphertext $$C$$, the nonce $$N$$, the associated data $$A$$, the key $$K$$, and the initial counter $$Ctr_0$$.

The decryption and verification process is as follows:
1. If $$Clen \leq Tlen$$, return Invalid.
2. Apply the counter generation function to generate the counter blocks $$Ctr_0, Ctr_1, \dots, Ctr_m$$, where $$m = \lceil Plen / 128 \rceil$$
3. For $$j \in [0, m]$$, let $$S_j = E(K, Ctr_j)$$
4. Let $$S = S_1 \; || \; S_2 \; || \; \dots \; || S_m$$
5. Let $$P = MSB_{Clen - Tlen}(C) \oplus MSB_{Clen - Tlen}(S)$$
6. Let $$T = LSB_{Tlen}(C) \oplus MSB_{Tlen}(S_0)$$
7. Apply the formatting function to $$(N, A, P)$$ to produce the blocks $$B_0, B_1, \dots, B_r$$
8. Let $$Y_0 = E(K, B_0)$$
9. For $$i \in [1, r]$$, let $$Y_i = E(K, B_i \oplus Y_{i - 1})$$
10. If $$T \neq MSB_{Tlen}(Y)$$, then return Invalid, else return $$P$$

-----

### Drawbacks of CCM
CCM is naturally inefficient as we require two complete passes through the plaintext, once to generate the MAC value, and once for encryption. Besides, CCM is not parallelisable as the underlying authentication is based on CBC mode, which is not parallelisable nor pipelinable. Further, the specification details require a tradeoff between the length of the nonce and the length of the tag, which is an unnecessary restriction.
