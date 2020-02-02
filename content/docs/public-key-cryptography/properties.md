---
title: Properties of Public Key Cryptography
markup: "mmark"
weight: 2
---

## Properties of public key cryptography
Public Key Cryptography (PKC) is based on a mathematical object called a trapdoor function, a function that maps a number $$x$$ to a number $$y$$, such that it is easy to compute $$y$$ from $$x$$ using the public key. However, computing $$x$$ from $$y$$ is tough unless you know the private key.

Think of $$x$$ as a plaintext and $$y$$ as a ciphertext then it is easy to compute $$C = f(K_E, P)$$ if the public key $$K_E$$ and the plaintext $$P$$ is known. It is also easy to compute $$P = f^{-1}(K_D, C)$$ if the private key $$K_D$$ and the ciphertext $$C$$ is known, however, it is infeasible to compute $$P = f^{-1}(K_D, C)$$ if we only know the ciphertext and not the private key.

Note that in public key cryptography, it should be computationally easy to generate public and private keys, and computationally easy to encrypt to decrypt given that the right key is known. Also, it should be computationally infeasible to compute the private key from the public key.
