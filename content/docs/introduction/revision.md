---
title: Revision
markup: "mmark"
weight: 1
---

## Revision
Two common mathematical notions in cryptography are **modulo arithmetic** and the **XOR operator**.

------

### The XOR Operator
In short, the **XOR** operator is a binary operation that outputs true only when its inputs differ. We will denote this operator by $$\oplus$$, also commonly denoted as $$\wedge$$ in other places.

#### Properties of XOR
There are four important properties of XOR that we will making use of in later sections.
- **commutative**: $$A \oplus B = B \oplus A$$
- **associative**: $$A \oplus (B \oplus C) = (A \oplus B) \oplus C$$
- **identity element**: $$A \oplus 0 = A$$
- **self-inverse**: $$A \oplus A = 0$$

------

### Modulo Arithmetic
For $$a, b \in \mathbb{Z}$$, $$n \in \mathbb{N}$$, we say that $$a$$ is congruent to $$b$$ modulo $$n$$ if and only if $$\exists k \in \mathbb{Z}$$ such that $$a - b = kn$$, and is denoted $$a \equiv b \; (\text{mod} \; n)$$.

#### Equivalence relation
The congruence relation is an equivalence relation as:
- reflexivity: $$a \equiv a \; (\text{mod} \; n)$$
- symmetry: $$a \equiv b \; (\text{mod} \; n) \implies b \equiv a \; (\text{mod} \; n)$$
- transitivity: $$a \equiv b \; (\text{mod} \; n), b \equiv c \; (\text{mod} \; n) \implies a \equiv c \; (\text{mod} \; n)$$

#### Inverses
If $$ax \equiv 1 \; (\text{mod} \; n), a, x \in \mathbb{Z}$$, then we say $$x$$ is the **multiplicative inverse** of $$a$$, denoted $$a^{-1}$$.

However, the existence of a multiplicative inverse does not always exist. A unique multiplicative of $$a$$ will exist in modulo $$n$$, if and only if, $$a$$ and $$n$$ are relatively prime, that is, $$gcd(a, n) = 1$$.

#### Euler's Totient Function
Euler's totient function counts the positive integers up to a given integer $$n$$ that are relatively prime to $$n$$, denoted $$\phi(n)$$. If $$n$$ is prime, then clearly all the integers from $$1$$ to $$n - 1$$ are relatively prime to $$n$$, thus $$\phi(n) = n - 1$$.

It follows that if we have two prime numbers, say $$p_1$$ and $$p_2$$, then we have, for $$n = p_1p_q, \phi(n) = \phi(p_1p_2) = (p_1 - 1)(p_2 - 1)$$.

#### Euler's Theorem
If $$a$$ and $$n$$ are coprime positive integers, then $$a^{\phi(n)} \equiv 1 \; (\text{mod} \; n)$$.