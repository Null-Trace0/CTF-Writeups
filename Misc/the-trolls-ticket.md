# The Troll's Ticket

## 1. Initial Investigation – Delhi

The challenge provided three files:

- `IRCTC_Tatkal_Hack.pdf`
- `outer_wordlist.txt`
- `README.txt`

I checked the supposed PDF and found out it was actually a ZIP archive.

```bash
file IRCTC_Tatkal_Hack.pdf
unzip -l IRCTC_Tatkal_Hack.pdf
```

I used the provided wordlist to crack it:

```bash
fcrackzip -u -D -p outer_wordlist.txt IRCTC_Tatkal_Hack.pdf
```

**Password:** `railwaystationroyaljourney`

After extracting it, I found `delhi.jpg`, `delhi_wordlist.txt`, and a README.

## 2. Delhi – Hidden ZIP Archive

I checked `delhi.jpg` with Binwalk and found an appended ZIP archive.

```bash
binwalk delhi.jpg
```

The ZIP started at offset `89038`, so I extracted it using `dd`:

```bash
dd if=delhi.jpg of=delhi_hidden.zip bs=1 skip=89038 status=progress
```

I used the Delhi wordlist to crack it:

```bash
fcrackzip -u -D -p delhi_wordlist.txt delhi_hidden.zip
```

**Password:** `cholebhature`

The archive contained `delhi.txt`, `mumbai.jpg`, and `mumbai_wordlist.txt`. The clue pointed to Mumbai.

## 3. Mumbai – Another Hidden ZIP

I checked `mumbai.jpg` with Binwalk and found another appended ZIP archive.

```bash
binwalk mumbai.jpg
```

Its offset was `1958742`, so I extracted it:

```bash
dd if=mumbai.jpg of=mumbai_hidden.zip bs=1 skip=1958742 status=progress
```

I used the Mumbai wordlist to crack the archive:

```bash
fcrackzip -u -D -p mumbai_wordlist.txt mumbai_hidden.zip
```

**Password:** `vada_pav`

The archive contained `mumbai.txt`, `chennai.jpg`, and `chennai_wordlist.txt`.

## 4. Chennai – Hidden ZIP

I repeated the same process with `chennai.jpg`.

```bash
binwalk chennai.jpg
```

The ZIP archive started at offset `167393`.

```bash
dd if=chennai.jpg of=chennai_hidden.zip bs=1 skip=167393 status=progress
```

I used the Chennai wordlist to crack it:

```bash
fcrackzip -u -D -p chennai_wordlist.txt chennai_hidden.zip
```

**Password:** `mylapore`

This archive contained `chennai.txt` and `kolkata.jpg`.

## 5. Kolkata – Hidden TAR Archive

I checked `kolkata.jpg` with Binwalk. This time, it found an appended POSIX TAR archive.

```bash
binwalk kolkata.jpg
```

The archive started at offset `157807`, so I extracted it:

```bash
dd if=kolkata.jpg of=kolkata_hidden.tar bs=1 skip=157807 status=progress
```

I extracted its contents:

```bash
mkdir final_stage
tar -xf kolkata_hidden.tar -C final_stage
```

This revealed three files:

- `kolkata.txt`
- `final_list.txt`
- `golden_diamond_ticket_locked.pdf`

## 6. Unlocking the Golden Diamond Ticket

The final PDF used Standard Security Handler Revision 6. The installed `pdfcrack` didn't support this encryption version, so I used Python's `pypdf` library to test the passwords from `final_list.txt`.

```python
from pypdf import PdfReader

pdf = "final_stage/golden_diamond_ticket_locked.pdf"

with open("final_stage/final_list.txt") as f:
    for password in map(str.strip, f):
        reader = PdfReader(pdf)

        if reader.decrypt(password):
            print(f"[+] Password found: {password}")
            break
    else:
        print("[-] No password matched.")
```

The script found the password:

**Password:** `peacock`

I decrypted the PDF and extracted its text, which revealed the flag.

## Flag

```text
InIt{j0urney_4cr0ss_1ndia}
```