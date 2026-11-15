# Where Am I?

## Investigation

I started by checking the provided image.

```bash
file image.jpg
exiftool image.jpg
```

One thing I noticed was that although the file was named `image.jpg`, it was actually a WebP image. I checked the metadata for GPS coordinates, but there wasn't anything useful there.

Since metadata wasn't helping, I tried reverse image searching. I also looked at the architecture in the picture, especially the old brick and stone buildings and the outdoor staircase.

After comparing the results, I identified the location as **Perugia, Italy**.

The remaining part was the timezone. Perugia uses CET during standard time and CEST during daylight saving time. The required timezone for the flag was CEST.

## Flag

```text
InIt{PERUGIA_ITALY_CEST}
```