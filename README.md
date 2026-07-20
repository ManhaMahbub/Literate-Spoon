# GNN-Based EMG Gesture Recognition (NinaPro DB1)

## Overview

This project builds a Graph Neural Network (GNN) to recognize hand gestures from surface
electromyography (EMG) signals. We use the **NinaPro DB1** dataset — 10-channel EMG recordings
from multiple subjects performing a series of hand and finger gestures. Raw EMG channels are
treated as **graph nodes**, with edges derived from inter-channel signal correlation, making
this a natural fit for graph-based modeling rather than a plain feature-table classifier.

The project follows a three-task pipeline over six weeks: (1) understand the data and review
related work, (2) build baseline models and a proposed GNN, (3) improve the model via ablation
and explain its decisions using explainability tools.

## Dataset

- **Source:** [NinaPro DB1](https://ninapro.hevs.ch/) (Kaggle mirror: `mansibmursalin/ninapro-db1-full-dataset`)
- **Signal:** 10-channel surface EMG, recorded continuously
- **Labels:** Gesture (`stimulus` / `restimulus`), repetition, subject, exercise
- **Task type:** Multi-class gesture classification from raw muscle signal

## Repository Structure

```
Group03_NinaProDB1_CSE475/
├── README.md
├── report/
│   ├── task1/Group03_NinaProDB1_task1_report.pdf
│   ├── task2/Group03_NinaProDB1_task2_report.pdf
│   └── task3/Group03_NinaProDB1_task3_report.pdf       
├── code/
│   ├── task1/Group03_NinaProDB1_task1_eda.ipynb
│   ├── task2/Group03_NinaProDB1_task2_baselines.ipynb
│   ├── task2/Group03_NinaProDB1_task2_proposed_model.ipynb
│   ├── task3/Group03_NinaProDB1_task3_improvement_ablation.ipynb
│   └── task3/Group03_NinaProDB1_task3_explainability.ipynb
├── related_work/
│   ├── Group03_NinaProDB1_related_work.tex
│   ├── references.bib
│   └── papers/                                         
└── models/
    ├── Group03_NinaProDB1_best.pth
    └── label_map.json
```

## Pipeline

| Task | Window | What happens |
|---|---|---|
| **Task 1** — Understand | 19–23 Jul 2026 | EDA (A–H) on windowed EMG features; related-work review (10 papers, 2022–2026); research gap identified |
| **Task 2** — Build | 26–30 Jul 2026 | ML baselines (SVM, RF, XGBoost, etc.); GNN proposed model with correlation-based channel graph; first results |
| **Task 3** — Improve | 2–6 Aug 2026 | Ablation study; 5-fold subject-wise CV with significance testing; SHAP + LIME explainability; final comparison to related work |

## How to Run

1. Clone this repository.
2. Download the NinaPro DB1 dataset from [Kaggle](https://www.kaggle.com/datasets/mansibmursalin/ninapro-db1-full-dataset) (or use the Kaggle notebook environment directly).
3. Open notebooks in order: `code/task1/` → `code/task2/` → `code/task3/`.
4. Each notebook is self-contained; run cells top to bottom.
5. Trained model checkpoints are saved to `models/`.

## Methodology Summary

- **Feature extraction:** Raw EMG is segmented into overlapping windows (200 samples, 100-sample
  step); six time-domain descriptors (RMS, MAV, WL, VAR, ZC, SSC) are computed per channel per
  window.
- **Graph construction:** Each of the 10 EMG channels is a node; edges are built from
  inter-channel signal correlation, computed **after** the train/test split to avoid leakage.
- **Evaluation:** Subject-wise (grouped) train/test split, 5-fold cross-validation, and a
  significance test (Wilcoxon signed-rank) against the best baseline, per course requirements.
- **Class imbalance:** The "rest" class dominates the dataset; macro-F1 and per-class recall are
  used as primary metrics rather than raw accuracy.


## Related Work

A structured review of 10 papers (2022–2026) on GNN/GCN-based EMG and physiological-signal
gesture recognition is available in `related_work/`, along with the identified research gap
motivating this project's approach.

