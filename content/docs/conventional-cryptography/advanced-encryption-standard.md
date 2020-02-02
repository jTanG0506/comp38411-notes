---
title: Advanced Encryption Standard
markup: "mmark"
weight: 4
---

## Advanced Encryption Standard (AES)
AES is the most-used cipher in the world, which was standardised by NIST in 2000 as a replacement for DES, and ever since it has become the world's de facto encryption standard. AES processes blocks of 128 bits using a secret key of 128, 192 or 256 bits, with the most common being the 128-bit key as it makes encryption slightly faster and the difference between a 128- and 256-bit security is redundant for most applications.

![AES Array](/docs/figures/aes-array.png)

Unlike other ciphers that work with individual bits or 64-bit words, AES views a 16-byte plaintext as a two-dimensional array of bytes $$(s = s_0, s_1, \dots, s_{15})$$, known as the internal state, or just state. AES transforms the bytes, columns, and rows of this array to produce the ciphertext. In order to transform its state, AES uses a substitution-permutation network structure, with 10 rounds for 128-bit keys, 12 for 192-bit keys, and 14 for 256-bit keys.

Here is the pseudocode for encryption in AES-128:
```
begin aes-encryption(S, K)
  AddRoundKey(S, K[0])
	
  for i in range(1, 9)
    SubBytes(S)
    ShiftRows(S)
    MixColumns(S)
    AddRoundKey(S, K[i])
		
  SubBytes(S)
  ShiftRows(S)
  AddRoundKey(S, K[i])
end
```

-----

### AES Overview
![AES Internals](/docs/figures/aes-internals.png)

- **AddRoundKey**: an XOR operation of the current block with the round key
- **SubBytes**: performs a byte-by-byte substitution on $$(s_0, s_1, \dots, s_{15})$$ with another byte according to an S-box, which in AES, is a lookup table of 256 elements
- **ShiftRows**: shifts the $$i$$th row of $$i$$ positions, for $$i \in [0, 3]$$
- **MixColumns**: applies the same linear transformation to each of the four columns of the state
\end{itemize}

Each of these operations contributes to AES's security in their own way:
- Without ```KeyExpansion```, all the rounds would use the same key, $$K$$, and AES would be vulnerable to slide attacks
- Without ```AddRoundKey```, encryption would not depend on the key; hence anyone could decrypt the ciphertext without a key
- Without ```SubBytes```, AES would be a large system of linear equations that can be solved using elementary algebra. Hence ```SubBytes``` brings nonlinear operations, which adds cryptographic strength
- Without ```ShiftRows```, changes in a given column would never affect the other columns. However, this is a desirable property; namely, *the avalanche effect* - a change in one bit of the input should affect all the output bits
- Without ```MixColumns```, changes in a byte would not affect any other bytes of the state. A chosen-plaintext attacker could then decrypt any ciphertext after storing 16 lookup tables of 256 bytes each that hold the encrypted values of each possible value of a byte

Recall that AES is a substitution-permutation network (SPN). Here, the substitution layer is ```SubBytes```, and the permutation layer is a combination of ```ShiftRows``` and ```MixColumns```.

-----

### AddRoundKey
The ```AddRoundKey``` transformation is a bitwise operation on the state and the round key. The operation is viewed as a column-wise operation between the 4 bytes of a state column and one word (4 bytes) of the round key so that it can be viewed as a byte-level operation. Note that the inverse transformation, ```InvAddRoundKey``` is the same as ```AddRoundKey``` because the XOR operation is its own inverse.

![AES AddRoundKey](/docs/figures/aes-add-round-key.png)

-----

### SubBytes
The ```SubBytes``` transformation maps bytes into a new byte using a simple lookup table. AES defines a $$16 \times 16$$ matrix of byte values, called an S-box, that consists of a permutation of all possible 256 8-bit values.

Each byte is mapped into a new byte in the following way: the leftmost 4 bits of the byte are used as a row value, and the rightmost 4 bits are used as a column value. These row and column values unique determine an entry in the S-box and the byte is mapped to this entry.

![AES SubBytes](/docs/figures/aes-sub-bytes.png)

For example, the hexadecimal value $$95$$ references row 9, column 5 of the S-box, which say, contains the value $$2A$$. Accordingly, the value $$95$$ is mapped into the value $$2A$$.

The inverse of this operation, ```InvSubBytes``` makes use of the inverse S-box, which is essentially the reverse mapping. For example, if the S-box outputs $$2A$$ for $$95$$, then the inverse S-box outputs $$95$$ for $$2A$$.

-----

### ShiftRows
The ```ShiftRows``` operation works by performing an $$i$$-byte circular left shift on the $$i$$-th row, with $$i = 0$$ for the first row. In other words, the first row is untouched, a 1-byte circular left shift is performed to the second row, a 2-byte circular left shift is performed on the third row, and a 3-byte circular left shift is performed on the bottom row.

![AES ShiftRows](/docs/figures/aes-shift-rows.png)

