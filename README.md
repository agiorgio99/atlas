# ATLAS: Automatic Transcription and Lyric Alignment System

> **Evaluating Lyric Transcription and Alignment Robustness Across Expressive Singing Styles**

**Group Members:** Antonello Giorgio · Hemanth Ramia Jegdish · Polyxeni Pouliou

---

## Overview

ATLAS benchmarks automatic lyric alignment and transcription systems under expressive singing conditions. Singing introduces acoustic phenomena absent from speech — vibrato, melisma, pitch slides, and dynamic variations — that challenge models trained predominantly on speech data. This project evaluates whether music-aware systems offer a meaningful advantage over speech-based baselines when applied to expressive vocal recordings.

**Central Research Question:**
> How robust are current automatic lyric transcription and forced-alignment systems when confronted with expressive, stylistically varied singing, and do singing-specific models outperform general speech models on this task?

---

## Tasks

### 1. Transcription Evaluation
Applying **FireRedASR** (SOTA speech-based ASR) to the GTSinger dataset to produce transcribed lyrics, measured against ground truth using:
- **WER** (Word Error Rate) — fraction of words incorrectly transcribed
- **PER** (Phoneme Error Rate) — phoneme-level transcription accuracy

### 2. Alignment Evaluation
Using **Montreal Forced Aligner (MFA)** to align both ground-truth and transcribed lyrics to audio, measuring alignment precision via:
- **TBE** (Time Boundary Error) — mean absolute difference (ms) between predicted and annotated word/phoneme onset and offset boundaries

### 3. Pitch-Informed Analysis
Applying **CREPE** for pitch contour extraction to characterise the degree of expressiveness (vibrato, melisma) in each recording, enabling correlation of acoustic complexity with transcription/alignment error.

### 4. Music-Aware Adaptation
Adapting **MM-ALT** (a multimodal lyric transcription system) to GTSinger and comparing its performance against the speech-based pipeline.

---

## Baseline

The combination of **FireRedASR + MFA** constitutes the primary speech-based baseline against which MM-ALT and other music-aware configurations are compared.

---

## Dataset

### GTSinger | [https://github.com/AaronZ345/GTSinger](https://github.com/AaronZ345/GTSinger) |
A large-scale, multi-style, multilingual singing voice dataset with precise phoneme-level annotations.

| Property | Details |
|---|---|
| Total duration | 80.59 hours |
| Singers | 20 professional singers |
| Languages | Chinese, English, Japanese, Korean, Russian, Spanish, French, German, Italian |
| Singing techniques annotated | Mixed voice, falsetto, breathy, pharyngeal, vibrato, glissando |
| Accompaniment | None (clean vocals) |

> All models are evaluated on the **English subset** of GTSinger.

---

## Tools & Models

| Tool / Model | Role | Repository |
|---|---|---|
| **MFA** | Forced alignment (baseline) | [github.com/MontrealCorpusTools/Montreal-Forced-Aligner](https://github.com/MontrealCorpusTools/Montreal-Forced-Aligner) |
| **FireRedASR** | ASR / transcription (SOTA) | [github.com/FireRedTeam/FireRedASR](https://github.com/FireRedTeam/FireRedASR) |
| **CREPE** | Pitch tracking | [github.com/marl/crepe](https://github.com/marl/crepe) |
| **MM-ALT** | Multimodal lyric transcription | [github.com/guxm2021/MM_ALT](https://github.com/guxm2021/MM_ALT) |

---

## Evaluation Plan

Results are broken down by singing style to identify where music-aware systems outperform speech-based ones. The key comparisons are:

- **Transcription accuracy** — WER and PER for FireRedASR vs. MM-ALT
- **Alignment quality** — MFA performance using both ground-truth and ASR-generated lyrics, measuring how transcription errors propagate to alignment
- **Expressiveness correlation** — whether higher acoustic complexity (e.g. strong vibrato, pitch slides detected by CREPE) correlates with larger alignment errors
