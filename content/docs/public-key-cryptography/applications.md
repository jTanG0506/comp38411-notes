---
title: Applications of Public Key Cryptography
markup: "mmark"
weight: 1
---

## Introduction to Public Key Cryptography

Public key encryption (or asymmetric encryption) makes use of two keys:
- **a public key**, which can be used by anyone who would like to encrypt a message for you
- **a private key**, which is used to decrypt messages encrypted using your public key

![Public Key Cryptography](/docs/figures/public-key-cryptography.png)

In the diagram above, we have a pair of asymmetric key, $$K_E$$ and $$K_D$$, we encrypt a plaintext $$C = E(K_E, P)$$ and decrypt a ciphertext $$P = D(K_D, C)$$. In this case, $$K_E$$ is the public key, and $$K_D$$ is the private key, which should be kept secret.

-----

## Applications of public key cryptography
The three main categories of applications of public key cryptosystems are:
1. **encryption / decryption**: the sender encrypts a message with the recipient's public key, which the recipient can decrypt using their private key
2. **digital signatures**: the sender *signs* a message with their private key, so the recipient can use the senders public key to verify that the sender of the message is indeed whom they claim they are
3. **key exchange**: two parties exchange a session key, which is a secret key for symmetric encryption generated for use for a particular session and valid for a short period

$$
\begin{array}{|l|c|c|c|}
\hline
\textbf{Algorithm}    & \text{Encryption / Decryption} & \text{Digital Signatures} & \text{Key Exchange} \\ \hline
\text{RSA}            & \text{Yes}                     & \text{Yes}                & \text{Yes}          \\ \hline
\text{Elliptic Curve} & \text{Yes}                     & \text{Yes}                & \text{Yes}          \\ \hline
\text{Diffie-Hellman} & \text{No}                      & \text{No}                 & \text{Yes}          \\ \hline
\text{DSS}            & \text{No}                      & \text{Yes}                & \text{No}           \\ \hline
\end{array}
$$

-----

### Encryption and decryption
Suppose Alice wishes to send a confidential message to Bob. Alice encrypts the message using Bob's public key, and upon receipt, Bob decrypts it using his private key. Only Bob can decrypt the message as only Bob owns his private key. With this approach, all participants have access to each other's public keys, usually stored in a public register or some accessible file.

![PKC Encryption and Decryption](/docs/figures/pkc-encryption-decryption.png)

-----

### Digital signatures
Suppose a message has a digital signature attached to it, then we can know that the holder of the private key for this signature signed the message and that the signature is authentic. As the owner of the private key is the only one who can produce a message with their digital signature, anyone can use their public key to verify the validity of the signature. Therefore, a digital signature acts as a verified signature by the private key holder, which is a property of undeniability called *non-repudiation*. 

![Digital Signatures](/docs/figures/digital-signatures.png)

Note that usually, the signature is signed on the hash of the message $$M$$, and should include a timestamp. The message cannot be altered without the senders private key, so the message is authenticated in terms of source and data integrity.

-----

### Double use of public key encryption
If Alice wants to send a message to Bob and she encrypts it with Bob's public key, Bob can decrypt it using his private key, but he cannot be sure that the origin of the message was Alice. Suppose Alice sends a message encrypted using her private key, then anyone could decrypt this message, meaning there is no protection of confidentiality.

However, we can provide both authentication and confidentiality by using the public-key scheme twice, as follows:

$$
\begin{aligned}
C & = E(PU_B, E(PR_A, P))\\
P & = D(PU_A, D(PR_B, C))
\end{aligned}
$$

First, we encrypt a message using the sender's private key, which provides a digital signature, then encrypt the result using the receiver's public key. The resulting ciphertext can be decrypted only by the intended receiver; thus, confidentiality is provided. However, the downside is that the public-key algorithm, which is complex, is required four times rather than two in each communication.

![Double PKC](/docs/figures/double-pkc.png)

Double use of public key encryption could be used to carry out a session key exchange between two parties. This session key is then typically used as a secret key for symmetric encryption for a particular transaction or session, and usually has a short lifetime.
