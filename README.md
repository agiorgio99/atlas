# ATLAS: Automatic Transcription and Lyric Alignment System

**Evaluating Lyric Transcription and Alignment Robustness Across Expressive Singing Styles**

> Music Information Retrieval — Group Project
> Universitat Pompeu Fabra, Barcelona, 2026
>
> **Authors:** Antonello Giorgio · Hemanth Ramia Jegdish · Polyxeni Pouliou

---

## Overview

ATLAS benchmarks automatic lyric transcription and phoneme-level forced alignment on expressive singing, using the English subset of the [GTSinger](https://github.com/GTSinger/GTSinger) dataset. We evaluate three ASR models and two forced alignment systems across five singing techniques (vibrato, breathy, pharyngeal, glissando, mixed voice & falsetto), and complement the analysis with CREPE-based pitch contour extraction.

**Key findings:**
- Whisper large-v3 achieves **18.1% WER** on singing; FireRedASR and wav2vec2 exceed **120% WER** with hallucination rates above 36%
- MFA and SOFA achieve comparable boundary MAE (~48–49 ms), but performance reverses across singers
- Glissando is the most acoustically distinctive technique (2.44× increase in within-phoneme F0 std)

---

## Repository Structure

```
atlas/
│
├── Alignment Results - MFA vs SOFA/    ← MFA & SOFA output TextGrids and evaluation CSVs
├── metadata2/                           ← Utterance inventory, TextGrid analysis, dataset stats
├── results/                             ← ASR model outputs: WER/PER CSVs and plots
│                                          (FireRedASR, wav2vec2, CREPE)
├── whisper_results/                     ← Whisper-specific WER/PER CSVs and plots
│
├── atlas_check_the_dataset.ipynb        ← Dataset exploration and validation
├── atlas_data_extraction_preparation.ipynb   ← Data prep, MFA corpus creation, manifests
├── atlas_textgrid_analysis.ipynb        ← TextGrid structure, phoneme duration analysis
├── atlas_fireredasr.ipynb               ← FireRedASR transcription + WER/PER evaluation
├── atlas_wav2vec2.ipynb                 ← wav2vec2 transcription + WER/PER evaluation
├── atlas_whisper.ipynb                  ← Whisper (small / large-v2 / large-v3) transcription
├── atlas_alignment_final_notebook.ipynb ← MFA vs SOFA alignment evaluation
├── atlas_crepe.ipynb                    ← CREPE pitch extraction and F0 analysis
│
└── README.md
```

---

## Data

The GTSinger English subset is **not included in this repository** due to its size. All data assets are hosted on Google Drive:

| Asset | Description | Link |
|---|---|---|
| `English.zip` | Full GTSinger English subset (raw, zipped) | [Download](https://drive.google.com/file/d/1DK1vnTs_d9JD7Xya4U5CEaXszIWyboFv/view?usp=drive_link) |
| `english_raw/` | Unzipped WAV + TextGrid files | [Drive folder](https://drive.google.com/drive/folders/1js-mjuUJP3AC9I5lhVlBB2ctxGaVHTVX?usp=sharing) |
| `english_prepared/` | Organised corpus for pipeline (singing/speech split) | [Drive folder](https://drive.google.com/drive/folders/1MjFnwpnDgs7toUL_QoerCfgsFJFnF58h?usp=sharing) |
| `metadata/` | CSVs, WAV manifests, MFA corpus `.lab` files | [Drive folder](https://drive.google.com/drive/folders/1_2pEDcStx_wExf5LakW93hvEU_a7Ykw0?usp=sharing) |
| `whisper_weights/` | Cached Whisper model weights (.pt files) | [Drive folder](https://drive.google.com/drive/folders/1hwA_jt48b1ZRdT9gmwpzf5SPLcKtpJFF?usp=sharing) |

> **Dataset:** GTSinger — 3 English singers (EN-Alto-1, EN-Alto-2, EN-Tenor-1), 6,892 utterances, 5 techniques, 48 kHz WAV, phoneme-level TextGrid annotations.

---

## Models Used

| Model | Role | Source |
|---|---|---|
| [FireRedASR-AED-L](https://github.com/FireRedTeam/FireRedASR) | ASR baseline (seq2seq) | HuggingFace: `FireRedTeam/FireRedASR-AED-L` |
| [wav2vec2-large-960h](https://huggingface.co/facebook/wav2vec2-large-960h) | ASR baseline (CTC) | HuggingFace: `facebook/wav2vec2-large-960h` |
| [Whisper](https://github.com/openai/whisper) | ASR (small / large-v2 / large-v3) | OpenAI |
| [MFA](https://github.com/MontrealCorpusTools/Montreal-Forced-Aligner) | Forced alignment (speech-trained) | `english_mfa` pretrained model |
| [SOFA](https://github.com/qiuqiao/SOFA) | Forced alignment (singing-trained) | Checkpoint: `tgm_en_v100` |
| [CREPE](https://github.com/marl/crepe) | Pitch extraction (F0) | `tiny` model |

---

## Notebooks

Run the notebooks in the following order. All Colab notebooks expect the Google Drive to be mounted at `/content/drive/MyDrive/atlas/`.

| # | Notebook | Environment | Description |
|---|---|---|---|
| 1 | `atlas_check_the_dataset.ipynb` | Google Colab | Inspect GTSinger structure, validate TextGrids, compute audio stats |
| 2 | `atlas_data_extraction_preparation.ipynb` | Google Colab | Unzip, organise corpus, extract GT lyrics, create MFA `.lab` files |
| 3 | `atlas_textgrid_analysis.ipynb` | Google Colab | Analyse TextGrid tiers, phoneme durations, annotation coverage |
| 4 | `atlas_fireredasr.ipynb` | Google Colab (GPU) | Transcribe singing & speech with FireRedASR; compute WER/PER |
| 5 | `atlas_wav2vec2.ipynb` | Google Colab (GPU) | Transcribe with wav2vec2-large-960h; compute WER/PER |
| 6 | `atlas_whisper.ipynb` | Google Colab (A100) | Transcribe with Whisper small/large-v2/large-v3; cross-model comparison |
| 7 | `atlas_alignment_final_notebook.ipynb` | Local (Windows) | Evaluate MFA vs SOFA on phoneme boundary MAE, IoU, TBE |
| 8 | `atlas_crepe.ipynb` | Google Colab | Extract F0 with CREPE; correlate pitch features with technique annotations |

---

## Results Summary

### ASR — Singing (3,646 utterances, technique + control groups)

| Model | WER% | PER% | Hallucination% | WER% (no halluc) |
|---|---|---|---|---|
| Whisper large-v3 | **18.1** | **11.2** | 1.1 | 16.7 |
| Whisper large-v2 | 18.8 | 12.4 | 1.2 | 17.2 |
| Whisper small | 25.3 | 15.4 | 1.3 | 23.5 |
| FireRedASR-AED-L | 120.3 | 103.6 | 36.1 | 85.8 |
| wav2vec2-large-960h | 123.5 | 104.6 | 38.2 | 88.3 |

Sanity checks on LibriSpeech test-clean: FireRedASR **2.2% WER**, wav2vec2 **4.3% WER** (0% hallucinations).

### Alignment — Matched subset (MFA: 747 utt., SOFA: 485 utt.)

| System | Boundary MAE (ms) | IoU | Recall | TBE@20ms | TBE@50ms |
|---|---|---|---|---|---|
| SOFA | **48.26** | 0.636 | 0.737 | 49.7% | 25.2% |
| MFA | 49.29 | **0.683** | **0.760** | **40.0%** | **17.6%** |

Singer-level: MFA wins on EN-Alto-1 and EN-Alto-2; SOFA wins on EN-Tenor-1 (43.2 vs 60.0 ms).

### CREPE — Within-phoneme F0 std ratio (technique present vs absent)

| Technique | F0 std with (Hz) | F0 std without (Hz) | Ratio |
|---|---|---|---|
| Glissando | 16.5 | 6.8 | **2.44×** |
| Vibrato | 8.8 | 7.5 | 1.18× |
| Pharyngeal | 8.5 | 7.4 | 1.16× |
| Mix / Falsetto / Breathy | < 7.2 | > 7.7 | < 1.0× |

---

## Requirements

```bash
pip install torch torchaudio transformers datasets
pip install jiwer g2p_en librosa soundfile tgt tqdm pandas matplotlib seaborn
pip install crepe huggingface_hub
# For FireRedASR: clone https://github.com/FireRedTeam/FireRedASR
# For MFA: see https://montreal-forced-aligner.readthedocs.io
# For SOFA: see https://github.com/qiuqiao/SOFA
```

Audio conversion to 16 kHz requires `ffmpeg`:
```bash
sudo apt-get install ffmpeg   # Linux / Colab
brew install ffmpeg           # macOS
```

---

## Citation

If you use this work, please cite GTSinger and the relevant model papers:

```bibtex
@article{zhang2024gtsinger,
  title   = {{GTSinger}: A Global Multi-Technique Singing Corpus with
             Realistic Music Scores for All Singing Tasks},
  author  = {Zhang, Yu and Pan, Changhao and others},
  journal = {Advances in Neural Information Processing Systems},
  volume  = {37},
  pages   = {1117--1140},
  year    = {2024}
}
```
