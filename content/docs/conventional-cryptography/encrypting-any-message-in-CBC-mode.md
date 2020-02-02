---
title: Encrypting any message in CBC mode
markup: "mmark"
weight: 6
---

## Encrypting any message in CBC mode
In this section, we will look at how to process a plaintext whose length is not a multiple of the block length. For example, how would we encrypt an 18-byte plaintext with AES-CBC when blocks are 16 bytes? We will look at techniques to deal with this problem.

-----

### Padding
Padding is a technique that allows you to encrypt a message of any length, by making the ciphertext slightly longer than the plaintext. Padding is used to expand a message to fill a complete block by adding extra bytes to the plaintext.

Here are the rules for padding 16-byte blocks:
- If there is 1 byte left - for example, the plaintext is 1 byte, 17 bytes or 32 bytes - pad the message with 15 bytes of 0f (15 in decimal)
- If there are 2 bytes left, pad the message with 14 bytes of 0e (14 in decimal)
- If there are 3 bytes left, pad the message with 13 bytes of 0d (13 in decimal)
- and so on...
- If there are 15 bytes left, pad the message with a single 01 byte
- If the plaintext is a multiple of 16 (the block length), add 16 bytes of 10 (16 in decimal).

![CBC Padding](/docs/figures/cbc-padding.png)

It follows that decryption works in the following manner:
- Decrypt all blocks as with unpadded CBC
- Ensure that the last bytes of the last block conform to the padding rule: that they finish with at least one 01 byte, at least two 02 bytes, or at least three 03 bytes, and so on. If the padding is not valid, the message is rejected. Otherwise, decryption strips the padding bytes and returns the plaintext bytes left.

**Note**: Padding makes the ciphertext longer by at least one byte and at most a block.

-----

### Ciphertext stealing
Ciphertext stealing is another technique that can be used to encrypt a message whose length is not a multiple of the block size. This technique is more complex and less popular than padding, and it is inelegant and hard to get right.

However, ciphertext stealing provides at least three benefits:
- Plaintexts can be of any *bit* length, not just bytes
- Ciphertexts are the same length as plaintexts
- Ciphertext stealing is not vulnerable to padding oracle attacks

In CBC mode, ciphertext stealing extends the last incomplete plaintext block with bits from the previous ciphertext blocks and then encrypts the resulting block. The last, incomplete ciphertext block is made up of the first bits from the previous ciphertext block, i.e. the bits that have not been appended to the last plaintext block.

![Ciphertext Stealing](/docs/figures/cbc-ciphertext-stealing.png)

In the example above, the last block, $$P_3$$, is complete and padded with zeros. $$P_3$$ is XORed with the previous ciphertext block, and the result is returned as $$C_2$$. The last ciphertext block, $$C_3$$, then consists of the first bits from the previous ciphertext block. Decryption is simply the inverse of this operation.