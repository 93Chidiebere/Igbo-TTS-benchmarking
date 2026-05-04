# Benchmarking Multimodal Semantic Alignment Between Speech and Text in Igbo

## Overview
This repository accompanies the research paper:

**“Benchmarking Multimodal Semantic Alignment Between Speech and Text Representations in the Igbo Language”**

This project presents the **first systematic benchmark** for cross-modal alignment between speech and text embeddings in Igbo, a low-resource African language with complex tonal and orthographic properties.

The work establishes:
- A **zero-shot baseline** showing complete misalignment between speech and text representations
- A **lightweight contrastive learning approach** to bridge the modality gap
- A **reproducible benchmark** for future research in African NLP

---

## Key Findings

- **Zero-shot alignment fails completely**  
  Paired cosine similarity ≈ **-0.0009** (statistically indistinguishable from random)

- **Lightweight projection significantly improves alignment**
  - Speech → Text Recall@10: **0.3362**
  - Text → Speech Recall@10: **0.3777**
  - Up to **5.1× improvement** over baseline

- **Statistically significant alignment achieved**
  - t = 15.95  
  - p ≈ 2.81 × 10⁻⁵⁴  

---

## Dataset

Due to size constraints, the dataset is **not hosted on GitHub**.

### Download Dataset
👉 **[https://drive.google.com/drive/folders/1siohiCmy9ZWC0yPiCRGpvqfNR-zFe5k0?usp=sharing]**

---

### Dataset Description
- Source: WAXAL African Speech Corpus  
- Language: Igbo  
- Total validated samples: **1,343**
- Subsample used for experiments: **699 speech-text pairs**
- Average duration: **~12.3 seconds**
- Speakers: 8 (balanced gender distribution)

Each record contains:
- Audio (MP3 bytes)
- Text transcription (Igbo with diacritics)
- Speaker metadata (ID, gender)
- Duration

---

## Methodology

### 1. Representation Learning
- **Speech Encoder:** Whisper-tiny (384-dimensional embeddings)
- **Text Encoder:** Multilingual MiniLM (384-dimensional embeddings)

### 2. Zero-Shot Evaluation
- Direct cosine similarity between embedding spaces
- Result: No meaningful alignment

### 3. Alignment Model
A **single-layer linear projection** maps speech embeddings into the text embedding space:

- Parameters: ~147K
- Loss: Symmetric InfoNCE
- Temperature: 0.07
- Training time: ~1.6 seconds (CPU)

### 4. Hard Negative Mining
- Top-k most similar incorrect samples selected as negatives
- Re-mined every **7 epochs** (curriculum learning)

---

## Evaluation Protocol

Two retrieval tasks:
- Speech → Text
- Text → Speech

Metrics:
- Recall@K (K = 1, 5, 10)
- Mean Reciprocal Rank (MRR)
- Mean Rank / Median Rank

Evaluation setup:
- Candidate pool size: 100 (1 correct + 99 distractors)

---

## Results Summary

| Method     | Direction | R@1   | R@10  | MRR   |
|------------|----------|------|------|------|
| Zero-shot  | S→T      | 0.0129 | 0.1102 | 0.0577 |
| Projected  | S→T      | 0.0658 | 0.3362 | 0.1557 |
| Zero-shot  | T→S      | 0.0129 | 0.1245 | 0.0612 |
| Projected  | T→S      | 0.0644 | 0.3777 | 0.1638 |

---

## Repository Structure
├── data/ # (Not included in repo)
├── embeddings/ # Cached embeddings
├── cache/ # Model cache
├── notebooks/ # Experiment notebooks
├── src/ # Core implementation (optional)
├── README.md
└── requirements.txt

## Reproducibility

To reproduce results:

Download dataset from Google Drive
Link: https://drive.google.com/drive/folders/1siohiCmy9ZWC0yPiCRGpvqfNR-zFe5k0?usp=sharing

## Key Insights
Multilingual models do not generalize to Igbo in zero-shot settings
Alignment requires explicit supervision
Small models + limited data can still yield meaningful improvements
Tonal and diacritic-rich languages introduce unique alignment challenges

## Limitations
Small number of speakers (8)
Studio-recorded speech only
Linear projection limits model expressiveness
No explicit tonal modeling
Future Work
Incorporate larger speech datasets (e.g., FLEURS)
Use stronger encoders (Whisper-base, LaBSE)
Explore non-linear alignment architectures
Introduce tone-aware representations