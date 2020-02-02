---
title: Key Management Issues
markup: "mmark"
weight: 1
---

## Key Management Issues

Key management refers to the management of keys in a cryptosystem, which includes:
1. **Generation**: how should keys be generated such that the strength of the key is appropriate for the use case?
1. **Storage**: how should keys be stored securely so that they cannot be easily stolen? Also, keys should never be stored alongside the data they are trying to protect, as any exfiltration of the protected data will also compromise the key.
1. **Transportation**: how should keys be delivered to their intended recipients securely? Physical exchange of keys is impractical while     sending the key through an existing encryption channel relies on the security of a previous key exchange.
1. **Establishment**: how can two communicating parties agree on a key to be used?
1. **Revocation**: how are keys revoked and replaced, should that key happen to be compromised, or we wish the withdraw the holders' privilege of that key?

-----

### Key Spaces

The **key space** is the set of all possible permutations of a key. For example, if your key consists of 3 letters, and each of these letters must be from the set $$\{A, B\}$$, then the key space would be $$\{AAA, AAB, ABA, ABB, BBB, BBA, BAB, BAA\}$$ with size $$8 = 2^3$$. Notice that we have to choose $$3$$ letters, and for each letter, we have $$2$$ choices; thus, the total number of combinations is $$2^3$$.

| Key Space | Size | Number of possible keys (6-bytes) | Number of possible keys (8-bytes) | Exhaustive search* (6-bytes)	| Exhaustive search* (8-bytes) |
|------------------------------	|:----:	|:---------------------------------:	|:---------------------------------:	|:------------------------------------------------------:	|:------------------------------------------------------:	|
| Lowercase letters | $$26$$ | $$3.1 \times 10^8$$ | $$2.1 \times 10^{11}$$ | 5 minutes | 2.4 days |
| Lowercase letters and digits | $$36$$ | $$2.2 \times 10^9$$ | $$2.8 \times 10^{12}$$ | 36 minutes | 33 days |
| Alphanumeric characters | $$62$$ | $$5.7 \times 10^{10}$$ | $$2.2 \times 10^{14}$$ | 16 hours | 6.9 years |
| Printable characters | $$95$$ | $$7.4 \times 10^{11}$$ | $$6.6 \times 10^{15}$$ | 8.5 days | 210 years |
| ASCII characters | $$128$$ | $$4.4 \times 10^{12}$$ | $$7.2 \times 10^{16}$$ | 51 days | 2300 years |

\* = at $$10^6$$ attempts/sec

To prevent an adversary from performing an exhaustive search for the key, the key space should be chosen to be large enough to make such a search infeasible. Besides, placing constraints on the alphabet will significantly reduce the number of possible keys, thus making ciphertexts much easier to break. Since computing power approximately doubles every 18 months, we should plan accordingly if we expect a key to withstand brute force attacks and have a lifetime of 10 years, for example.

-----

### Key Generation

A desirable property for keys is that they must be selected in a truly random fashion. Should this not be the case, and the attacker can determine some factor which may influence the selection of the key, the search space can be significantly reduced. For example, humans generally do not choose passwords randomly; therefore, attackers can try the more obvious ones first instead of a systematic brute force search.

A good key is a random number. Ordinary random number generators such as `java.util.Random` may not be a good enough for this purpose; however, we have cryptographically secure pseudo-random number generators, such as the `SecureRandom` class in the `java.security` package.

#### Random numbers
Given $$k \in \mathbb{Z}$$ and a sequence of numbers $$n_1, n_2, \dots$$, if an observer cannot predict $$n_k$$, even if all of $$n_1, \dots, n_{k-1}$$ are known, then we say $$n_k$$ is a random number. Random numbers can be obtained from a source of uncertainty, or *source of entropy*, such as atmospheric noise, stock market data and polarization of photons.

#### Strong mixing functions
We can generate pseudo-random numbers from a **strong mixing function**, which is, a function that takes two or more inputs having some *randomness* (such as CPU load and arrival times of network packets) and produces an output where each bit depends on some non-linear function of all the bits of the inputs. Cryptographic hashing functions and encryption algorithms, such as MD5, SHA, and DES, are all examples of strong mixing functions.

For example, in a UNIX system, you may use the process state at a given time (`date; ps gaux`) as the input to an MD5 function to generate a pseudorandom number, where `ps gaux` lists all the information about all the processes on the system.

-----

### Key Storage
Once we have a secret key, you need to keep it secret, to the same degree as all the data it encrypts. Otherwise, an attacker would not need to go through all the trouble of trying to break the cipher system if the key can be recovered due to inadequate key storage procedures. For example, an attacker could bribe a clerk for $$\pounds 1000$$ instead of spending $$\pounds 10m$$ on building a cryptanalysis machine.

#### Key wrapping
Attackers may be able to defeat access control mechanisms, so we should encrypt the key using a second key. Ideally, a key should never appear unencrypted outside the encryption device and not be stored on a medium connected to the network. The problem with this approach is that the second key must be available when you need to decrypt the protected key. Usually, the second key is often generated from a password supplied by the user when they need to use the protected key - this is how private keys for SSH are usually protected.

#### Hardware tokens
If the key resides in memory, attackers may be able to read the key if they can get access to the machine. In this approach, the key is stored on a physical token, such as a smart card or a USB dongle, and remains safe even if the machine is compromised, as they usually require you to enter a PIN to unlock the key from the secure memory.

This approach is costly and also less convenient as it requires you to carry the hardware token with you and run the risk of losing it or it being stolen. As the hardware token can be stolen, we should split the key into two halves, and store one half in the card and the other half in the machine.

------

### Session Key Establishment
A session key is a temporary key which is used for the duration of a logical connection. Ideally, the session key should have a very short lifetime, and a new session key should be used for each new session. 

It is desirable to exchange session keys frequently, as this means the opponent has less ciphertext available for cryptanalysis for any given session key, thus making the key more secure. Also, the shorter lifetime of a session key limits the exposure, in terms of both time and amount of data, in the event of key compromise. We can avoid long-term storage of a large number of secret keys, by only creating them for each exchange.

However, the distribution of session keys will delay the start of any exchange and places a burden on network capacity. Therefore, we must try to balance these considerations when determining a suitable lifetime for a particular session key.
