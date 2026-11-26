# Red John Returns

## Investigation

I followed the GitHub link associated with Red John's account and found a file named `for_patrick`.

The file contained six hashes along with four technical questions. The clue provided was:

> I never liked even company.

I took this as a hint to use only the odd-numbered entries.

### Cracking the Hashes

I used [CrackStation](https://crackstation.net/) to identify the hashes.

The results, in their original order, were:

| Position | Hash type | Plaintext |
|---|---|---|
| 1 | MD5 | `smiley` |
| 2 | SHA-1 | `password` |
| 3 | SHA-256 | `teacup` |
| 4 | MD5 | `qwerty` |
| 5 | NTLM | `lisbon` |
| 6 | SHA-256 | `letmein` |

### Answering the Questions

The four technical questions asked about:

- Default SSH port: `22`
- Year of the Morris worm: `1988`
- Default RDP port: `3389`
- Year of WannaCry: `2017`

Following the clue, I selected the odd-numbered entries from the combined sequence of hash results and question answers:

1. `smiley`
2. `1988`
3. `teacup`
4. `3389`
5. `lisbon`

Combining them in that order gave the flag.

## Flag

```text
InIt{smiley_1988_teacup_3389_lisbon}
```