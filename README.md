# CNN-Att

Convolutional attention modeling improves copy number variant classification from whole exome sequencing.

## Overview

CNN-Att is a dual-input convolutional neural network with attention for exon-level copy number variant (CNV) classification from whole-exome sequencing (WES) data.

Each exon-centered window is classified into:
- 0 — No-call  
- 1 — Deletion (DEL)  
- 2 — Duplication (DUP)

The model integrates normalized read depth, genomic coordinates, and chromosome identity, as described in the manuscript.

---

## Input Format

Each exon window must contain:

- Columns 0–1001: 1002 normalized depth values  
- Column 1002: Genomic start coordinate  
- Column 1003: Genomic end coordinate  
- Column 1004: Chromosome ID  
- Label column: 0 (No-call), 1 (DEL), 2 (DUP)

Notes:
- Depth values are min–max normalized to [0,1].
- Missing/padded depth tokens are encoded as −1.
- Chromosomes are one-hot encoded (chr1–22, X, Y).
- The model produces one prediction per exon window.

---

## Training Summary

Pretraining:
- 300 ECOLE samples  
- 435,251 exon windows  
- 70/30 stratified train/validation split  
- SMOTE applied to training partition only (random_state=42)  
- Adam optimizer (LR=0.001)  
- Early stopping (patience=10)

Fine-tuning:
- 5 expert-labeled samples (training)
- 2 expert-labeled samples (evaluation)
- Encoder frozen
- LR=0.0002

Pretrained and fine-tuned weights are provided in this repository.

---

## Stratified CNV Frequency Analysis

The notebook `Training_Frequency_Stratified_CNV_Performance.ipynb` reproduces the stratified evaluation described in Supplementary Table S7.

This analysis:
- Computes per-exon CNV frequency in the 300-sample training set
- Stratifies true CNV test windows into:
  - Never (0%)
  - Rare (0–5%)
  - Often (5–50%)
  - Majority (>50%)
- Reports CNV, DEL, and DUP recall per stratum


---

## Reproducing Results

1. Obtain ECOLE exon-level matrices and labels:
   https://zenodo.org/record/8202814

2. Prepare input in the format described above.

3. Use `Notebook.ipynb` for training or evaluation.

---

## Requirements

- Python 3.9+
- TensorFlow 2.x
- scikit-learn
- imbalanced-learn
- NumPy
- Pandas

GPU acceleration is recommended.

---

## Citation

If you use this repository, please cite:

Ouhmouk M., Abik M.  
Convolutional attention modeling improves copy number variant classification from whole exome sequencing.
