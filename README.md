#  Scale × ODYSSEY — Astronomical Object Classification

##  Project Overview

A deep learning pipeline that classifies astronomical images into five major celestial object categories

| Class | Label |
|-------|-------|
| Spiral Galaxy | `spiral` |
| Elliptical Galaxy | `elliptical` |
| Nebula | `nebula` |
| Star Cluster | `star_cluster` |
| Planetary Object | `planetary` |

The model learns directly from raw image data without manually engineered astrophysical features, using transfer learning on EfficientNet-B3, with Grad-CAM visualizations for interpretability.

## 📊 Dataset Sources

| Dataset | Source | Description |
|---------|--------|-------------|
| Galaxy Zoo | [Zooniverse](https://www.zooniverse.org/projects/zookeeper/galaxy-zoo/) | Labeled galaxy morphology — spiral & elliptical |
| DeepSky Dataset | [Kaggle](https://www.kaggle.com/datasets/) | Nebulae, galaxies, and star cluster imagery |
| Hubble Image Archive | [NASA/ESA](https://hubblesite.org/images/gallery) | Public astronomical imagery from Hubble |
| Astronomy Classification Dataset | [Kaggle](https://www.kaggle.com/datasets/muratkokludataset/star-dataset) | Preprocessed astronomical image classification |

**Total images used:** ~12,400 (after cleaning and balancing)  
**Train / Val / Test split:** 70% / 15% / 15%

---

##  Model Architecture

```
Input Image (224×224×3)
        ↓
   Preprocessing
   (Normalize, Augment)
        ↓
EfficientNet-B3 Backbone
   (pretrained on ImageNet)
        ↓
  Global Average Pooling
        ↓
   Dropout (p=0.4)
        ↓
  Dense (256, ReLU)
        ↓
   Dropout (p=0.3)
        ↓
  Dense (5, Softmax)
        ↓
  Predicted Class Label
```

**Why EfficientNet-B3?**
- Strong ImageNet pretraining generalizes well to astronomical textures
- Compound scaling balances depth/width/resolution efficiently
- <5s inference on CPU for a single image 

---

##  Final Test Metrics
Training in  still in progress

##  Setup Instructions

### Prerequisites

```bash
Python >= 3.9
pip
```

### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/scale-odyssey.git
cd scale-odyssey
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Download Datasets

See [`docs/dataset_setup.md`](docs/dataset_setup.md) for step-by-step instructions to download and place datasets in `data/`.

Expected structure:
```
data/
├── train/
│   ├── spiral/
│   ├── elliptical/
│   ├── nebula/
│   ├── star_cluster/
│   └── planetary/
├── val/
└── test/
```

### 4. Train the Model

```bash
python src/train.py --epochs 30 --batch_size 32 --lr 1e-4
```

### 5. Evaluate

```bash
python src/evaluate.py --checkpoint models/best_model.pth
```

### 6. Run Inference on a Single Image

```bash
python src/inference.py --image path/to/image.jpg
```

### 7. Generate Grad-CAM Visualization

```bash
python src/gradcam.py --image path/to/image.jpg --checkpoint models/best_model.pth
```

---

##  Training Script

See [`src/train.py`](src/train.py) — key hyperparameters:

```python
BACKBONE     = "efficientnet_b3"
IMG_SIZE     = 224
BATCH_SIZE   = 32
EPOCHS       = 30
LR           = 1e-4
WEIGHT_DECAY = 1e-5
DROPOUT      = 0.4
NUM_CLASSES  = 5
```

Training uses:
- **Loss:** CrossEntropyLoss with label smoothing (0.1)
- **Optimizer:** AdamW
- **Scheduler:** CosineAnnealingLR
- **Augmentations:** Random horizontal/vertical flip, rotation (±15°), color jitter, random crop

---

##  Repository Structure

```
scale-odyssey/
├── README.md                  ← This file (one-pager report)
├── requirements.txt
├── src/
│   ├── dataset.py             ← Dataset class & augmentations
│   ├── model.py               ← EfficientNet-B3 model definition
│   ├── train.py               ← Training loop
│   ├── evaluate.py            ← Metrics + confusion matrix
│   ├── inference.py           ← Single-image inference
│   └── gradcam.py             ← Grad-CAM visualization
├── notebooks/
│   └── exploration.ipynb      ← EDA + sample visualizations
├── data/
│   └── sample_images/         ← A few sample images for quick testing
├── models/
│   └── .gitkeep               ← Trained weights go here (not tracked)
├── results/
│   ├── confusion_matrix.png
│   ├── training_curves.png
│   └── gradcam_samples/
└── docs/
    └── dataset_setup.md
```

---

##  Explainability: Grad-CAM

The model produces Grad-CAM heatmaps to show which image regions drive predictions. See [`src/gradcam.py`](src/gradcam.py) and sample outputs in [`results/gradcam_samples/`](results/gradcam_samples/).

---



## Team

| Name | Roll No. |
|------|----------|
| A. Ram Vara Prasad | 25110055 |

---

*IIT Gandhinagar — Technical Council Summer Projects 2026-27*
