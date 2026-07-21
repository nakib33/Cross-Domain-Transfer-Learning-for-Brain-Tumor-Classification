# Cross-Domain Transfer Learning for Brain Tumor Classification

## Abstract

This repository presents a comprehensive deep learning framework for evaluating cross-domain transfer learning in brain tumor MRI classification under limited training data. The study investigates how ImageNet-pretrained EfficientNet-B0 compares with training from scratch across multiple training data fractions.

A progressive three-stage fine-tuning strategy, extensive checkpoint recovery system, and multi-seed evaluation are implemented to ensure reproducible and reliable experiments. Model performance is evaluated using Accuracy, Macro F1-score, and the proposed Transfer Efficiency Index (TEI), providing quantitative insight into the effectiveness of transfer learning under different data availability scenarios.

---

## Project Overview

The primary objective of this project is to evaluate whether transfer learning provides significant benefits when limited annotated medical imaging data are available.

Experiments compare two training strategies:

- ImageNet Pretrained EfficientNet-B0
- EfficientNet-B0 Trained from Scratch

Both approaches are evaluated using identical experimental settings over six different training data fractions.

Training fractions:

- 5%
- 10%
- 20%
- 30%
- 50%
- 100%

Each experiment is repeated using three independent random seeds to reduce experimental bias.

---

## Dataset

### Brain Tumor MRI Dataset

Dataset contains four classes:

- Glioma
- Meningioma
- No Tumor
- Pituitary Tumor

Dataset Statistics

| Split | Images |
|--------|--------|
| Training | 5,712 |
| Testing | 1,488 |
| Total | 7,200 |

Image Size

```
224 × 224 RGB
```

---

## Model Architecture

Base Model

```
EfficientNet-B0
```

Transfer Learning Initialization

```
ImageNet Pretrained Weights
```

Classification Head

```
Global Average Pooling
↓
Dropout
↓
Fully Connected Layer
↓
4 Output Classes
```

Loss Function

```
Focal Loss
```

Optimizer

```
AdamW
```

Learning Rate Scheduler

```
CosineAnnealingWarmRestarts
```

---

## Three-Stage Fine-Tuning Strategy

### Stage 1

Epochs

```
1 – 10
```

Training

- Freeze backbone
- Train classifier only

---

### Stage 2

Epochs

```
11 – 25
```

Training

- Unfreeze top two EfficientNet blocks
- Fine tune higher-level features

---

### Stage 3

Epochs

```
26+
```

Training

- Unfreeze complete network
- Fine tune entire model using a very small learning rate

---

## Experimental Design

Each experiment consists of

```
6 Data Fractions
×
2 Training Modes
×
3 Random Seeds
=
36 Total Experiments
```

Training Modes

- Transfer Learning
- Training From Scratch

---

## Training Pipeline

```
MRI Images
      │
      ▼
Data Augmentation
      │
      ▼
Train / Validation Split
      │
      ▼
EfficientNet-B0
      │
      ▼
Three-Stage Fine Tuning
      │
      ▼
Validation
      │
      ▼
Best Model Checkpoint
      │
      ▼
Testing
      │
      ▼
Performance Metrics
```

---

## Data Augmentation

Basic Augmentation

- Resize
- Normalization
- Horizontal Flip

Full Augmentation (100% Training Data)

- Random Rotation
- Random Horizontal Flip
- Random Vertical Flip
- Color Jitter
- Random Affine
- Normalization

---

## Training Configuration

| Parameter | Value |
|------------|-------|
| Architecture | EfficientNet-B0 |
| Input Size | 224 × 224 |
| Optimizer | AdamW |
| Loss | Focal Loss |
| Scheduler | CosineAnnealingWarmRestarts |
| Epochs | 50 |
| Seeds | 3 |
| Classes | 4 |

---

## Resume Training Support

The repository includes a complete checkpoint recovery system.

Features include

