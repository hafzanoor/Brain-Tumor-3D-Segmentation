# 3D Multimodal Brain Tumor Segmentation (in progress)

Groundwork for my thesis research on brain tumor segmentation, supervised by Dr. Fiza Zafar at
Bahauddin Zakariya University, Multan. The pipeline is built on a public benchmark first; the next
step is extension to real-world clinical MRI data.

**Status:** pipeline complete; full training and test-set evaluation in progress. Results will be
added here once training is finished.

## Data
- **Medical Segmentation Decathlon, Task01 Brain Tumour** (derived from BraTS): 484 labelled 3D MRI
  volumes, each with four modalities: FLAIR, T1w, T1gd (contrast-enhanced) and T2w.
- **Patient-level split:** 338 train / 73 validation / 73 test patients (`patient_splits.csv`).
  Splitting by patient, not by slice, prevents the train-test leakage that inflates many 2D results.
- The dataset itself is not included in this repository; it is available from the Decathlon website
  and on Kaggle.

## Method
| Step | Choice |
|---|---|
| Normalization | Per-modality z-score computed on brain voxels only; background stays 0 |
| Targets | Three overlapping regions: Whole Tumor (labels 1-3), Tumor Core (2-3), Enhancing Tumor (3) |
| Model | Lightweight 3D U-Net (16-32-64-128 channels), instance normalization, transposed-convolution decoder with skip connections |
| Loss / optimizer | Soft Dice loss on sigmoid outputs, Adam (lr = 1e-4) |
| Training | Random 128 x 128 x 128 patches (batch size 1) on a Kaggle Tesla T4 GPU |
| Validation | Deterministic centre crop; best checkpoint saved by validation Dice loss |
| Evaluation | Full-volume inference on test patients; Dice per region (WT, TC, ET) |

## Results
| Region | Test Dice (mean +/- std) |
|---|---|
| Whole Tumor | to be added |
| Tumor Core | to be added |
| Enhancing Tumor | to be added |

## Notebooks
- `01_training_and_evaluation.ipynb`: data loading, preprocessing, 3D U-Net, training loop and test-set Dice evaluation
- `02_segmentation_app.ipynb`: Gradio prototype that segments an uploaded 4-channel NIfTI volume, reports
  region volumes, shows an axial slice overlay, and retrieves related passages from neuro-oncology
  textbooks (MiniLM embeddings + FAISS). Textbooks are not included for copyright reasons.

## Related work
- [Explainable Brain Tumor MRI Classification with Uncertainty-Aware Decision Support](https://github.com/hafzanoor/MRI_Agent_Gradio):
  2D classification, train-test leakage audit and calibration analysis.

> Research prototype for educational use only. Not for clinical diagnosis.

Author: Hafza Noor
