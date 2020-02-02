---
title: Digest Functions
markup: "mmark"
weight: 1
---

## Digest functions
Digest functions, also known as hash functions, takes an input of any length and produces a short, fixed sized output, known as the **digest** or **hash value**. Digest functions are used everywhere in the world: Git's revision control system uses them to identify files in a repository; forensics use hash values to prove that digital artefacts have not been modified - and many more.

![Digest Function](/docs/figures/digest-function.png)

As digest functions have a compression property, this function is a many-to-one mapping, so collisions are unavoidable. However, it should be difficult to find collisions in a good digest function. We will look at what it means for a hash function to be cryptographically secure by introducing two notions - namely, collision resistance and preimage resistance.

-----

### Preimage Resistance
A **preimage** of a given hash value, $$h$$, is any message, $$m$$, such that Hash($$m$$) = $$h$$. Digest functions are sometimes referred to as **one-way functions** as it is easy to compute the hash from any message, but impossible to go the other way.

Suppose I have some message $$m$$, such that Hash($$m$$) = $$h$$. Even given unlimited computing power, you would never be able to determine the $$m$$ that I used to produce this particular hash since many messages are hashing to the same value. You would be able to find messages that produced this hash value, possibly including the one I picked, but would be unable to determine the messaged I used.

In practice, we must be sure that it is practically impossible to find any message that maps to a given hash value, nor just the messaged that was used for, which is what the notion of preimage resistance is.

**First preimage resistance** (or just preimage resistance), also known as the one-way property, refers to the case where is it practically impossible to find a message that hashes to a given value. That is, for any given $$h$$, it is difficult to find $$m$$ such that Hash($$m$$) = $$h$$.

**Second preimage resistance**, also known as weak collision resistance, on the other hand, refers to the case that given a message, $$m_1$$, it is practically impossible to find another message, $$m_2$$, that hashes to the same value as $$m_1$$. That is, it is difficult to find $$m_1 \neq m_2$$, such that Hash($$m_1$$) = Hash($$m_2$$).

-----

### Collision Resistance
As a given hash function always produces a fixed-length output, collisions will inevitably exist due to the **pigeonhole principle**, which states that if you have $$m$$ holes and $$n$$ pigeons to put into those holes, and $$n > m$$, then at least one hole must contain more than one pigeon. More precisely, if you use $$n$$ bits to represent the digest, there are only $$2^n$$ distinct values for the digest.

Despite the inevitable, it should be hard to find two distinct messages, $$m_1 \neq m_2$$, such that Hash($$m_1$$) = Hash($$m_2$$). If a hash function satisfies this condition, then we say that it is **strong collision resistant**, or just collision resistant.

-----

### Remarks
For any given hash function $$H$$:
- if $$H$$ is collision resistant, then $$H$$ is also second preimage resistant
- if $$H$$ is second preimage resistant, then $$H$$ is also preimage resistant

#### Proof
We will use proof by contrapositive. As a reminder, this means that proving $$(\neg q \rightarrow \neg p)$$ is equivalent to proving $$(p \rightarrow q)$$.
- Suppose that $$H$$ is not second preimage resistant and we need to show $$H$$ is not collision resistant. We can choose a random message $$m_1$$, and since $$H$$ is not second preimage resistant, we can find $$m_2 \neq m_1$$ such that $$H(m_1) = H(m_2)$$. Then, we have found two distinct messages that produce the same hash, hence a collision.
- Suppose $$H$$ is not preimage resistant and we need to show that $$H$$ is not second preimage resistant. Suppose for a given message $$m_1$$, we have $$H(m_1) = h$$. Since $$H$$ is not preimage resistant, we can find values $$m_2$$ such that $$H(m_2) = h$$, so we can choose a $$m_2 \neq m_1$$. Then, we have found a $$m_2 \neq m_1$$ such that $$H(m_1) = H(m_2)$$, and so $$H$$ is not second preimage resistant.

-----

### Cryptographically insecure digest functions
Suppose $$H$$ is a hash function that is not weak collision resistant. Now, if Alice was to send a signed message to Bob, namely $$m || s$$, where $$s = E(PR_a, H(m))$$ and $$PR_a$$ is Alice's private key. An attack would be capable of intercept the message plus the encrypted hash code, $$m || s$$, and since $$H$$ is not weak collision resistant, the attack can find another message $$m'$$ such that $$H(m) = H(m')$$. Hence, the attacker has forged Alice's signature $$s$$ on the message $$m'$$.

Now suppose instead, $$H$$ is not strong collision resistance. Again, assume Alice was to send a signed message to Bob. First, Alice would find two messages with the same hash, i.e. $$m_1 \neq m_2$$ such that $$H(m_1) = H(m_2)$$. Alice then sends Bob one of the messages to sign, say $$m_1$$, and now Alice can repudiate this signature, and claim that Bob signed the message $$m_2$$.
