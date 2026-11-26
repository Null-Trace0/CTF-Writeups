# Beepy

## Investigation

I started by checking the audio file:

```bash
file audio.wav
ffprobe audio.wav
```

It was an 8-bit mono WAV file with a sample rate of 8000 Hz and a duration of 18.48 seconds.

I didn't have Audacity installed, so I used Python's `wave` module to inspect the audio and find the short and long beeps.

I wrote a small script to detect the sound segments and measure their durations:

```python
import wave
import numpy as np

with wave.open("audio.wav", "rb") as wav:
    rate = wav.getframerate()
    frames = wav.readframes(wav.getnframes())
    samples = np.frombuffer(frames, dtype=np.uint8)

threshold = 20
active = np.abs(samples.astype(int) - 128) > threshold
changes = np.diff(np.r_[False, active, False].astype(int))
starts = np.where(changes == 1)[0]
ends = np.where(changes == -1)[0]
segments = []
for start, end in zip(starts, ends):
    duration = (end - start) / rate
    segments.append((start, end, duration))

for i, (start, end, duration) in enumerate(segments):
    gap = 0
    if i + 1 < len(segments):
        next_start = segments[i + 1][0]
        gap = (next_start - end) / rate

    print(f"Pulse: {duration:.3f}s | Gap: {gap:.3f}s")
```

The pulse durations were approximately `0.059s` and `0.179s`, which I identified as dots and dashes. The gaps were around `0.061s` within a Morse character and `0.181s` between characters.

Using those timings, I converted the beeps into Morse code:

```text
-... ...-- ...-- .--. -.-- ..--.- -- ----- .-. ... ...-- ..--.- .---- ... ..--.- -.-. ----- ----- .-..
```

I decoded the sequence using International Morse code and got:

```text
B33PY_M0RS3_1S_C00L
```

## Flag

```text
InIt{B33PY_M0RS3_1S_C00L}
```