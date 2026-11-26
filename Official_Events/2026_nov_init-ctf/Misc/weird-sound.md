# Weird Sound

## Investigation

I started by checking the audio file:

```bash
file audio.wav
exiftool audio.wav
```

It was a 16-bit mono WAV file with a sample rate of 48 kHz and a duration of around 7 seconds.

The challenge description mentioned a spectrum analyzer, so I decided to generate a spectrogram using FFmpeg:

```bash
ffmpeg -i audio.wav -lavfi "showspectrumpic=s=2048x1024:legend=1" -frames:v 1 spectrogram.png
```

I opened `spectrogram.png` and noticed some hidden text. The problem was that the text was blurry and the characters were very close together.

I experimented with different resolutions and visualization settings until the text became readable. It revealed the flag.

## Flag

```text
InIt{W0ah_Y0u_S33_M3!}
```