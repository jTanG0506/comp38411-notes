---
title: Transposition
markup: "mmark"
weight: 3
---

## Transpositions
Another technique for mapping is achieved by performing some **permutation** on the plaintext letters. This technique is referred to as a transposition cipher. One way of doing this would be to write the message in a rectangle, row by row, and read the column message off, column by column, but permute the order of the columns. The order of the columns then becomes the key to the algorithm.

#### Example
![Transposition Example](/docs/figures/transposition-example.png)

In the example above, the plaintext is *attackpostponeduntiltomorrow*, the key is $$4312567$$, and to obtain the ciphertext, start with the column that is labelled 1, write down the letters in that column (top to bottom), then proceed to the column labelled 2, and repeat.

A pure transposition cipher is easily recognised since it has the same letter frequencies as the plaintext, so cryptanalysis is reasonably straightforward and involves trying different permutations of the column indices. The transposition cipher can be made significantly more secure by performing multiple stages of the transposition, thus making it harder to reconstruct.
