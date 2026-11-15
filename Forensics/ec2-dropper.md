# ec2-dropper

## Investigation

I started by extracting the provided ZIP and checking its contents:

```bash
unzip incident_ec2.zip
cd incident_ec2
```

The archive contained a Python script, an encrypted vault, incident notes and system logs.

I checked `agent/vault.py` first to understand how the vault was encrypted. I found that the key was derived from the EC2 instance ID using SHA-256:

```python
SALT = "s4lt_"

def derive_key(iid: str) -> bytes:
    return hashlib.sha256((SALT + iid).encode()).digest()
```

So the next step was finding the instance ID.

I checked the available logs:

```bash
cat artifacts/cloud-init-output.log
cat artifacts/syslog.txt
cat artifacts/crontab.txt
```

The instance ID was present in `cloud-init-output.log`:

```text
i-0a1b2c3d4e5f67890
```

The script also checked for a cached instance ID before trying to retrieve it from the EC2 metadata service. Since I was working offline, I created the cache file manually:

```bash
echo -n 'i-0a1b2c3d4e5f67890' > agent/instance-id.cache
```

Then I ran the provided script against the encrypted vault:

```bash
python3 agent/vault.py agent/vault.bin
```

It successfully decrypted the vault and printed the JSON data containing the flag.

## Flag

```text
InIt{imds_k3y3d_s3cr3t_v4ult}
```