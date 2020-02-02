---
title: One Time Pad
markup: "mmark"
weight: 2
---

## One-time pad
A **one-time pad** is a cipher that guarantees **perfect secrecy**, even if an attacker has unlimited computing power, it is impossible to learn anything from the plaintext except for its length. The one-time pad takes a plaintext $$P$$ and a random key $$K$$, that is the same length as $P$ and produces a ciphertext $$C$$, defined as $$C = P \oplus K$$, where $$C$$, $$P$$ and $$K$$ are bit strings of the same length and $$\oplus$$ is the bitwise exclusive OR operator.

---

### Decrypting the one-time pad
Decrypting the one-time pad is identical to encryption, $$P = C \oplus K$$. $$C \oplus K = (P \oplus K) \oplus K = P \oplus K \oplus K = P$$

#### Example
If $$P = 01101101$$ and $$K = 10110100$$, then
- encryption: $$C = P \oplus K = 01101101 \oplus 10110100 = 11011001$$
- decryption: $$P = C \oplus K = 11011001 \oplus 10110100 = 01101101$$

---

### Practicality or perfect secrecy
The design of the one-time pad makes it inconvenient to use as it requires a key as long as the plaintext and a new random key for each new message. To encrypt a 1TB hard drive, you would need another 1TB hard drive to store the key. It is also inconvenient to store and distribute all these non-repeating keys, as well as to ensure synchronisation of keys between the sender and the receiver.