- Automatic checkpoint saving after every epoch
- Resume interrupted experiments
- Restore optimizer state
- Restore scheduler state
- Restore best validation weights
- Restore training history
- Restore patience counter
- Restore fine-tuning stage

This enables long-running experiments to continue from the last completed epoch without losing progress.

---

## Evaluation Metrics

The following metrics are reported.

### Accuracy

Overall classification accuracy.

### Macro F1 Score

Balanced performance across all tumor classes.

### Standard Deviation

Variation across multiple random seeds.

### Transfer Efficiency Index (TEI)

Measures the practical benefit of transfer learning compared with training from scratch while considering dataset size.

---

## Experimental Results

| Training Data | Transfer Learning Accuracy | Scratch Accuracy | Transfer Learning F1 | TEI |
|---------------|---------------------------|------------------|----------------------|-----|
| 5% | 78.27% | 66.85% | 77.45% | 0.048 |
| 10% | 81.35% | 78.77% | 80.82% | 0.0096 |
| 20% | 85.56% | 82.81% | 85.25% | 0.0092 |
| 30% | 90.10% | 89.71% | 89.82% | 0.0013 |
| 50% | 92.56% | 91.06% | 92.37% | 0.0044 |
| 100% | 94.46% | 93.69% | 94.36% | 0.0021 |

---

## Generated Results

The repository automatically produces

- Overall experimental summary
- Training history
- Transfer Efficiency Index table
- Publication-quality figures
- Confusion matrices
- Per-class F1 scores
- Saved trained models
- CSV reports
- JSON summaries

---

## Project Structure

```text
.
├── data/
│   ├── Training/
│   └── Testing/
│
├── notebooks/
│   └── Brain_Tumor_Transfer_Learning.ipynb
│
├── results/
│   ├── figures/
│   ├── models/
│   ├── epoch_ckpts/
│   ├── all_results.csv
│   ├── tei_results.csv
│   ├── checkpoint.csv
│   └── results_summary.json
│
├── README.md
└── requirements.txt
```

---

## Installation

Clone the repository

```bash
git clone https://github.com/username/repository-name.git

cd repository-name
```

Create environment

```bash
python -m venv venv
```

Activate environment

Windows

```bash
venv\Scripts\activate
```

Linux

```bash
source venv/bin/activate
```

Install dependencies

```bash
pip install -r requirements.txt
```

---

## Running Experiments

Run the notebook

```bash
jupyter notebook
```

or

```bash
jupyter lab
```

Execute all notebook cells sequentially.

Experiments automatically save checkpoints after every epoch.

Interrupted training resumes automatically.

---

## Output Files

```
results/

├── figures/
│   ├── fig1_main_results.png
│   ├── fig2_confusion_matrices.png
│   ├── fig3_training_curves.png
│
├── models/
│
├── epoch_ckpts/
│
├── all_results.csv
├── tei_results.csv
├── checkpoint.csv
└── results_summary.json
```

---

## Reproducibility

The repository ensures reproducible experiments through

- Fixed random seeds
- Automatic checkpoint recovery
- Consistent preprocessing
- Multi-seed evaluation
- Saved optimizer state
- Saved scheduler state
- Saved model weights
- Complete experiment logs

---

## Citation

If you use this repository in your research, please cite the corresponding publication.

```bibtex
@article{Author2026,
  title={Cross-Domain Transfer Learning for Brain Tumor Classification},
  author={Author},
  journal={Under Review},
  year={2026}
}
```

---

## Author

Nakib Uddin Ahmed

M.Sc. in Information Technology  
Shahjalal University of Science and Technology (SUST)

Research Interests

- Artificial Intelligence
- Medical Image Analysis
- Deep Learning
- Computer Vision
- Transfer Learning
- Explainable AI

---

## License

This project is released under the MIT License.

---

## Acknowledgements

This work utilizes

- PyTorch
- Torchvision
- EfficientNet
- Scikit-learn
- NumPy
- Pandas
- Matplotlib
- OpenCV

The experiments were conducted using the Brain Tumor MRI Dataset developed by Masoud Nickparvar.
