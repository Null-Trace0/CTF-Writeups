# Camera Lost & Found

## Investigation

I started by checking the contents of the provided archive:

```bash
7z l "2026-india-trip.7z"
```

It contained 42 JPEG images. I extracted them using the provided password:

```bash
7z x "2026-india-trip.7z"
```

The password was:

```text
camera-found
```

Next, I checked the EXIF metadata of all the images and saved the output to `result.txt`:

```bash
exiftool *.jpg > result.txt
```

While reviewing the metadata, I noticed that `IMG_38.jpg` had a URL in its `Image Description` field:

```text
https://ctf.pir4cy.com/exif-flag
```

I opened the URL and found a Base64-encoded string:

```text
SW5JdHtoMWQxbmdfMW5fbTN0YWQ0dDR9Cg==
```

I decoded it using:

```bash
echo 'SW5JdHtoMWQxbmdfMW5fbTN0YWQ0dDR9Cg==' | base64 -d
```

This revealed the flag.

## Flag

```text
INJw{h1d1ng_1n_m3tad4t4}
```