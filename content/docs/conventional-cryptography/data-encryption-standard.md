---
title: Data Encryption Standard
markup: "mmark"
weight: 3
---

## Data Encryption Standard (DES)
The DES algorithm is a 16 round Feistel cipher, which takes a 64-bit plaintext block and a 56-bit key as input. The DES algorithm was first published in 1977 as a US Federal Standard and is a de facto international standard for banking security. The DES consists of three main stages: the initial permutation, the round function, and the final permutation.

The key scheduling algorithm for DES generates 48-bits subkeys, $$k_1, k_2, \dots, k_{16}$$, from the 56-bit key $$K$$. Details for the key scheduling algorithm have been omitted but is easily found online, for the interested readers.

-----

### Initial Permutation ($$IP$$)
The initial permutation of the DES algorithm changes the order of the plaintext before the first round of encryption. The structure of the initial permutation is shown in the table below.

$$     
\def\arraystretch{1.3}
\begin{array}{|c|c|c|c|c|c|c|c|}
\hline
58 & 50 & 42 & 34 & 26 & 18 & 10 & 2\\ \hline
60 & 52 & 44 & 36 & 28 & 20 & 12 & 4\\ \hline
62 & 54 & 46 & 38 & 30 & 22 & 14 & 6\\ \hline
64 & 56 & 48 & 40 & 32 & 24 & 16 & 8\\ \hline
57 & 49 & 41 & 33 & 25 & 17 &  9 & 1\\ \hline
59 & 51 & 43 & 35 & 27 & 19 & 11 & 3\\ \hline
61 & 53 & 45 & 37 & 29 & 21 & 13 & 5\\ \hline
63 & 55 & 47 & 39 & 31 & 23 & 17 & 7\\ \hline
\end{array}
$$

-----

### DES round function
The DES round function consists of several steps:
1. The 32-bit half-block input is expanded and permuted to 48 bits
2. The XOR operator is applied to the expanded input and the round key
3. The result is split into eight lots of 6-bit pieces
4. Each 6-bit piece is used as an index to an S-box to produce a 4-bit result
5. The results are permuted through a P-Box

#### Step 1: The Expansion Function
The expansion function $$E$$ is used to expand and permute the 32-bit input into a 48-bit block. This function is relatively straightforward, and the expansion-permutation is shown below.

$$     
\def\arraystretch{1.3}     
\begin{array}{|c|c|c|c|c|c|}
\hline
32 &  1 &  2 &  3 &  4 & 5\\ \hline
 4 &  5 &  6 &  7 &  8 & 9\\ \hline
 8 &  9 & 10 & 11 & 12 & 13\\ \hline
12 & 13 & 14 & 15 & 16 & 17\\ \hline
16 & 17 & 18 & 19 & 20 & 21\\ \hline
20 & 21 & 22 & 23 & 24 & 25\\ \hline
24 & 25 & 26 & 27 & 28 & 29\\ \hline
28 & 29 & 30 & 31 & 32 &  1\\ \hline
\end{array}
$$

#### Step 2: Exclusive OR
The exclusive OR operator, $$\oplus$$, is applied to the 48-bit output from the expansion function $$E$$ and the 48-bit round key, $$K_i$$.

#### Step 3: Splitting
The 48-bits are split into eight groups of 6-bits, you can see this as splitting it into the rows in the table from Step 1.

#### Step 4: DES S-Boxes
Each of the 6-bit bits is passed through a different S-Box as follows: the row and column are selected using the outer two and inner four bits of the binary representation of the input, respectively.

For example, an input of $$101100$$ would use the third row (as the outer two bits are $$10$$, which is 2 and we start counting at 0) and the seventh column (the inner two bits are $$0110$$, which is 6 and we start counting at 0).

For sake of completeness, the eight S-boxes are listed below.

