---
title: Message Authentication Code
markup: "mmark"
weight: 2
---

## Message Authentication Code
A Message Authentication Code (MAC) is a function which protects the integrity and authenticity of a message $$M$$, by producing a fixed-length value $$T = MAC(K, M)$$, called the authentication tag. The value $$K$$ is a shared secret key between the communicating parties.

![MAC-In-Operation](/docs/figures/mac-in-operation.png)

-----

### Worked Example of MAC
Suppose Alice wants to send a message $$M$$ to Bob. First, they would need to share a common secret key $$K$$, then Alice would send the message $$M$$, along with its authentication tag, $$T = MAC(K, M)$$ to Bob. Upon receiving the message, Bob computes $$T' = MAC(K, M)$$ and checks that it is equal to the authentication tag received, i.e. $$T' = T$$. Since Alice and Bob are the only people that know the secret key, so if the authentication tags match, then:
- Bob is assured that the message has not been altered (either accidentally or maliciously) in transit, confirming the **integrity** of the message
- Bob is assured that the message did indeed come from Alice, confirming the **authenticity** of the message

-----

### Fresh messages
Suppose an attacker was to eavesdrop on Alice and Bob's communication and was able to capture a message and its tag sent by Alice to Bob, and later send them again to Bob pretending to be Alice. This is a replay attack, and we say that a message is fresh if it is not a reply.

To achieve fresh messages, we can include a sequence number for each message, which is incremented for each new message and authenticated along with the message. Along with a timestamp and a random session key (contributed partially or entirely by the recipient), the recipient can be assured that the messages are in proper sequence and that the message is fresh.

-----

### Security of MAC
By the birthday attack, for an $$n$$-bit hash coding algorithm, the cost of finding a collision is $$2^{n / 2}$$. That is, a pool of $$2^{n/2}$$ random generated messages will contain at least one pair of messages with the same digest with a probability of 0.5. This attack is called the birthday attack as the combinatorial issues involved are the same as in the birthday paradox. As a standard, we should use at least 160-bits for a MAC. It is worth mentioning that a MAC is not a digital signature, so it does not provide non-repudiation.

#### Birthday Paradox
The birthday paradox states that given a group of 23 or more randomly chosen people, the probability that at least two of them will have the same birthday is more than 50%.
