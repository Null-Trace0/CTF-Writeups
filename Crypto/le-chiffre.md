# Le Chiffre

## Investigation

The challenge provided the following hint:

> The key is the hour when everything goes quiet.

I interpreted this as midnight, so I used `MIDNIGHT` as the key in a Vigenère decoder.

Decrypting the ciphertext with that key revealed the flag.

## Flag

```text
InIt{v1g3n3r3_w4s_n0t_th3_4nsw3r}
```