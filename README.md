# RefEnhNet
> ⚠️ **NOTE**: The complete code files will be uploaded after the paper is accepted.

**Self-Prior Guided Underwater Image Enhancement**

---

## 📑 Table of Contents
- [Overview](#overview)
- [Paper Information](#paper-information)
- [Method](#method)
- [Results](#results)
- [Environment Setup](#environment-setup)
- [Code Structure](#code-structure)
- [Getting Started](#getting-started)
- [Citation](#citation)
- [Contact](#contact)

---

## 🔍 Overview
Underwater images often suffer from severe spatial heterogeneous degradation, leading to the **"70/30 effect"** (most regions acceptable, high-degradation regions insufficiently enhanced).

Inspired by **ancient mural restoration** (using intact regions to repair damaged parts), we propose **RefEnhNet**, a self-prior guided enhancement method that:
1. Uses low-degradation regions within the same semantic category as natural priors
2. Quantifies degradation via depth, color, and illumination clustering
3. Explicitly aligns texture, color, and illumination for consistent enhancement

---

## 📄 Paper Information
### Title
**Self-Prior Guided Underwater Image Enhancement Inspired by Ancient Mural Restoration**

### Authors
Huachuan Ma, Lin Tian, Yifeng Zhou, Dejin Zhang  
*Shenzhen University*

### Abstract
The quality degradation of underwater images severely restricts the accuracy of downstream visual tasks. Mainstream enhancement methods typically employ a globally uniform processing strategy that fails to effectively address the spatial heterogeneity of image degradation, resulting in the **"70/30 effect"**.To solve this problem, inspired by ancient mural restoration, we propose a self-prior guided underwater image enhancement method. We first perform preliminary enhancement and semantic segmentation, then construct a multi-dimensional feature vector to quantify degradation via clustering. Finally, **RefEnhNet** extracts priors from low-degradation regions to guide high-degradation region enhancement, improving visual consistency and naturalness.Comprehensive experiments show our method outperforms state-of-the-art methods in both subjective visual quality and objective metrics.

---

## 🧠 Method
### Overall Pipeline
![Pipeline](illustration/1.jpg)
Three-stage framework:
1. **Preliminary Enhancement & Semantic Segmentation** (WaterNet + CoralScapes)
2. **Degradation Quantification** (depth/color/illumination + K-means clustering)
3. **Self-Prior Guided Enhancement** (RefEnhNet with feature alignment)

### Degradation Quantification
![Degradation](illustration/3.jpg)
- Multi-dimensional feature: **depth (0.6) + color (0.3) + illumination (0.1)**
- Unsupervised K-means clustering to split low/high degradation regions

### RefEnhNet Architecture
![Network](illustration/2.jpg)
- **Texture Consistency Module (TCM)**
- **Color Consistency Module (CCM)**
- **Illumination Consistency Module (ICM)**
- Residual learning + multi-objective loss

---

## 📊 Results
### Visual Comparison
![Visual Results 1](illustration/4.jpg)
![Visual Results 2](illustration/6.jpg)
![Visual Results 3](illustration/7.jpg)

### Quantitative Performance
| Method | TS | CDS | IC | NSC | Params(M) | FLOPs(G) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| RefEnhNet | **0.72** | **0.67** | **0.75** | **32.5** | 1.5 | 13.6 |

- Outperforms SOTA methods on **UIEB, UFO120, LSUI** datasets
- Lightweight (1.5M params) and efficient (13.6G FLOPs)
- Boosts downstream tasks: **+30~40% feature points**, **+15~20% matching rate**

### Ablation Study
![Ablation](illustration/5.jpg)
All modules (TCM/CCM/ICM/FL) are critical for consistent, natural enhancement.

---

## 🛠 Environment Setup
### Requirements
python>=3.8
tensorflow>=2.8.0
torch>=1.10.0
torchvision>=0.11.0
opencv-python>=4.5.5
numpy>=1.21.0
scipy>=1.7.0
matplotlib>=3.4.0
scikit-learn>=1.0.0
pillow>=8.3.0
tqdm>=4.62.0

### Install
```bash
pip install -r requirements.txt
RefEnhNet/
├── README.md               # Project description
├── requirements.txt        # Dependencies
├── LICENSE                 # License
├── src/                    # Core code
│   ├── __init__.py
│   ├── config.py           # Configs & paths
│   ├── data/               # Data loading & processing
│   │   ├── __init__.py
│   │   ├── dataset.py      # Dataset class
│   │   └── transforms.py   # Preprocessing
│   ├── model/              # Network modules
│   │   ├── __init__.py
│   │   ├── refenhnet.py    # Main RefEnhNet
│   │   ├── modules.py      # TCM/CCM/ICM blocks
│   │   └── loss.py         # Multi-objective loss
│   ├── utils/              # Tools
│   │   ├── __init__.py
│   │   ├── metrics.py      # TS/CDS/IC/NSC
│   │   ├── depth_estimate.py # Depth Anything v2
│   │   └── segmentation.py # CoralScapes wrapper
│   ├── train.py            # Training script
│   └── test.py             # Inference & evaluation
├── datasets/               # Datasets
│   ├── UIEB/
│   ├── UFO120/
│   ├── LSUI/
│   └── SweetCorals/
├── checkpoints/            # Pre-trained & trained models
├── results/                # Output enhanced images
│   ├── qualitative/
│   └── quantitative/
└── docs/                   # Paper figures & docs
    ├── fig3.png
    ├── fig4.png
    ├── fig5.png
    ├── fig7.png
    ├── fig8.png
    └── fig10.png
