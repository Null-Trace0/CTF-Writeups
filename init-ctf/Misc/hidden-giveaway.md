# Hidden Giveaway

## Investigation

The challenge provided an image named `india_tourist_map.png` with a hint about something hidden in the tourist map.

I started by checking its metadata:

```bash
exiftool india_tourist_map.png
```

The image was 935 × 909 pixels, RGB, with an 8-bit depth. There were no suspicious metadata fields.

I also checked for embedded files using Binwalk:

```bash
binwalk india_tourist_map.png
```

It only identified the PNG itself.

Since those checks didn't reveal anything, I decided to inspect the colour channels using Python Pillow. I installed Pillow:

```bash
sudo pacman -S python-pillow
```

I created `inspect.py` to check the image mode, dimensions, colour channels and their value ranges:

```python
from PIL import Image

img = Image.open("india_tourist_map.png")

print("Mode:", img.mode)
print("Size:", img.size)
print("Bands:", img.getbands())

for band in img.getbands():
    channel = img.getchannel(band)
    print(f"{band}: min={channel.getextrema()[0]}, max={channel.getextrema()[1]}")
```

I ran it:

```bash
python inspect.py
```

The output confirmed that the image used RGB channels.

Next, I extracted the least significant bit (LSB) from each colour channel. I created `extract_lsb.py`:

```python
from PIL import Image

img = Image.open("india_tourist_map.png")
pixels = img.load()

width, height = img.size

for channel, name in enumerate(["red", "green", "blue"]):
    output = Image.new("L", (width, height))
    out_pixels = output.load()

    for y in range(height):
        for x in range(width):
            value = pixels[x, y][channel] & 1
            out_pixels[x, y] = value * 255

    output.save(f"{name}_lsb.png")
```

I ran the script:

```bash
python extract_lsb.py
```

This generated three images: `red_lsb.png`, `green_lsb.png` and `blue_lsb.png`.

After inspecting them, I found a QR code in `blue_lsb.png`.

I scanned the QR code and got this URL:

```text
https://ctf.pri4cy.com/init-misc1
```

Opening the URL gave me a Base64-encoded string. I decoded it to recover the flag.

## Flag

```text
InIt{h1d1ng_1n_b1ue_l1gh7}
```