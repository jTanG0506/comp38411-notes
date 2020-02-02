---
title: Symmetric Encryption
markup: "mmark"
weight: 1
---

**Cryptography** is the practice and study of techniques for secure communication through the use of codes so that only those for whom the information is intended can be read and process it. An original message is known as the **plaintext**, whereas the coded message is known as **ciphertext**. The process of converting from plaintext from ciphertext is known as **enciphering** or **encryption**, and restoring the plaintext from ciphertext is known as deciphering or **decryption**. The system that performs encryption and decryption is known as a **cryptographic system** or a **cipher**. Techniques used for deciphering a message without knowledge of the enciphering details is known as **cryptanalysis**.

-----

## Symmetric encryption
**Symmetric encryption**, also referred to as conventional encryption was the only type of encryption in use before the development of public-key encryption in the 1970s. In symmetric encryption, the key used to decrypt is the same as the key used to encrypt.

**Notation**: with plaintext $$P$$ and encryption key $$K$$ as input, a cipher $$E$$ will produce the ciphertext $$C$$, we denote this by $$C = E(K, P)$$. Similarly, when the cipher is in decryption mode, we will write $$P = D(K, C)$$.

![Encryption and Decryption](/docs/figures/encryption-and-decryption.png)

### Encryption techniques
There is a range of techniques used in encryption:
- substitution: replacing letters with other letters, for example, $$a$$ becomes $$b$$, this causes a *confusion* effect
- transposition / permutation: a rearrangement of letters, for example, $$abcd$$ becomes $$acdb$$, causing a *diffusion* effect
- the xor operator $$\oplus$$: logical operator which outputs true when exactly one of the inputs are true

Classical (historic) algorithms are based on substitutions and permutations. In modern ciphers, the substitution technique takes $$N$$ bits of input and outputs a different set of $$N$$ bits using a lookup table, called **S-Boxes**. For transpositions, modern ciphers permute $$N$$ bits using a lookup table, called **P-Boxes**.

-----

### Caesar Cipher
The Caesar Cipher is the simplest use of a **substitution cipher**, which encrypts a message by shifting each of the letters down three positions in the alphabet, wrapping back around to A if the shift reaches Z. The algorithm can be expressed as follows. For each plaintext letter $$p$$, substitute the ciphertext $$c = E(3, p) = p + 3 \; (\text{mod} \; 26)$$.

![Caesar Cipher](/docs/figures/caesar-cipher.png)

If it is known that a given ciphertext is a Caesar cipher, then a brute-force attack can be easily performed by simply trying all the 25 possible keys, and it is practical in this case as
- the encryption and decryption algorithms are in public domain
- there are only 25 keys to try
- the language of the plaintext is known and easily recognisable

Another technique we can use it break the Caesar Cipher is **frequency analysis**, which exploits the uneven distribution of letters in languages. For example, in English, E is the most common letter, so if it happens to be that Z is the most common letter in a ciphertext, then the most likely plaintext value at this position is E.

![Frequency Analysis](/docs/figures/frequency-analysis.png)
