---
title: Breaking Textbook RSA Signatures
markup: "mmark"
weight: 7
---

## Hybrid cryptosystems
Symmetric and asymmetric ciphers each have their advantages and disadvantages. Asymmetric ciphers such as public key ciphers, are significantly slower than symmetric ciphers. For example, DES is around 1000 faster than RSA in hardware, and 100 times faster in software. With symmetric ciphers, all involved parties have to exchange the key used to encrypt the data before they can decrypt it, and cannot provide non-repudiation without the involvement of a trusted third party.

A hybrid cryptosystem is a combination of asymmetric and symmetric encryption to benefit from the strengths of each form of encryption. We use the public key cipher for establishing and transformation the symmetric key, which is then used by the symmetric cipher for bulk encryption.

![Hybrid Cryptosystem](/docs/figures/hybrid-cryptosystem.png)

A digital envelope consists of two pieces of information:
- a randomly generated symmetric key, known as the bulk encryption key, which is encrypted with the asymmetric key cipher using the recipients public key
- the plaintext message encrypted with the symmetric key cipher using the bulk encryption key

A digital envelope is decrypted by first using the recipient's private key to decrypt the secret key for symmetric key cipher, which is then used to decrypt the message to recover the plaintext.

An example of a digital envelope is Pretty Good Privacy (PGP), a cryptography software that provides cryptographic privacy and data communication authentication.