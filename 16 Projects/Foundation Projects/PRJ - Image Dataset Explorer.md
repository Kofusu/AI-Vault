---
type: project
status: idea
domain:
  - scientific-computing
  - computer-vision
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# PRJ - Image Dataset Explorer

## Problem Statement

Training sering dimulai sebelum memahami resolusi, aspect ratio, distribusi class, duplicate, corrupt file, dan bias dataset.

## Tujuan

Membuat exploratory analysis pipeline untuk dataset gambar menggunakan NumPy, Pandas, Matplotlib, Pillow, dan OpenCV.

## Input

- root directory
- optional label CSV
- optional class-from-folder convention

## Output

- manifest per image
- ringkasan statistik
- class distribution
- resolution/aspect-ratio plots
- corrupt/duplicate candidate report
- contact sheet sample normal dan anomali

## Kolom Manifest

```text
path
label
width
height
channels
mode
extension
file_size
aspect_ratio
mean_intensity
is_corrupt
hash
```

## Pipeline

```text
discover → validate → extract metadata → aggregate
   → visualize → inspect anomalies → report
```

## Struktur Project

```text
image-dataset-explorer/
├── configs/
├── src/image_explorer/
│   ├── discovery.py
│   ├── metadata.py
│   ├── quality.py
│   ├── visualization.py
│   └── report.py
├── tests/
└── reports/
```

## Milestone

- [ ] M1 — file discovery
- [ ] M2 — metadata DataFrame
- [ ] M3 — quality checks
- [ ] M4 — plots dan contact sheet
- [ ] M5 — reproducible report

## Acceptance Criteria

- Corrupt image tidak menghentikan pipeline.
- Statistik dapat dihitung per class dan keseluruhan.
- Plot memiliki label yang benar.
- Sample visualization memakai seed.
- Duplicate disebut candidate sampai diverifikasi.
- Tidak ada train/test information leakage jika split tersedia.

## Analisis Wajib

- imbalance
- missing label
- resolution/aspect ratio
- grayscale vs RGB
- class-specific background/size shortcut
- duplicate lintas split
- outlier file size

## Deliverable

- manifest
- folder figure
- Markdown report
- daftar recommended actions sebelum training

## Materi Terkait

- [[NumPy Fundamentals]]
- [[Pandas Fundamentals]]
- [[Matplotlib Fundamentals]]
- [[Pillow Fundamentals]]
- [[OpenCV Fundamentals]]
- [[Dataset and Data Quality]]

## Next Action

- [ ] Pilih dataset kecil dan definisikan schema manifest.