$$     
\def\arraystretch{1.3}     
\begin{array}{l|cccccccccccccccc}
\hline
S1 &  0 &  1 &  2 &  3 &  4 &  5 &  6 &  7 &  8 &  9 & 10 & 11 & 12 & 13 & 14 & 15 \\ \hline
 0 & 14 &  4 & 13 &  1 &  2 & 15 & 11 &  8 &  3 &  0 &  6 & 12 &  5 &  9 &  0 &  7 \\
 1 &  0 & 15 &  7 &  4 & 14 &  2 & 13 &  1 & 10 &  6 & 12 & 11 &  9 &  5 &  3 &  8 \\
 2 &  4 &  1 & 14 &  8 & 13 &  6 &  2 & 11 & 15 &  2 &  9 &  7 &  3 & 10 &  5 &  0 \\
 3 & 15 & 12 &  8 &  2 &  4 &  9 &  1 &  7 &  5 &  1 &  3 & 14 & 10 &  0 &  6 & 13
\end{array}
$$

$$
\def\arraystretch{1.3}     
\begin{array}{l|cccccccccccccccc}
\hline
S2 &  0 &  1 &  2 &  3 &  4 &  5 &  6 &  7 &  8 &  9 & 10 & 11 & 12 & 13 & 14 & 15 \\ \hline
 0 & 15 &  1 &  8 & 14 &  6 & 11 &  3 &  4 &  9 &  7 &  2 & 13 & 12 &  0 &  5 & 10 \\
 1 &  3 & 13 &  4 &  7 & 15 &  2 &  8 & 14 & 12 &  0 &  1 & 10 &  6 &  9 & 11 &  5 \\
 2 &  0 & 14 &  7 & 11 & 10 &  4 & 13 &  1 &  5 &  8 & 12 &  6 &  9 &  3 &  2 & 15 \\
 3 & 13 &  8 & 10 &  1 &  3 & 15 &  4 &  2 & 11 &  6 &  7 & 12 &  0 &  5 & 14 &  9
\end{array}
$$

$$
\def\arraystretch{1.3}     
\begin{array}{l|cccccccccccccccc}
\hline
S3 &  0 &  1 &  2 &  3 &  4 &  5 &  6 &  7 &  8 &  9 & 10 & 11 & 12 & 13 & 14 & 15 \\ \hline
 0 & 10 &  0 &  9 & 14 &  6 &  3 & 15 &  5 &  1 & 13 & 12 &  7 & 11 &  4 &  2 &  8 \\
 1 & 13 &  7 &  0 &  9 &  3 &  4 &  6 & 10 &  2 &  8 &  5 & 14 & 12 & 11 & 15 &  1 \\
 2 & 13 &  6 &  4 &  9 &  8 & 15 &  3 &  0 & 11 &  1 &  2 & 12 &  5 & 10 & 14 &  7 \\
 3 &  1 & 10 & 13 &  0 &  6 &  9 &  8 &  7 &  4 & 15 & 14 &  3 & 11 &  5 &  2 & 12
\end{array}
$$

$$
\def\arraystretch{1.3}     
\begin{array}{l|cccccccccccccccc}
\hline
S4 &  0 &  1 &  2 &  3 &  4 &  5 &  6 &  7 &  8 &  9 & 10 & 11 & 12 & 13 & 14 & 15 \\ \hline
 0 &  7 & 13 & 14 &  3 &  0 &  6 &  9 & 10 &  1 &  2 &  8 &  5 & 11 & 12 &  4 & 15 \\
 1 & 13 &  8 & 11 &  5 &  6 & 15 &  0 &  3 &  4 &  7 &  2 & 12 &  1 & 10 & 14 &  9 \\
 2 & 10 &  6 &  9 &  0 & 12 & 11 &  7 & 13 & 15 &  1 &  3 & 14 &  5 &  2 &  8 &  4 \\
 3 &  3 & 15 &  0 &  6 & 10 &  1 & 13 &  8 &  9 &  4 &  5 & 11 & 12 &  7 &  2 & 14
\end{array}
$$