The inverse of this transformation, ```InvShiftRows``` performs the circular shifts in the opposite direction for the last three rows, with a 1-byte circular right shift for the second row, and so on.

-----

### MixColumns
The ```MixColumns``` transformation operates on each column individually. Each byte of a column is mapped into a new value that is a function of all four bytes of that column. The transformation can be defined by the following matrix multiplication on the state:

$$
\begin{bmatrix}
  02 & 03 & 01 & 01\\ 
  01 & 02 & 03 & 01\\
  01 & 01 & 02 & 03\\
  03 & 01 & 01 & 02
\end{bmatrix}
\begin{bmatrix}
  s_{0,0} & s_{0,1} & s_{0,2} & s_{0,3}\\ 
  s_{1,0} & s_{1,1} & s_{1,2} & s_{1,3}\\
  s_{2,0} & s_{2,1} & s_{2,2} & s_{2,3}\\
  s_{3,0} & s_{3,1} & s_{3,2} & s_{3,3}
\end{bmatrix}=
\begin{bmatrix}
  s_{0,0}^{'} & s_{0,1}^{'} & s_{0,2}^{'} & s_{0,3}^{'}\\ 
  s_{1,0}^{'} & s_{1,1}^{'} & s_{1,2}^{'} & s_{1,3}^{'}\\
  s_{2,0}^{'} & s_{2,1}^{'} & s_{2,2}^{'} & s_{2,3}^{'}\\
  s_{3,0}^{'} & s_{3,1}^{'} & s_{3,2}^{'} & s_{3,3}^{'}
\end{bmatrix}
$$

The individual additions and multiplications are are performed over the finite field $$GL(2^8)$$. The ```MixColumns``` transformations on a single column can be expressed as:

