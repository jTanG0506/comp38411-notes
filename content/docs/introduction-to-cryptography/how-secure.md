---
title: How Secure?
markup: "mmark"
weight: 5
---

## How secure?
### Unconditional security
An encryption scheme is said to be **unconditionally secure** if the ciphertext generated by the scheme does not contain enough information to determine the corresponding plaintext uniquely, no matter how much is available. In other words, it is impossible to defeat the system regardless of how much time and computational power is available, simply because the required information is not there.

-----

### Computational security
The only known scheme to be unconditionally secure is the one-time pad, which is impractical in most applications. As a result, one can strive for an algorithm that meets one or both of the following criteria:
- the cost of breaking the cipher exceeds the value of the encrypted information
- the time required to break the cipher exceeds the useful lifetime of the information

An encryption scheme is said to be **computationally secure** if either of the above criteria is met. However, it is difficult to estimate the amount of effort required to cryptanalyse ciphertext successfully.

-----

### Provable security
**Provable security** is about proving that breaking the cryptographic system is at least as difficult as solving another problem known to be hard, such as integer factorisation. Such security proof guarantees that the crypto remains safe as long as the hard problem remains hard. We say that breaking some cipher is **reducible** to problem $$X$$ if any method used to solve problem $$X$$ also yields a method to break the cipher.

-----

### Ad hoc security
**Ad hoc security**, sometimes called **heuristic security**, is where the cryptographic system is not provably secure, and the only reason to trust it is that many skilled people tried to break it and failed. As a result, the claims of *security* generally remain questionable and unforeseen attacks remain a threat. For example, we use AES to communicate using our mobile phones securely, but AES is not provably secure.