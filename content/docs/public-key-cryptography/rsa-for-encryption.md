---
title: Using RSA for encryption
markup: "mmark"
weight: 5
---

Typically, RSA is used in combination with a symmetric encryption scheme (such as AES), where RSA is used to encrypt a symmetric key which is then used to encrypt a message with a symmetric cipher, such as AES.

As discussed in the previous section, the naive textbook application of RSA is insecure, due to malleability. In order to make RSA ciphertexts nonmalleable, the ciphertext should consist of some additional data called *padding*, in addition to the message data. The standard way to encrypt with RSA in this fashion is to use Optimal Asymmetric Encryption Padding (OAEP). This scheme involves creating a bit string which is as large as the modulus by padding the message with extra data and randomness before applying the RSA function.