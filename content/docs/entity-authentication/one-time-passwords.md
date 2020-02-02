---
title: One Time Passwords
markup: "mmark"
weight: 3
---

## One Time Passwords

**One Time Passwords** (OTP), as the name suggests, are passwords which are only used once. OTPs are safe from eavesdroppers and subsequent impersonation.

Some variations of OTP include:
1. **Shared list of one-time passwords**: The claimant and the verifier share a set of $$n$$ passwords, each valid for a single authentication. A variation of this involves the use of a *challenge-response table*, whereby the claimant and the verifier share a table of matching challenge-response pairs - note that this technique is not the cryptography challenge-response protocol.
1. **Sequentially updated one-time passwords**: The claimant and the verifier start by sharing a single secret. During authentication using password $$i$$, the claimant creates and transmits a new password $$(i + 1)$$ encrypted under a key derived from password $$i$$.
1. **OTP sequences based on a one-way function**: This method can be viewed as a challenge-response protocol where the challenge is implicitly defined by the current position within the password sequence. An example of this approach is Lamport's OTP scheme.

-----

### Lamport's OTP Scheme
In Lamport's OTP Scheme, the claimant begins with a secret $$w$$. Let $$H$$ be a one-way function, which defines the password sequence: $$w, H(w), H(H(w)), \dots, H^t(w)$$. The password for the $$i^{th}$$ identification session is defined to be $$w_i = H^{t - i}(w)$$.

1. **Setup**: The claimant $$A$$ begins with a secret $$w$$. Let $$H$$ be a one-way function. Fix $$t$$, the number of identifications allowed. $$A$$ transfers the initial shared secret $$w_0 = H^t(w)$$ to the verifier $$B$$, in a secure manner. $$B$$ initialises its counter for $$A$$ to $$i_A = 1$$.
1. **Identification**: For session $$i$$, $$A$$ computes $$w_i = H^{t - i}(w)$$, either from $$w$$ itself, or from an appropriate immediate value, and transmits this value to $$B$$. $$B$$ checks that $$i = i_A$$ and that the received password $$w_i$$ satisfies $$H(w_i) = w_{i - 1}$$. If both checks succeed, $$B$$ accepts the password, and increments $$i_A$$ by one and saves $$w_i$$ for the next session verification.

-----

### Hashing-Based OTP
Suppose the claimant and the verifier share a password $$P$$, and let $$H$$ be an agreed hash function. For authentication, the claimant would send $$r \; || \; H(r, P)$$ to the verifier, which computes the hash of the received value of $$r$$ and its local copy of $$P$$, and compares it against the received hash value. $$r$$ should be a time-variant parameter, such as a sequence number or timestamp, to avoid replay attacks.

-----

### Challenge-Handshake Authentication Protocol
Challenge-Handshake Authentication Protocol (CHAP) is an authentication scheme used by Point-to-Point Protocol (PPP) servers to validate the identity of remote clients.

![CHAP](/docs/figures/chap.png)

For this protocol, the server and user are assumed to have a shared symmetrical key $$K$$. To achieve authentication, the server sends the user a random number, which acts as the challenge message. The user concatenates the random number and the shared key $$K$$, computes the hash and sends it to the server. This hash proves that the user knows the value of $$K$$, without revealing the value of $$K$$ - this is known as a zero-knowledge-based protocol.
