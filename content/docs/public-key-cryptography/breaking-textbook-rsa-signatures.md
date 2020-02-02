---
title: Breaking Textbook RSA Signatures
markup: "mmark"
weight: 6
---

## Breaking Textbook RSA Signatures
Textbook RSA signature refers to the method in which a message, $$x$$, is signed by directly computing $$y \equiv x^d \; \text{mod} \; n$$, where $$x \in [0, n - 1]$$. Similar to textbook encryption, textbook RSA signing is simple to implement but also insecure against several attacks.

-----

### Trivial forgery
One attack on textbook RSA signing involves trivial forgery. Notice that $$0^d \equiv 0 \; \text{mod} \; n$$, $$1^d \equiv 1 \; \text{mod} \; n$$ and $$(n - 1)^d \equiv 0 \; \text{mod} \; n - 1$$, so an attacker can forge signatures for $$0$$, $$1$$, or $$n - 1$$, without the knowledge of the private key $$d$$.

-----

### Blinding attack
Suppose Bob wants to get Alice's signature on a message, $$M$$, that Bob knows Alice would never knowingly sign. For this attack, Bob would first find some value, $$R$$, such that $$R^eM \; \text{mod} \; n$$ is a message that Alice would knowingly sign. Next, Bob would convince Alice to sign that message, so that Bob would know $$S \equiv (R^eM)^d \; \text{mod} \; n$$. Bob can now derive Alice's signature of $$M$$, namely $$M^d$$, with the knowledge of $$S$$. Recall that in RSA, by definition, we have $$A^{ed} \equiv A \; \text{mod} \; n$$, for $$A \in [0, n - 1]$$. It follows that:

$$S \equiv (R^eM)^d \equiv R^{ed}M^d \equiv RM^d \; \text{mod} \; n$$

Dividing the equation through by $$R$$ gives:

$$S/R \equiv RM^d/R \equiv M^d \; \text{mod} \; n$$

Hence, Bob can obtain Alice's signature on a message $$M$$, which Alice would never knowingly sign. As you can see, this is a worrying yet practical attack.

-----

### Full Domain Hash Signatures
Full Domain Hash (FDH) is a signature scheme in which the message is hashed to a number, $$x$$, and the signature is $$y = x^d \; \text{mod} \; n$$. To verify a signature $$y$$, we compute $$x = y^e \; \text{mod} \; n$$ and compare the result with the hash of the message.

![Full Domain Hash](/docs/figures/full-domain-hash.png)

-----

### RSA Probabilistic Signature Scheme
The RSA Probabilistic Signature Scheme (PSS) is to RSA what OAEP is to RSA encryption. It is a standard designed to make message signing more secure by using additional padding data. Like all public-key signature schemes, PSS works on a message's hash rather than on the message itself. PSS is more complicated to implement than FDH, but its security proof offers slightly higher security guarantees than the proof of FDH, due to its use of randomness.