$$
\begin{aligned}
s_{0, j}^{'} & = (2 \cdot s_{0, j}) \oplus (3 \cdot s_{1, j}) \oplus s_{2, j} \oplus s_{3, j}\\
s_{1, j}^{'} & = s_{0, j} \oplus (2 \cdot s_{1, j}) \oplus (3 \cdot s_{2, j}) \oplus s_{3, j}\\
s_{2, j}^{'} & = s_{0, j} \oplus s_{1, j} \oplus (2 \cdot s_{2, j}) \oplus (3 \cdot s_{3, j})\\
s_{3, j}^{'} & = (3 \cdot s_{0, j}) \oplus s_{1, j} \oplus s_{2, j} \oplus (2 \cdot s_{3, j})
\end{aligned}
$$

It follows that the inverse of this transformation would be multiplication by the inverse of the matrix. In symbols:

$$
\begin{bmatrix}
  0E & 0B & 0D & 09\\ 
  09 & 0E & 0B & 0D\\
  0D & 09 & 0E & 0B\\
  0B & 0D & 09 & 0E
\end{bmatrix}
\begin{bmatrix}
  s_{0,0} & s_{0,1} & s_{0,2} & s_{0,3}\\ 
  s_{1,0} & s_{1,1} & s_{1,2} & s_{1,3}\\
  s_{2,0} & s_{2,1} & s_{2,2} & s_{2,3}\\
  s_{3,0} & s_{3,1} & s_{3,2} & s_{3,3}
\end{bmatrix}=
\begin{bmatrix}
  s_{0,0}^{'} & s_{0,1}^{'} & s_{0,2}^{'} & s_{0,3}^{'}\\ 
  s_{1,0}^{'} & s_{1,1}^{'} & s_{1,2}^{'} & s_{1,3}^{'}\\
  s_{2,0}^{'} & s_{2,1}^{'} & s_{2,2}^{'} & s_{2,3}^{'}\\
  s_{3,0}^{'} & s_{3,1}^{'} & s_{3,2}^{'} & s_{3,3}^{'}
\end{bmatrix}
$$

-----

### Key Schedule
The AES key schedule algorithm is ```KeyExpansion```. This function creates 11 round keys $$(K_0, K_1, \dots, K_{10})$$ of 16 bytes each from the 16-byte key, using the same S-box as ```SubBytes``` and a combination of XORs.

If an attacker knows any round key, $$K_i$$, they can determine all of the other round keys, as well as the main key, $$K$$, by reversing the algorithm. The ability to get the key from any round key is usually seen as an imperfect defence against side-channel attacks, where an attacker may easily recover a round key.

-----

### Decrypting AES
To decrypt a ciphertext, AES unwinds each operation by taking its inverse function; the inverse lookup table of ```SubBytes``` reverses the ```SubBytes``` transformation, ```ShiftRow``` shifts in the opposite direction, ```MixColumn```'s inverse is applied (as the matrix inverse), and the ```AddRoundKey```'s inverse is itself as the inverse of an XOR is another XOR.

As shown in the figure below, the sequence of transformation for decryption differs from that for encryption. This has the disadvantage that two separate software modules are needed for applications that required both encryption and decryption. However, we can construct an equivalent version of decryption that has the same sequence of transformations as the encryption, with transformations replaced by their inverse. In order to achieve this equivalence, a change in key schedule is required.

![AES Encryption vs Decryption](/docs/figures/aes-encryption-vs-decryption.png)

By comparing the encryption and decryption rounds, we see that in order to bring the decryption structure in line with the encryption structure, we need to make two changes. The first two stages of the decryption round need to be interchanged, and the second two stages of the decryption round need to be interchanged, that is, ```InvMixColumns``` needs to be interchanged with ```AddRoundKey```, and ```InvSubBytes``` needs to be interchanged with ```InvShiftRows```.

![AES Decryption](/docs/figures/aes-decryption.png)

#### Interchanging InvShiftRows and InvSubBytes
```InvShiftRows``` shifts the bytes in the opposite direction (as ```ShiftRows```) but does not alter the byte contents and is independent of the byte contents. Conversely, ```InvShiftRows``` alters the contents of the bytes but does not alter the sequence of the bytes and is independent of the sequence of the bytes. Therefore, these two operations commute and can be interchanged.
For a given state $$S_i = (s_{i, 1}, s_{i, 2}, \dots, s_{i ,16})$$:

$$\text{InvShiftRows[InvSubBytes(}S_i\text{)]}=\text{InvSubBytes[InvShiftRows(}S_i)]$$

#### Interchanging AddRoundKey and InvMixColumns
Neither of ```AddRoundKey``` and ```InvMixColumns``` alters the sequence of bytes. If we view the key as a sequence of words, then both ```AddRoundKey``` and ```InvMixColumns``` operate on the state one column at a time. These two operations are linear with respect to the columns of the input. That is, for a given state $$S_i = (s_{i, 1}, s_{i, 2}, \dots, s_{i ,16})$$ and round key $$w_j$$:

$$\text{InvMixColumns(}S_i \oplus w_j\text{)}=\text{[InvMixColumns(}S_j\text{)]} \oplus \text{[InvMixColumns(}w_j\text{)]}$$

To see this, suppose that the first column of the state is the sequence $$(y_0, y_1, y_2, y_3)$$ and the first column of the round key $$w_j$$ is $$(k_0, k_1, k_2, k_3)$$, then we need to show

$$
\begin{bmatrix}
  0E & 0B & 0D & 09\\ 
  09 & 0E & 0B & 0D\\
  0D & 09 & 0E & 0B\\
  0B & 0D & 09 & 0E
\end{bmatrix}
\begin{bmatrix}
  y_0 \oplus k_0\\ 
  y_1 \oplus k_1\\
  y_2 \oplus k_2\\
  y_3 \oplus k_3
\end{bmatrix}=
\begin{bmatrix}
  0E & 0B & 0D & 09\\ 
  09 & 0E & 0B & 0D\\
  0D & 09 & 0E & 0B\\
  0B & 0D & 09 & 0E
\end{bmatrix}
\begin{bmatrix}
  y_0\\ 
  y_1\\
  y_2\\
  y_3
\end{bmatrix}\oplus
\begin{bmatrix}
  0E & 0B & 0D & 09\\ 
  09 & 0E & 0B & 0D\\
  0D & 09 & 0E & 0B\\
  0B & 0D & 09 & 0E
\end{bmatrix}
\begin{bmatrix}
  k_0\\ 
  k_1\\
  k_2\\
  k_3
\end{bmatrix}
$$

We will demonstrate that for the first column entry. We need to show that

$$[0E \cdot (y_0 \oplus k_0)] \oplus [0B \cdot (y_1 \oplus k_1)] \oplus [0D \cdot (y_2 \oplus k_2)] \oplus [09 \cdot (y_3 \oplus k_3)]$$

is equivalent to

$$[0E \cdot y_0] \oplus [0B \cdot y_1] \oplus [0D \cdot y_2] \oplus [09 \cdot y_3] \oplus [0E \cdot k_0] \oplus [0B \cdot k_1] \oplus [0D \cdot k_2] \oplus [09 \cdot k_3]$$

We can see that this equivalence holds by inspection. Therefore, we can interchange ```AddRoundKey``` and ```InvMixColumns```, given that we first apply ```InvMixColumns``` to the round key. Note that ```InvMixColumns``` is not applied to the round key for the input for the first ```AddRoundKey``` transformation (before round 1) nor the last ```AddRoundKey``` transformation (round 10). This is as these two ```AddRoundKey``` transformations are not interchanged with ```InvMixColumns``` to produce the equivalent decryption algorithm.

---

### Is AES secure?
Fundamentally, AES is seen as a secure cipher as all output bits depend on all input bits in a complex and pseudorandom way. This was achieved by carefully choosing each component for a particular reason - ```MixColumns``` for its maximal diffusion properties and ```SubBytes``` for its optimal non-linearity, for example. However, there is no proof that AES is immune to all possible attacks, as we do not know what all possible attacks are, and it is not always the case that we know how to prove a cipher is secure against a given attack. AES is said to be heuristic secure, as many skilled people have attempted to break AES, and failed to do so.