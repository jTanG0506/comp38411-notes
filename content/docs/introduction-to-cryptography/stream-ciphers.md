---
title: Stream Ciphers
markup: "mmark"
weight: 4
---

## Stream ciphers
A **stream cipher** is one that encrypts a digital data stream one bit or one byte at a time. Stream ciphers generate pseudorandom bits, a bit like how the one-time pad works, as explained in the previous section. Stream ciphers take two input values, a key $$K$$ and a nonce $$N$$, the key should be secret and is usually 128 or 256 bits. A nonce is a number that number only used once, commonly referred to as the initial value ($$IV$$) in the context of stream ciphers, which does not need to be secret, but it should be unique for each key and is usually between 64 and 128 bits.

![Stream Cipher](/docs/figures/stream-cipher.png)

Stream ciphers produce a pseudorandom stream of bits called the **keystream**, denoted $$KS$$ in the diagram above. The keystream is XORed to plaintext to encrypt it and then XORed again to the ciphertext to decrypt it. In symbols, a stream cipher generates a keystream $$KS = SC(K, N)$$, encrypts as $$C = P \oplus KS$$ and decrypts as $$P = C \oplus KS$$.

Stream ciphers allow you to encrypt a message with key $$K_1$$ and nonce $$N_1$$ and then encrypt another message with key $$K_1$$ and nonce $$N_2 \neq N_1$$, or with $$K_2 \neq K_1 \neq N_1$$. Suppose we encrypted two messages with the same key $$K_1$$ and nonce $$N_1$$, meaning we used the same keystream. That means we could have $$C_1 = P_1 \oplus KS$$, $$C_2 = P_2 \oplus KS$$, which means we can determine $$P_2$$ if we know $$P_1$$ as

$$
\begin{aligned}
C_1 = P_1 \oplus KS & \implies KS = P_1 \oplus C_1\\
C_2 = P_2 \oplus KS & \implies C_2 = P_2 \oplus (P_1 \oplus C_1)\\
& \implies C_2 = P_2 \oplus P_1 \oplus C_1
\end{aligned}
$$