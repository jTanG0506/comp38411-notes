---
title: Authenticated Ciphers
markup: "mmark"
weight: 6
---

## Authenticated Ciphers
Authenticated ciphers are similar to the cipher and MAC combinations; they work like normal ciphers except that they return an authentication tag together with the ciphertext. In symbols, authenticated cipher encryption is denoted $$AE(K, P) = (C, T)$$ and decryption is denoted $$AD(K, C, T) = P$$.

Authenticated decryption will return a plaintext $$P$$, given that the ciphertext and authentication tag are both valid; otherwise, it will return an error to prevent the processing of a possibly forged plaintext. Authenticated ciphers are fundamentally stronger than a basic cipher, as plaintexts are discarded if the tag is valid, meaning attackers cannot perform chosen-ciphertext attacks.

-----

### Predictability
In order to prevent an attacker from determining whether the same plaintext has been encrypted twice, we feed the cipher with an initial value $$IV$$ or a nonce, which is a value that is only used once. Thus, authenticated encryption can be expressed as $$AE(K, P, N)$$ and authenticated decryption can be expressed as $$AD(K, C, T, N) = P$$, where $$N$$ is the nonce.
