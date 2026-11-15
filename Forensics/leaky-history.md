# Leaky History

## Investigation

I started by checking the repository and its Git history:

```bash
cd tanvoro-deploy
ls -la
git status
git log --oneline --all --decorate
```

The working tree was clean, and there was nothing suspicious in the visible commit history.

I then ran:

```bash
git fsck --full
```

This returned a dangling commit:

```text
dangling commit 6dfa863c4a9a1700c6f09a608761f82ef9c241a2
```

I checked what was changed in that commit:

```bash
git show --stat 6dfa863c4a9a1700c6f09a608761f82ef9c241a2
```

It showed a file named `deploy/secrets.env`.

I viewed the commit:

```bash
git show 6dfa863c4a9a1700c6f09a608761f82ef9c241a2
```

The deleted file contained AWS credentials, a deployment token and a database password. I found the flag in the same file.

## Flag

```text
InIt{d4ngl1ng_c0mm1t_l34k}
```