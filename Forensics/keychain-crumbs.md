# Keychain-Crumbs

## Investigation

I started by checking the packet capture with `tshark`:

```bash
tshark -r capture.pcap -q -z io,phs
```

Then I listed the HTTP requests:

```bash
tshark -r capture.pcap -Y "http.request" -T fields \
-e frame.number -e ip.src -e ip.dst \
-e http.request.method -e http.host -e http.request.uri
```

I noticed repeated POST requests to `update.icloud-sync.cc`, all going to `/api/collect`.

I extracted their payloads into `posts.hex`:

```bash
tshark -r capture.pcap -Y 'http.request.method == "POST"' \
-T fields -e frame.number -e http.file_data > posts.hex
```

The payloads were hex-encoded JSON containing chunk IDs and data. I wrote `reassemble.py` to decode them, sort the chunks and join them. It then Base64-decoded the result and saved it as `exfiltrated.bin`.

```bash
python reassemble.py
```

The script:

```python
import json
import base64
from collections import defaultdict

parts = defaultdict(str)

with open("posts.hex") as f:
    for line in f:
        line = line.strip()
        if not line:
            continue

        payload = json.loads(bytes.fromhex(line))
        parts[payload["id"]] = payload["part"]

print(f"[+] Received {len(parts)} chunks")
print(f"[+] Chunk IDs: {sorted(parts)}")

encoded = "".join(parts[i] for i in sorted(parts))
print(f"[+] Reassembled length: {len(encoded)}")

try:
    decoded = base64.b64decode(encoded)
    with open("exfiltrated.bin", "wb") as f:
        f.write(decoded)

    print("[+] Saved decoded data to exfiltrated.bin")
except Exception as e:
    print(f"[-] Base64 decoding failed: {e}")
```

I checked the recovered file and found that it was a ZIP archive:

```bash
file exfiltrated.bin
unzip -l exfiltrated.bin
```

I extracted it and checked the Keychain dump:

```bash
mkdir recovered
unzip exfiltrated.bin -d recovered
cat recovered/exfil/Library/Keychains/keychain_dump.txt
```

There were several stored credentials, but the `backup-key` entry caught my attention. Its value was Base64-encoded.

I decoded it:

```bash
echo 'SW5JdHtrM3ljaDRpbl9jcnVtYnNfbDM0azNkXzB2M3JfcGw0MW5faHR0cH0=' | base64 -d
```

That gave me the flag.

## Flag

```text
InIt{k3ych4in_crumbs_l34k3d_0v3r_pl41n_http}
```