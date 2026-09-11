# Results summary

All numbers below are the ones plotted and tabulated in the README and on the
listening page, collected in one place.

## Comparison with published lightweight denoisers

Our full-size model next to published lightweight real-time denoisers. The
DNS OVRL scores are taken from each model's own publication and were not
measured on a common test set, so they indicate magnitude only.

| Year | Model | Params | MMAC / s | DNS OVRL ↑ |
|---|---|---|---|---|
| 2018 | RNNoise | ~60 K | ~40 | 2.53 |
| 2024 | FSPEN | 79 K | 89 | not published |
| 2025 | UL-UNAS | 171 K | 35 | 2.71 |
| 2026 | **Ours (W16)** | ≈ 24 K | 33 | **2.93** |

## Model variants — DNSMOS on the held-out test set

Training data: DNS-Challenge speech and noise, on-the-fly mixing, SNR uniformly
−5 … 15 dB, 4 s crops, 16 kHz. Scoring: DNSMOS (P.808 MOS; P.835 SIG / BAK / OVRL).

| Variant | Trainable params | MMAC / s | P.808 ↑ | SIG ↑ | BAK ↑ | OVRL ↑ |
|---|---|---|---|---|---|---|
| W16 | ≈ 24 K | 33 | 3.63 | 3.22 | 3.95 | 2.93 |
| W12 | ≈ 15 K | 22 | 3.50 | 3.15 | 3.88 | 2.84 |
| W8 | ≈ 7.9 K | 14 | 3.44 | 3.10 | 3.80 | 2.77 |
| W8 + KD | ≈ 7.9 K | 14 | 3.48 | 3.13 | 3.81 | 2.80 |

## Footprint

Embedded build: GCC 15, `-O3 -ffast-math`, single-precision hard-float; instruction
counts from an instruction-set emulator, 62.5 frames / s.

| | W16 | W12 | W8 + KD |
|---|---|---|---|
| Compute (M instructions / s) | 133.3 | 89.8 | 54.8 |
| Instructions per 16 ms frame | 2.13 M | 1.44 M | 0.88 M |
| Flash: code + weights | 121.7 KB | 86.4 KB | 58.3 KB |
| Library static RAM (`.bss` + `.data`) | 0 | 0 | 0 |
| Instance RAM (caller-provided) | 95.9 KB | 74.2 KB | 54.3 KB |
| Total | 217.6 KB | 160.6 KB | 112.6 KB |

## Host runtime (reference only)

Apple M1, clang 15 `-O3 -ffast-math`, single core, 60 s of audio, best of 3:

| | W16 | W12 | W8 + KD |
|---|---|---|---|
| Time per 16 ms frame | 128 µs | 90 µs | 56 µs |
| Real-time factor | 0.0080 | 0.0056 | 0.0035 |

## Listening clips — per-clip scores

DNSMOS on each published file; SI-SNR against the clean recording after removing
the library's 256-sample output delay. Raw values: `listening_clips_scores.csv`.

| Utterance | Variant | OVRL ↑ | SIG ↑ | BAK ↑ | P.808 ↑ | SI-SNR (dB) ↑ |
|---|---|---|---|---|---|---|
| p232_005 | Noisy input | 2.51 | 3.55 | 2.54 | 2.87 | 1.9 |
| p232_005 | W16 | 2.96 | 3.27 | 3.91 | 3.84 | 11.8 |
| p232_005 | W12 | 3.04 | 3.31 | 4.02 | 3.80 | 13.2 |
| p232_005 | W8 + KD | 2.82 | 3.13 | 3.86 | 3.80 | 12.6 |
| p232_005 | Clean reference | 3.40 | 3.63 | 4.13 | 4.26 | – |
| p232_070 | Noisy input | 2.85 | 3.60 | 3.20 | 3.34 | 11.3 |
| p232_070 | W16 | 2.94 | 3.31 | 3.81 | 3.61 | 14.9 |
| p232_070 | W12 | 3.21 | 3.44 | 4.10 | 3.62 | 15.5 |
| p232_070 | W8 + KD | 3.08 | 3.35 | 4.03 | 3.72 | 14.6 |
| p232_070 | Clean reference | 3.37 | 3.61 | 4.12 | 3.68 | – |
| p232_358 | Noisy input | 3.21 | 3.54 | 4.00 | 3.23 | 15.5 |
| p232_358 | W16 | 3.24 | 3.51 | 4.10 | 3.41 | 19.6 |
| p232_358 | W12 | 3.24 | 3.48 | 4.13 | 3.52 | 21.9 |
| p232_358 | W8 + KD | 3.22 | 3.45 | 4.15 | 3.53 | 21.7 |
| p232_358 | Clean reference | 3.31 | 3.60 | 4.07 | 3.22 | – |
| p257_240 | Noisy input | 2.71 | 3.57 | 2.98 | 3.06 | 16.7 |
| p257_240 | W16 | 2.68 | 3.15 | 3.51 | 3.20 | 19.9 |
| p257_240 | W12 | 2.58 | 2.90 | 3.92 | 3.29 | 18.3 |
| p257_240 | W8 + KD | 2.72 | 3.11 | 3.69 | 3.34 | 17.9 |
| p257_240 | Clean reference | 3.13 | 3.49 | 3.89 | 3.42 | – |
| p257_430 | Noisy input | 2.50 | 3.52 | 2.60 | 2.65 | 6.7 |
| p257_430 | W16 | 2.64 | 2.93 | 3.84 | 3.31 | 12.8 |
| p257_430 | W12 | 2.69 | 2.94 | 3.99 | 3.39 | 12.7 |
| p257_430 | W8 + KD | 2.58 | 2.91 | 3.76 | 3.26 | 11.4 |
| p257_430 | Clean reference | 2.93 | 3.35 | 3.78 | 3.38 | – |
| **mean** | Noisy input | 2.76 | 3.55 | 3.06 | 3.03 | 10.4 |
| **mean** | W16 | 2.89 | 3.24 | 3.83 | 3.47 | 15.8 |
| **mean** | W12 | 2.95 | 3.21 | 4.03 | 3.52 | 16.3 |
| **mean** | W8 + KD | 2.88 | 3.19 | 3.90 | 3.53 | 15.6 |
| **mean** | Clean reference | 3.23 | 3.53 | 4.00 | 3.59 | – |