$$
\def\arraystretch{1.3}     
\begin{array}{l|cccccccccccccccc}
\hline
S5 &  0 &  1 &  2 &  3 &  4 &  5 &  6 &  7 &  8 &  9 & 10 & 11 & 12 & 13 & 14 & 15 \\ \hline
 0 &  2 & 12 &  4 &  1 &  7 & 10 & 11 &  6 &  8 &  5 &  3 & 15 & 13 &  0 & 14 &  9 \\
 1 & 14 & 11 &  2 & 12 &  4 &  7 & 13 &  1 &  5 &  0 & 15 & 10 &  3 &  9 &  8 &  6 \\
 2 &  4 &  2 &  1 & 11 & 10 & 13 &  7 &  8 & 15 &  9 & 12 &  5 &  6 &  3 &  0 & 14 \\
 3 & 11 &  8 & 12 &  7 &  1 & 14 &  2 & 13 &  6 & 15 &  0 &  9 & 10 &  4 &  5 &  3
\end{array}
$$

$$
\def\arraystretch{1.3}     
\begin{array}{l|cccccccccccccccc}
\hline
S6 &  0 &  1 &  2 &  3 &  4 &  5 &  6 &  7 &  8 &  9 & 10 & 11 & 12 & 13 & 14 & 15 \\ \hline
 0 & 12 &  1 & 10 & 15 &  9 &  2 &  6 &  8 &  0 & 13 &  3 &  4 & 14 &  7 &  5 & 11 \\
 1 & 10 & 15 &  4 &  2 &  7 & 12 &  9 &  5 &  6 &  1 & 13 & 14 &  0 & 11 &  3 &  8 \\
 2 &  9 & 14 & 15 &  5 &  2 &  8 & 12 &  3 &  7 &  0 &  4 & 10 &  1 & 13 & 11 &  6 \\
 3 &  4 &  3 &  2 & 12 &  9 &  5 & 15 & 10 & 11 & 14 &  1 &  7 &  6 &  0 &  8 & 13
\end{array}
$$

$$
\def\arraystretch{1.3}     
\begin{array}{l|cccccccccccccccc}
\hline
S7 &  0 &  1 &  2 &  3 &  4 &  5 &  6 &  7 &  8 &  9 & 10 & 11 & 12 & 13 & 14 & 15 \\ \hline
 0 &  4 & 11 &  2 & 14 & 15 &  0 &  8 & 13 &  3 & 12 &  9 &  7 &  5 & 10 &  6 &  1 \\
 1 & 13 &  0 & 11 &  7 &  4 &  9 &  1 & 10 & 14 &  3 &  5 & 12 &  2 & 15 &  8 &  6 \\
 2 &  1 &  4 & 11 & 13 & 12 &  3 &  7 & 14 & 10 & 15 &  6 &  8 &  0 &  5 &  9 &  2 \\
 3 &  6 & 11 & 13 &  8 &  1 &  4 & 10 &  7 &  9 &  5 &  0 & 15 & 14 &  2 &  3 & 12
\end{array}
$$

$$
\def\arraystretch{1.3}     
\begin{array}{l|cccccccccccccccc}
\hline
S8 &  0 &  1 &  2 &  3 &  4 &  5 &  6 &  7 &  8 &  9 & 10 & 11 & 12 & 13 & 14 & 15 \\ \hline
 0 & 13 &  2 &  8 &  4 &  6 & 15 & 11 &  1 & 10 &  9 &  3 & 14 &  5 &  0 & 12 &  7 \\
 1 &  1 & 15 & 13 &  8 & 10 &  3 &  7 &  4 & 12 &  5 &  6 & 11 &  0 & 14 &  9 &  2 \\
 2 &  7 & 11 &  4 &  1 &  9 & 12 & 14 &  2 &  0 &  6 & 10 & 13 & 15 &  3 &  5 &  8 \\
 3 &  2 &  1 & 14 &  7 &  4 & 10 &  8 & 13 & 15 & 12 &  9 &  0 &  3 &  5 &  6 & 11
\end{array}
$$

#### Step 5: DES P-Boxes
The final step of the DES round function, $$f$$ is to permute (using a permutation function $$P$$), the eight groups of 4-bits. The permutation table is shown below.

