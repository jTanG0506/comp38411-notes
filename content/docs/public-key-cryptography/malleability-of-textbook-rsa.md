---
title: Malleability of Textbook RSA Encryption
markup: "mmark"
weight: 4
---

## Malleability of Textbook RSA Encryption
Textbook RSA encryption refers to the RSA encryption scheme wherein the plaintext contains only the message you want to encrypt. For example, given a string $$x = x_1 x_2 x_3$$, we first convert it to a number by concatenating the ASCII encodings of each of the three letters as a byte, say $$y_1 y_2 y_3$$, which we might then encrypt by computing $$(y_1 y_2 y_3)^e \; \text{mod} \; n$$. Without knowledge of the secret exponent $$d$$, there would be no way to decrypt the message.

However, textbook RSA encryption is deterministic, that is, encrypting the same plaintext twice will yield the same ciphertext twice. Also, given two textbook RSA ciphertexts $$y_1 \equiv x_1^e \; (\text{mod} \; n)$$ and $$y_2 \equiv x_1^e \; (\text{mod} \; n)$$, we can derive the ciphertext $$x_1 \times x_2$$ by multiplying the two separate ciphertexts together:

$$y_1 \times y_2 \equiv x_1^e \times x_2^e \equiv (x_1 \times x_2)^e \; (\text{mod} \; n)$$

Therefore an attacker can create a new valid ciphertext from two RSA ciphertexts, compromising the security of your encryption by allowing them to deduce information about the original message. We say that this weakness makes the textbook RSA encryption **malleable**.