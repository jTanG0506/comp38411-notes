---
title: Fiestel Schemes
markup: "mmark"
weight: 2
---

## Feistel schemes
IBM engineer Horst Feistel designed a block cipher that works as follows:
1. Split the 64-bit block into two 32-bit halves, $$L$$ and $$R$$
2. Set $$L$$ to $$L \oplus F(R)$$, where $$F$$ is a substitution-permutation round
3. Swap the values of $$L$$ and $$R$$
4. Repeat steps 2 and 3, 15 times
5. Merge $$L$$ and $$R$$ into the 64-bit output block

![Feistel Block Cipher](/docs/figures/feistel-block-cipher.png)

A functionally equivalent representation would be to alternate the round operations $$L = L \oplus F(R)$$ and $$R = R \oplus F(L)$$ instead of swapping $$L$$ and $$R$$, as shown in the diagram above. Note that each round takes a different round key, where each one is derived from the 56-bit key, $$K$$.

In pseudocode:
```
begin feistelScheme(K, r = 16)
  plaintext = (L[0], R[0])
	
  for i in range(1, r)
    derive subkey k[i] from the key K
    L[i] = R[i - 1]
    R[i] = L[i - 1] XOR f(R[i - 1], k[i])
		
  ciphertext = (R[r], L[r])
end
```

-----

### Decrypting the Feistel scheme
Decryption is the same algorithm as encryption, except that the subkeys are applied in the reverse order.

#### Proof
To prove this, it suffices to show that the first round of the decryption is the inverse of the last round of encryption. That is, we need to show that $$LD_1 = RE_{15}$$ and $$RD_1 = LE_{15}$$.

The output of the encryption algorithm $$(RE_{16}, LE_{16})$$ is the input of the decryption algorithm $$(LD_0, RD_0)$$, so we get that $$RE_{16} = LD_0$$ and $$LE_16 = RD_0$$.

It follows that $$LD_1 = RD_0 = KE_{16} = RE_{15}$$. Now all that remains is to show that $$RD_1 = LE_{15}$$.
$$
\begin{aligned}
RD_1 & = LD_0 \oplus f(RD_0, K_{16})\\
& = RE_{16} \oplus f(RD_0, K_{16})\\
& = (LE_{15} \oplus f(RE_{15}, K_{16})) \oplus f(RD_0, K_{16})\\
& = LE_{15} \oplus f(RD_0, K_{16}) \oplus f(RD_0, K_{16})\\
& = LE_{15}
\end{aligned}
$$

By continuity, we can see that the decryption algorithm does in fact cancel out the encryption algorithm, with the keys applied in reverse order.