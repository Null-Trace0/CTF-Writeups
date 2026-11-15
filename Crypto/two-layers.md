# Two Layers

## Investigation

The challenge involved two layers of encoding. I started by decoding the provided Base64 string:

```bash
echo 'VmFWZ3tlMGdfZ3UzYV9vNjRfMWZfYTBnX2YzcGhlMWdsfQ==' | base64 -d
```

The output was still unreadable, so I used an online tool to brute-force the decoded string. It revealed the flag.

## Flag

```text
InIt{r0t_th3n_b64_1s_n0t_s3cur1ty}
```