$$
\def\arraystretch{1.3}     
\begin{array}{|c|c|c|c|c|c|c|c|}
\hline
16 &  7 & 20 & 21 & 29 & 12 & 28 & 17\\ \hline
 1 & 15 & 23 & 26 &  5 & 18 & 31 & 10\\ \hline
 2 &  8 & 24 & 14 & 32 & 27 &  3 &  9\\ \hline
19 & 13 & 30 &  6 & 22 & 11 &  4 & 25\\ \hline
\end{array}
$$

-----

### Inverse Permutation
The final permutation occurs after the 16 rounds of DES are completed, it is the inverse of the initial permutation (IP) and is shown in the table below.

$$
\def\arraystretch{1.3}
\begin{array}{|c|c|c|c|c|c|c|c|}
\hline
40 & 8 & 48 & 16 & 56 & 24 & 64 & 32\\ \hline
39 & 7 & 47 & 15 & 55 & 23 & 63 & 31\\ \hline
38 & 6 & 46 & 14 & 54 & 22 & 62 & 30\\ \hline
37 & 5 & 45 & 13 & 53 & 21 & 61 & 29\\ \hline
36 & 4 & 44 & 12 & 52 & 20 & 60 & 28\\ \hline
35 & 3 & 43 & 11 & 51 & 19 & 59 & 27\\ \hline
34 & 2 & 42 & 10 & 50 & 18 & 58 & 26\\ \hline
33 & 1 & 41 &  9 & 49 & 17 & 57 & 25\\ \hline
\end{array}
$$

-----

### Decrypting DES
Like all other Feistel ciphers, the process the decryption in DES is the same as encryption, apart from the fact that the keys for each round are applied in reverse order, i.e. $$k_{16}$$ first and $$k_1$$ last.

-----

### Problems with DES}
#### 56-bit key
One of the main weaknesses of DES its key length of 56 bits, meaning there are $$2^{56}$$ possible keys, which can be broken on average in $$2^{55}$$ trials. With current technology, a rate of $$10^{13}$$ encryptions per second is reasonable, meaning it would take just one hour for an exhaustive key search.

#### Nature of the S-boxes
The design criteria for the eight S-boxes used in DES were not made public, which led to the suspicion that the boxes were constructed in such a way that cryptanalysis is possible for an opponent who knows the weaknesses in the S-boxes. Although some unexpected behaviours of the S-boxes have been discovered, nobody has yet succeeded in discovering the supposed fatal weakness in S-boxes.

-----

### Triple DES
Because of its vulnerability to brute-force attacks, DES has been widely replaced by stronger encryption schemes. One approach is to use completely new algorithms, such as AES. Another alternative, which preserves the existing investment in software and equipment, is to use multiple encryptions with DES and multiple keys. Triple DES (3DES) is the most widely used multiple-encryption scheme.

#### Triple DES with Two Keys (EDE2)
Triple DES with comes in two variants, a two-key one and a three-key one. The two-key version follows an encrypt-decrypt-encrypt (EDE) sequence and makes use of two keys, $$K_1$$ and $$K_2$$, each of 56 bits. The two-key version of triple DES works as follows:

$$
\begin{aligned}
C & = E(K_1, D(K_2, E(K_1, P)))\\
P & = D(K_1, E(K_2, E(K_1, C)))
\end{aligned}
$$

There is no cryptographic significance to the use of the decryption, it is simply there for backward compatibility with single DES users, as:

$$
\begin{aligned}
C & = E(K_1, D(K_1, E(K_1, P))) = E(K_1, P)\\
P & = D(K_1, E(K_1, E(K_1, P))) = D(K_1, C)
\end{aligned}
$$

![EDE2](/docs/figures/ede2.png)

#### Triple DES with Three Keys (EDE3)
A preferred alternative is triple DES with three keys, and is defined as:

$$
\begin{aligned}
C & = E(K_3, D(K_2, E(K_1, P)))\\
P & = D(K_3, E(K_2, E(K_1, C)))
\end{aligned}
$$

![EDE2](/docs/figures/ede3.png)

Although we have a key length of 168 bits, the effective key length is much shorter due do the meet-in-the-middle attack (described in a later section). EDE3 can be backward compatible with DES by setting either $$K_3 = K_2$$ or $$K_1 = K_2$$.