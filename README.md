<div align="center">

# Yaaran — یاراں
### Burushaski to English Speech Translation System

*First open-source ASR pipeline for Burushaski — an endangered language isolate spoken in Gilgit-Baltistan, Pakistan*

![Python](https://img.shields.io/badge/Python-3.10+-blue) ![React](https://img.shields.io/badge/React-18-61DAFB) ![Flutter](https://img.shields.io/badge/Flutter-3.x-02569B) ![Whisper](https://img.shields.io/badge/OpenAI-Whisper-412991) ![License](https://img.shields.io/badge/License-MIT-green)

**Habib University — Senior Year Project 2025**

| Team | Role |
|---|---|
| Azkaa Nasir | |
| Fatima Faisal | |
| Adina Adnan Mansoor | |
| Mahrukh Yousuf | |

**Supervised by:** Tauqeer Saleem · Dr. Abdul Samad

</div>

---

## Overview

Burushaski is a **language isolate** spoken by approximately 70,000–100,000 people in the Gilgit-Baltistan region of Pakistan. It has no known linguistic relatives, no standardised writing system, and until this work, no automatic speech recognition system.

**Yaaran** (یاراں — "friends" in Burushaski) is an end-to-end speech translation ecosystem that enables:
- Real-time Burushaski speech → English text translation
- Community-driven data collection and corpus building
- Continuous model improvement through user feedback

This project represents the **first documented ASR baseline for Burushaski**, fine-tuned on a dataset of ~12,000 audio-text pairs collected and curated as part of this work.

---

## Repository Structure

```
yaaran/
│
├── translation-app/          # Main translation application
│   ├── mobile/               # Flutter mobile app (Android)
│   ├── web/                  # React web frontend
│   └── backend/              # Python inference server
│
├── data-collection-pwa/      # Progressive Web App for corpus building
│   ├── frontend/             # React PWA
│   └── backend/              # Supabase (managed)
│
├── dashboard/                # Analytics & monitoring dashboard
│
├── model/
│   ├── training/             # Fine-tuning scripts
│   ├── augmentation/         # Data augmentation pipeline
│   └── evaluation/           # Evaluation scripts & metrics
│
└── README.md
```

---

## System Components

### 1. Translation App
The primary user-facing application for Burushaski → English speech translation.

**Features:**
- Record Burushaski speech → receive English translation
- Suggest corrections to translations (crowdsourced improvement loop)
- Available as Android mobile app and web application

**Stack:**
| Layer | Technology |
|---|---|
| Mobile frontend | Flutter (Android) |
| Web frontend | React |
| Inference backend | Python (FastAPI) |
| Model | Fine-tuned Whisper Large-v2 |

---

### 2. Data Collection PWA
A Progressive Web App enabling community volunteers to contribute to the Burushaski corpus.

**Features:**
- Volunteers read standardised Burushaski transliterations and record audio
- Submit feedback and corrections on existing transcriptions
- Accessible on any device via browser — no installation required

**Stack:**
| Layer | Technology |
|---|---|
| Frontend | React (PWA) |
| Backend & Database | Supabase |

---

### 3. Dashboard
Internal monitoring and analytics dashboard tracking dataset growth and collection quality from the PWA.

**Features:**
- Dataset statistics (total recordings, hours, speaker distribution)
- Submission quality metrics
- Feedback and correction tracking

---

## Model

### Architecture
Fine-tuned **OpenAI Whisper** (encoder-decoder transformer) for direct Burushaski speech → English text translation in a single pass.

### Training Cycles

| Cycle | Train Size | Test Size | Model | WER ↓ | BLEU ↑ |
|---|---|---|---|---|---|
| 1 | 7,115 | 1,779 | Whisper Small | 34.33% | 65.42 |
| 1 | 7,115 | 1,779 | Whisper Medium | 25.71% | 69.94 |
| 1 | 7,115 | 1,779 | Whisper Large-v2 | 24.54% | 70.90 |
| 2 | 11,976 | 2,994 | Whisper Small | 24.33% | 74.10 |
| 2 | 11,976 | 2,994 | Whisper Medium | 17.28% | 79.87 |
| 2 | 11,976 | 2,994 | Whisper Large-v2 | 16.38% | 80.53 |
| 3 (aug) | 11,976 + 9,000 aug | 2,994 | Whisper Medium | 11.57% | 86.43 |

### Data Augmentation
Augmentation techniques were selected via ablation study on Whisper Small:

| Technique | Purpose | WER Impact |
|---|---|---|
| RIR (Room Impulse Response) | Acoustic environment robustness | Best single augmentation |
| VAD (Voice Activity Detection) | Silence removal, cleaner signal | Consistent improvement |
| Pitch Shift | Speaker diversity simulation | Positive on medium+ models |
| Gaussian noise + speed | Tested, rejected | Degraded performance (WER +15%) |

### Evaluation Metrics
- **WER** — Word Error Rate (primary ASR metric)
- **CER** — Character Error Rate
- **BLEU** — Translation quality (Papineni et al., 2002)
- **chrF++** — Character n-gram F-score, recommended for morphologically rich languages (Popović, 2017)
- **BERTScore F1** — Semantic similarity (Zhang et al., 2020)

---

## Language Preservation

Burushaski faces accelerating endangerment as younger speakers shift to Urdu and English for digital communication. Yaaran addresses this through three pathways:

1. **Documentation** — transcribing audio into searchable, archivable text
2. **Accessibility** — enabling Burushaski speakers to use technology in their native language
3. **Corpus building** — every recording via the PWA expands the only structured Burushaski speech dataset in existence

> *"A language dies when its last speaker dies. A language is preserved when it lives in the tools its speakers use every day."*

---

## Setup

### Inference Backend
```bash
git clone https://github.com/your-username/yaaran.git
cd yaaran/translation-app/backend

pip install -r requirements.txt

# Point to your model path
python app.py --model_path ./whisper_burushaski_large_final
```

### Data Collection PWA
```bash
cd yaaran/data-collection-pwa/frontend
npm install
npm run build
# Configure Supabase credentials in .env
```

### Mobile App
```bash
cd yaaran/translation-app/mobile
flutter pub get
flutter run
```

---

## Requirements

```
transformers>=4.36.0
torch>=2.0.0
faster-whisper
librosa
soundfile
fastapi
uvicorn
```

---

## Citation

```bibtex
@thesis{yaaran2025,
  title     = {Yaaran: Automatic Speech Recognition and Translation for Burushaski},
  author    = {Nasir, Azkaa and Faisal, Fatima and Mansoor, Adina Adnan and Yousuf, Mahrukh},
  year      = {2025},
  school    = {Habib University},
  note      = {First ASR baseline for Burushaski language},
  supervisor = {Tauqeer Saleem and Dr. Abdul Samad}
}
```

---

## References

- Radford et al. (2023) — *Robust Speech Recognition via Large-Scale Weak Supervision* (Whisper)
- Ko et al. (2015) — *Audio Augmentation for Speech Recognition*
- Papineni et al. (2002) — *BLEU: a Method for Automatic Evaluation of Machine Translation*
- Popović (2017) — *chrF++: words helping character n-grams*
- Zhang et al. (2020) — *BERTScore: Evaluating Text Generation with BERT*
- Bird (2020) — *Decolonising Speech and Language Technology*
- UNESCO (2010) — *Atlas of the World's Languages in Danger*
- Conneau et al. (2020) — *Unsupervised Cross-lingual Representation Learning for Speech*
- Babu et al. (2022) — *XLS-R: Self-supervised Cross-lingual Speech Representation Learning at Scale*

---

## License

MIT License — see [LICENSE](LICENSE)

---

<div align="center">
Built at Habib University with the goal of ensuring Burushaski lives in the digital world.<br>
یاراں — For the speakers of Gilgit-Baltistan.
</div>
