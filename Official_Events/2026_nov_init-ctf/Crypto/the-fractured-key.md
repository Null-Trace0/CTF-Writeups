# The Fractured Key

## Investigation

The provided ZIP contained two files: `intercepted.txt` and `key_gen.py`.

I checked `key_gen.py` first to understand how the encryption key was generated. The script used Python's `random` module, with a seed between `0` and `255`, to generate a 32-byte key.

The ciphertexts were encrypted using AES-256-CBC. The first 16 bytes of each encrypted message were used as the IV.

Since the seed could only be one of 256 possible values, I wrote a small Python script to try every seed and decrypt the ciphertexts.

The seeds that produced valid plaintexts were 42, 201, and 137, respectively. The first and third were decoys; the second contained the actual flag.

## Flag

```text
InIt{ps3ud0_r4nd0m_1s_n0t_r4nd0m_k3y_sp4c3_t00_sm4ll}
```