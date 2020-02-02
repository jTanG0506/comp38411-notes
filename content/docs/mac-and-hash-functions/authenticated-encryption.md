---
title: Authenticated Encryption
markup: "mmark"
weight: 5
---

## Authenticated Encryption
Authenticated encryption (AE) algorithms simultaneously protect the confidentiality and authenticity of messages. There are four conventional approaches to providing both confidentiality and encryption for a message $$M$$.

-----

### Hash-then-encrypt
For **hash-then-encrypt**, we first compute the hash of the plaintext message, $$h = H(M)$$, then encrypt the message and the hash together, $$C = E(K, (M \; || \; h))$$. The sender transmits $$C$$, and upon receipt, the recipient first decrypts $$C$$ by computing $$M \; || \; h = D(K, C)$$. Next, the recipient computes $$H(M)$$ and verifies it is equal to the $$h$$ received.

-----

### MAC-then-encrypt
The **MAC-then-encrypt** approach computes an authentication tag $$T = MAC(K_2, P)$$ for a plaintext $$P$$. Then, it creates the ciphertext by encrypting the plaintext and the tag together, that is, $$C = E(K_1, P \; || \; T)$$. The sender transmits only $$C$$, as it contains both the encrypted plaintext and tag. Upon receipt, the recipient decrypts $$C$$ by computing $$P \; || \; T = D(K_1, C)$$ to obtain the plaintext $$P$$ and tag $$T$$. Next, the recipient computes the $$MAC(K_2, P)$$ and verifies it is equal to $$T$$ received.

-----

### Encrypt-then-MAC
The **encrypt-then-MAC** approach computes the ciphertext $$C = E(K_1, P)$$ and a tag based on the ciphertext, $$T = MAC(K_2, C)$$. The sender transmits both $$C$$ and $$T$$ to the intended recipient. Upon receipt, the recipient computes $$MAC(K_2, C)$$ and verifies that it is equal to the $$T$$ received. If so, the plaintext can be computed as $$P = D(K_1, C)$$.

-----

### Encrypt-and-MAC
The **encrypt-and-MAC** approach computes the ciphertext and the authentication tag separately. Given a plaintext $$P$$, the sender computes a ciphertext $$C = E(K_1, P)$$, and an authentication tag $$T = MAC(K_2, P)$$. Once the ciphertext and authentication tag has been generated, the sender transmits both to the intended recipient. Upon receipt, the recipient obtains the plaintext $$P = D(K_1, C)$$, then computes $$MAC(K_2, P)$$ using the decrypted plaintext and compares the result to the tag $$T$$ received.
