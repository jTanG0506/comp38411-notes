---
title: Diffie-Hellman Protocol
markup: "mmark"
weight: 2
---

## Diffie-Hellman

Diffie-Hellman was the first public-key exchange protocol invented, a protocol that allows two parties to establish a shared secret by exchanging information visible to an eavesdropper. Prior to Diffie-Hellman, establishing a shared secret required inconvenient procedures such as physically exchanging sealed envelopes containing the secrets.

-----

### Diffie-Hellman Function
The Diffie-Hellman function usually works with groups $$\mathbb{Z}_p$$, that is, the set of positive integer numbers, along with multiplication modulo $$p$$ as the group operation. The Diffie-Hellman function also requires a public parameter, $$g$$, known as the base number.

The Diffie-Hellman function involves two private values chosen randomly from the group $$\mathbb{Z}_p$$ by the two communicating parties, denoted $$a$$ and $$b$$. The public value associated with $$a$$ is $$A = g^a \; mod \; p$$ and is sent to the other party through a publicly readable message. Likewise, the public value associated with $$b$$ is $$B = g^b \; mod \; p$$, which is sent to the owner of $$a$$ through a publicly readable message.

The **shared secret** can now be computed by combining either public value with the other private value, as follows:

$$A^b = (g^a)^b = g^{ab} = g^{ba} = (g^b)^a = B^a$$

The value $$g^{ab}$$ is the shared secret, which can then be passed to a *key derivation function* in order to one or more shared generate symmetric keys. A key derivation function is a hash function that will return a random-looking string of the size of the desired key length.

-----

### Diffie-Hellman Protocol
The Diffie-Hellman Protocol is the first published public-key algorithm, which allows two parties that have never met to securely exchange keys that can be used for subsequent encryption of messages.

The security of Diffie-Hellman protocols relies on the difficulty of the discrete logarithm problem in a finite field. This is as Diffie-Hellman can be broken if we are able to recover the private value $$a$$ from its public value $$g^a$$, which is reducible to solving a discrete logarithm problem.

------

### Anonymous Diffie-Hellman
Anonymous Diffie-Hellman is the simplest of the Diffie-Hellman protocols, where each party picks a random value to use as a private key, say $$a$$ for Alice and $$b$$ for Bob, and sends the corresponding public key to the other peer.

![Anonymous Diffie Hellman](/docs/figures/anonymous-diffie-hellman.png)

The anonymous Diffie-Hellman protocol works as follows:

1.  Alice and Bob both pick a random exponent, $$a$$ and $$b$$
1.  Alice uses her exponent $$a$$ and the base number $$g$$ to compute $$A = g^a$$ and sends this to Bob
1.  Bob receives $$A$$ and computes $$A^b$$ which is equal to $$g^{ab}$$
1.  Bob uses his exponent $$b$$ and the base number $$g$$ to compute $$B = g^b$$ and sends this to Alice
1.  Alice receives $$B$$ and computes $$B^a$$ which is equal to $$g^{ab}$$

This protocol is called anonymous because it is not authenticated, as the participants have no identity that can be verified by either party, and neither party holds a long-term key. That is, Alice cannot prove to Bob that she is Alice, and vice versa.

From the diagram below, we see that anonymous Diffie-Hellman is vulnerable to man-in-the-middle attacks, where an eavesdropper needs to intercept messages to make Alice and Bob believe that they shared keys with one another. However, they shared keys with the attacker, and the attacker can intercept and read any messages encrypted with both Alice and Bob unaware.

![Anonymous Diffie Hellman Attack](/docs/figures/anonymous-diffie-hellman-attack.png)

As normal, Alice and Bob pick random exponents, $$a$$ and $$b$$, and Alice computes and sends $$A$$ to Bob. The attacker intercepts this message, and then picks a random exponent $$c$$ and computes $$C = g^c$$ to send to Bob, instead of $$A$$. Since this protocol has no authentication, Bob believes that $$C$$ is indeed sent from Alice and proceeds to compute $$g^{bc}$$. Bob then computes and sends $$B$$ to Alice, but again, the attacker intercepts the message and picks another random exponent $$d$$, computes $$D = g^d$$ and sends this to Alice. Alice then computes $$g^{ad}$$ as well.

As a result, the attacker ends up using sharing the secret $$g^{ad}$$ with Alice, and another secret $$g^{bc}$$ with Bob, though Alice and Bob believe that they are sharing a single secret with each other. This means the attacker is able to intercept the encrypted messages, decrypt them, modify the cleartext and encrypt them before sending them on - all without Alice and Bob knowing.

-----

### Authenticated Diffie-Hellman
For authenticated Diffie-Hellman, both parties are equipped with both a private and a public key, allowing the parties to sign their messages in order to stop an attacker sending messages on their behalf. The signatures are computed using a public-key signature scheme such as RSA, so if an attacker wants to send a message on behalf of one of the parties, the attacker would need to forge a valid signature.

Suppose Alice and Bob wish to establish a secret using authenticated Diffie-Hellman. Alice has a private key, $$priv_A$$, as well as Bob's public key, $$pub_B$$. We say that this type of $$priv/pub$$ key pair is *long-term* as it is fixed in advance and remains constant through subsequent runs of the protocol. The long-term private keys should be kept a secret, while the public keys are considered to be known to an attacker.

![Authenticated Diffie Hellman](/docs/figures/authenticated-diffie-hellman.png)

Authenticated Diffie-Hellman works as follows:

1. Alice and Bob both pick a random exponent, $$a$$ and $$b$$
1. Alice calculates $$A$$ and a signature $$sig_A$$ using her private key $$priv_A$$ and sends both $$A$$ and $$sig_A$$ to Bob
1. Bob receives ($$A$$, $$sig_A$$) and verifies $$sig_A$$ using $$pub_A$$. If the signature is invalid, then he knows the message did not come from Alice, so he discards $$A$$
1. If the signature is verified, then Bob will compute $$g^{ab}$$ from $$A$$ and his random exponent $$b$$
1. Bob calculates $$B$$ and a signature $$sig_B$$ using his private key $$priv_B$$ and sends both $$B$$ and $$sig_B$$ to Alice
1. Alice receives ($$B$$, $$sig_B$$) and verifies $$sig_B$$ using $$pub_B$$ and only computes $$g^{ab}$$ if she can verify Bob's signature