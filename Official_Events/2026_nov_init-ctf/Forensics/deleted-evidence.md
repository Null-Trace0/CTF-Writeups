# Deleted Evidence

## Investigation

I started by checking the provided disk image:

```bash
file evidence.img
fdisk -l evidence.img
```

The image contained an NTFS partition starting at sector `2048`.

I used `fls` from SleuthKit to look for deleted files:

```bash
fls -r -d -o 2048 evidence.img
```

Among the results, I found a deleted file named `recovery_code.gz` under `Users/j.morris/Documents/`. Its metadata address was `75-128-2`.

I recovered the file using `icat`:

```bash
icat -o 2048 evidence.img 75-128-2 > recovery_code.gz
```

After checking the archive, I extracted its contents:

```bash
gzip -t recovery_code.gz
zcat recovery_code.gz
```

The output was:

```text
VaVg{q3y3g3q_ohg_a0g_t0a3}
```

The string was encoded, so I decoded it to get the flag.

## Flag

```text
InIt{d3l3t3d_but_n0t_g0n3}
```