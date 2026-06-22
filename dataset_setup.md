# Dataset Setup Guide

This guide explains how to download and structure the datasets used in Scale × ODYSSEY.

---

## Step 1 — Galaxy Zoo (Spiral & Elliptical)

1. Go to [Zooniverse Galaxy Zoo](https://www.zooniverse.org/projects/zookeeper/galaxy-zoo/)
2. Download the labeled morphology CSV and images
3. Filter for spiral (`smooth-or-featured_featured-or-disk_fraction > 0.6`) and elliptical (`smooth-or-featured_smooth_fraction > 0.6`)
4. Place images in `data/train/spiral/` and `data/train/elliptical/`

Alternatively, the **GalaxyZoo1 dataset** is available on Kaggle:
```
https://www.kaggle.com/datasets/jaimetrickz/galaxy-zoo-1
```

---

## Step 2 — DeepSky Dataset (Nebulae, Star Clusters)

1. Search Kaggle for "DeepSky astronomical images"
2. Download and extract
3. Place nebula images in `data/train/nebula/`
4. Place star cluster images in `data/train/star_cluster/`

---

## Step 3 — Hubble Image Archive (Planetary Objects)

1. Visit [HubbleSite Image Gallery](https://hubblesite.org/images/gallery)
2. Filter by "Planets" and "Solar System"
3. Download at least 300-400 images
4. Place in `data/train/planetary/`

---

## Step 4 — Create Val and Test Splits

After placing all images in `data/train/<class>/`, run:

```bash
python src/split_dataset.py --input_dir data/train --val_ratio 0.15 --test_ratio 0.15
```

This will automatically create `data/val/` and `data/test/` with the correct structure.

---

## Final Expected Structure

```
data/
├── train/
│   ├── spiral/          (~2,100 images)
│   ├── elliptical/      (~2,100 images)
│   ├── nebula/          (~2,100 images)
│   ├── star_cluster/    (~2,100 images)
│   └── planetary/       (~2,100 images)
├── val/
│   ├── spiral/          (~450 images)
│   ├── elliptical/
│   ├── nebula/
│   ├── star_cluster/
│   └── planetary/
└── test/
    ├── spiral/          (~450 images)
    ├── elliptical/
    ├── nebula/
    ├── star_cluster/
    └── planetary/
```

---

## Notes

- All images are resized to **224×224** during training via transforms
- FITS files (`.fits`) are supported but will be loaded in grayscale then converted to RGB
- Minimum recommended images per class: **500** for meaningful training
