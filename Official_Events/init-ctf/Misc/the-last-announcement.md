# The Last Announcement

## Investigation

I started by checking the provided audio file:

```bash
file Train_Announcement.wav
exiftool Train_Announcement.wav
```

It was a 16-bit stereo WAV file with a sample rate of 48 kHz and a duration of around 17.39 seconds.

I decided to inspect the two audio channels separately, so I split them using FFmpeg:

```bash
ffmpeg -i Train_Announcement.wav -af "pan=mono|c0=FL" left.wav
ffmpeg -i Train_Announcement.wav -af "pan=mono|c0=FR" right.wav
```

Next, I generated spectrograms for the original audio and both channels:

```bash
ffmpeg -i Train_Announcement.wav -lavfi "showspectrumpic=s=2400x1200:legend=1:scale=log" -frames:v 1 spectrogram.png

ffmpeg -i left.wav -lavfi "showspectrumpic=s=2400x1200:legend=1:scale=log" -frames:v 1 left_spectrogram.png

ffmpeg -i right.wav -lavfi "showspectrumpic=s=2400x1200:legend=1:scale=log" -frames:v 1 right_spectrogram.png
```

I opened the generated spectrograms using `mpv`:

```bash
mpv spectrogram.png
mpv left_spectrogram.png
mpv right_spectrogram.png
```

The hidden flag was visible in the spectrogram.

## Flag

```text
InIt{h1d1ng_f1l3s_1n_4ud10}
```