# The Careless Twins

## Investigation

I checked the provided RSA code and noticed that both servers were using the same prime `p` to generate their moduli.

I used Python's `gcd()` to find the common prime from `n1` and `n2`.

```python
p = gcd(n1, n2)
```

With `p` recovered, I calculated `q1` using `n1`, then calculated the totient and private exponent.

```python
q1 = n1 // p
phi = (p - 1) * (q1 - 1)
d = pow(e, -1, phi)
```

I used the private exponent to decrypt the ciphertext and converted the result back into bytes.

## Flag

```text
InIt{n3v3r_r3us3_pr1m3s_1n_rs4}
```