# Known Plaintext PRNG

## Investigation

The challenge came with a Python script that encrypted the flag using XOR in 4-byte blocks. It also used an LCG to generate a new key for each block.

Since I knew the flag started with `InIt`, I used those first four bytes to recover the initial key by XORing them with the first four bytes of the ciphertext.

The recovered key was:

```text
9011a89e
```

I then followed the same key-generation logic from the provided script:

```python
key = (key * 1103515245 + 12345) & 0xFFFFFFFF
```

Using the recovered key, I recreated the process in Python and XORed each ciphertext block with its corresponding generated key. This gave me the original plaintext.

## Flag

```text
InIt{kn0wn_pl41nt3xt_4tt4cks_4r3_c00l}
```