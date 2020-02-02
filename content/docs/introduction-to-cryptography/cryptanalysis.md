---
title: Cryptanalysis
markup: "mmark"
weight: 6
---

## Cryptanalysis
Usually, the target of an encryption system is recovering the key in use rather than only recovering the plaintext of a single ciphertext. **Cryptanalysis** refers to attack techniques which rely on the nature of the algorithm, in addition to perhaps some knowledge of the general characteristics of the plaintext or even some sample plaintext-ciphertext pairs.

The type of cryptanalytic attack used depends on the amount of information known to the cryptanalyst. The most challenging problem is presented when the ciphertext is all that is available, not even the encryption algorithm. However, in general, we assume that the opponent does know the algorithm used for encryption.

-----

### Ciphertext-only attack
In a ciphertext-only attack, the opponent relies on the analysis of the ciphertext itself, generally applying various statistical tests to it. For this approach to work, the opponent must have some general idea of the type of plaintext that is concealed, such as English text, an EXE executable, a Java source file, and so on. As a result, the ciphertext-only attack is the easiest to defend against as the information the opponent has to work with is minimal.

-----

### Known-plaintext attack
In a known-plaintext attack, the opponent has plaintext-ciphertext pairs at their disposable, or know that specific plaintext patterns will appear in a message. For example, there may be a standardised header to an electronic funds transfer message. With this knowledge, the analyst may be able to deduce the key based on how the known plaintext is transformed.

-----

### Chosen-plaintext attack
Suppose the analyst is somehow able to get access to the source system and encrypt their chosen-plaintext, they can deliberately pick patterns that can be expected to reveal the structure of the key. This type of attack is known as a chosen-plaintext attack, and an example of this strategy is differential cryptanalysis.