---
type: dataset
status: evaluating
domain:
task:
source:
source_url:
license:
version:
format:
size:
created: {{date}}
updated: {{date}}
tags:
  - dataset
---

# {{title}}

## 1. Ringkasan

Jelaskan dataset ini dalam 2–4 kalimat.

Contoh:

> CIFAR-10 adalah dataset image classification berisi 60.000 gambar RGB berukuran 32×32 yang terbagi menjadi 10 kelas.

---

## 2. Tujuan Dataset

Dataset ini dibuat untuk task apa?

- Image Classification
- Object Detection
- Semantic Segmentation
- Instance Segmentation
- Pose Estimation
- OCR
- Tracking
- Depth Estimation
- 3D Reconstruction
- Vision-Language
- Lainnya:

---

## 3. Sumber

- Nama penyedia:
- Website resmi:
- Paper:
- Repository:
- Download URL:
- Tanggal akses:

---

## 4. License dan Penggunaan

- License:
- Boleh untuk penggunaan komersial:
- Boleh dimodifikasi:
- Wajib atribusi:
- Ada batasan distribusi:
- Ada data sensitif:
- Catatan legal:

> [!warning]
> Jangan pakai dataset untuk project production sebelum license dan aturan penggunaannya benar-benar dicek.

---

## 5. Versi Dataset

- Version:
- Release date:
- Last updated:
- Perubahan dari versi sebelumnya:
- Checksum:

---

## 6. Statistik Dataset

| Informasi | Nilai |
|---|---:|
| Total samples | |
| Train samples | |
| Validation samples | |
| Test samples | |
| Jumlah kelas | |
| Jumlah anotasi | |
| Total size | |
| Resolusi rata-rata | |
| Durasi video total | |
| Jumlah sequence | |

Hapus baris yang nggak relevan.

---

## 7. Data Split

| Split | Jumlah | Persentase | Keterangan |
|---|---:|---:|---|
| Train | | | |
| Validation | | | |
| Test | | | |

### Split Strategy

- Random split
- Stratified split
- Group-based split
- Time-based split
- Subject-based split
- Scene-based split
- Official split

### Risiko Data Leakage

- Apakah objek/subjek yang sama muncul di split berbeda?
- Apakah frame video berdekatan masuk ke split berbeda?
- Apakah preprocessing dilakukan sebelum split?
- Apakah duplicate image tersebar ke split lain?

---

## 8. Struktur Folder

```text
dataset/
├── train/
│   ├── images/
│   └── labels/
├── val/
│   ├── images/
│   └── labels/
└── test/
    ├── images/
    └── labels/
```

Sesuaikan dengan struktur sebenarnya.

---

## 9. Format Data

### Input

- Image
- Video
- Text
- Point cloud
- Depth map
- LiDAR
- Multimodal

### File Format

- Images:
- Labels:
- Metadata:
- Video:
- Point cloud:

### Contoh Sample

```json
{
  "image": "images/0001.jpg",
  "label": 3
}
```

---

## 10. Format Annotation

Pilih yang relevan:

- Classification label
- Bounding box
- Polygon
- Segmentation mask
- Keypoints
- Caption
- Tracking ID
- Depth map
- Point cloud labels
- Camera pose

### Schema Annotation

```json
{
  "image_id": 1,
  "category_id": 3,
  "bbox": [120, 80, 200, 150]
}
```

### Coordinate Convention

- Format bounding box:
  - `x_min, y_min, x_max, y_max`
  - `x, y, width, height`
  - normalized YOLO format
- Origin koordinat:
- Satuan:
- Resolusi referensi:

---

## 11. Daftar Kelas

| ID | Class | Jumlah Sample | Persentase |
|---:|---|---:|---:|
| 0 | | | |
| 1 | | | |
| 2 | | | |

---

## 12. Distribusi Data

Catat distribusi penting:

- Distribusi kelas
- Distribusi resolusi
- Distribusi aspect ratio
- Distribusi pencahayaan
- Distribusi lokasi
- Distribusi subjek
- Distribusi device/camera
- Distribusi waktu

Tambahkan grafik:

```md
![[class-distribution.png]]

![[image-resolution-distribution.png]]
```

---

## 13. Contoh Data

### Contoh Normal

```md
![[sample-normal-01.png]]
```

### Contoh Sulit

```md
![[sample-hard-01.png]]
```

### Contoh Anomali

```md
![[sample-anomaly-01.png]]
```

---

## 14. Data Quality

### Masalah yang Ditemukan

- [ ] Label salah
- [ ] Missing label
- [ ] Duplicate data
- [ ] Corrupted file
- [ ] Class imbalance
- [ ] Low-resolution image
- [ ] Annotation tidak konsisten
- [ ] Data leakage
- [ ] Background bias
- [ ] Domain mismatch
- [ ] Metadata hilang

### Detail

| Masalah | Jumlah | Dampak | Tindakan |
|---|---:|---|---|
| | | | |

---

## 15. Bias dan Limitasi

Bahas:

- Geographic bias
- Demographic bias
- Camera/device bias
- Lighting bias
- Background bias
- Class imbalance
- Representation gap
- Temporal bias
- Annotation bias
- Synthetic-to-real gap

### Dampak ke Model

Jelaskan model bisa gagal pada kondisi apa.

---

## 16. Privacy dan Etika

- Apakah ada wajah manusia?
- Apakah ada nomor kendaraan?
- Apakah ada informasi pribadi?
- Apakah ada anak-anak?
- Apakah ada data medis?
- Apakah consent tersedia?
- Apakah data perlu dianonimkan?

### Tindakan

- Blur wajah
- Hapus EXIF
- Mask nomor kendaraan
- Encrypt data
- Batasi akses
- Dokumentasikan consent

---

## 17. Preprocessing

### Preprocessing Wajib

- Resize:
- Crop:
- Normalize:
- Color conversion:
- Denoising:
- Padding:
- Tokenization:
- Point-cloud filtering:

### Normalization

```python
mean = []
std = []
```

### Catatan

Pastikan preprocessing untuk validation dan test tidak memakai informasi dari data training secara tidak fair.

---

## 18. Data Augmentation

- Horizontal flip
- Rotation
- Random crop
- Color jitter
- Blur
- Noise
- Cutout
- MixUp
- CutMix
- Mosaic
- Copy-Paste
- Synthetic augmentation

### Augmentation Policy

```yaml
augmentation:
  horizontal_flip: 0.5
  rotation: 10
  color_jitter: true
```

---

## 19. Cara Download

```bash
# command download
```

Atau:

```python
# script download dataset
```

---

## 20. Cara Load Dataset

```python
from pathlib import Path

dataset_root = Path("data/dataset-name")

for image_path in dataset_root.rglob("*.jpg"):
    print(image_path)
```

---

## 21. Dataset Loader

### PyTorch

```python
from pathlib import Path
from PIL import Image
from torch.utils.data import Dataset


class CustomDataset(Dataset):
    def __init__(self, image_paths: list[Path], transform=None):
        self.image_paths = image_paths
        self.transform = transform

    def __len__(self) -> int:
        return len(self.image_paths)

    def __getitem__(self, index: int):
        image_path = self.image_paths[index]
        image = Image.open(image_path).convert("RGB")

        if self.transform is not None:
            image = self.transform(image)

        return image
```

### Validation Loader

- [ ] File exists
- [ ] Label valid
- [ ] Shape benar
- [ ] Dtype benar
- [ ] Range pixel benar
- [ ] Class ID valid
- [ ] Annotation tidak kosong

---

## 22. Baseline Model

| Model | Metric | Score | Sumber |
|---|---|---:|---|
| | | | |

---

## 23. Evaluation Metrics

Metric utama:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- mAP
- AP50
- AP75
- mIoU
- Dice
- PCK
- MOTA
- IDF1
- RMSE
- LPIPS
- FID

### Evaluation Protocol

Jelaskan:

- Split yang dipakai
- Metric implementation
- Threshold
- IoU threshold
- Image resolution
- Test-time augmentation
- Apakah pretrained weights digunakan

---

## 24. Cocok Digunakan Untuk

- Task:
- Skala eksperimen:
- Baseline:
- Reproduksi paper:
- Fine-tuning:
- Deployment prototype:

---

## 25. Tidak Cocok Digunakan Untuk

Contoh:

- Production pada domain berbeda
- Evaluasi generalization lintas negara
- Model real-time karena resolusi terlalu tinggi
- Face recognition karena tidak ada consent
- Small-object detection karena objek terlalu besar

---

## 26. Rencana Penggunaan

Project terkait:

- [[PRJ-001 Nama Project]]

Experiment terkait:

- [[EXP-001 Baseline]]

Paper terkait:

- [[Nama Paper]]

---

## 27. Keputusan

- [ ] Gunakan dataset
- [ ] Gunakan dengan cleaning
- [ ] Gunakan sebagian
- [ ] Gabungkan dengan dataset lain
- [ ] Tolak dataset
- [ ] Perlu evaluasi lanjutan

### Alasan

---

## 28. Checklist Sebelum Training

- [ ] License sudah dicek
- [ ] Dataset version dicatat
- [ ] Checksum dicatat
- [ ] Split aman dari leakage
- [ ] Label sudah divalidasi
- [ ] Duplicate sudah dicek
- [ ] Corrupted file sudah dicek
- [ ] Distribusi kelas sudah dianalisis
- [ ] Bias sudah dicatat
- [ ] Preprocessing sudah ditentukan
- [ ] Evaluation protocol sudah jelas
- [ ] Dataset loader sudah dites
- [ ] Sample visualization sudah dicek

---

## 29. Open Questions

- 
- 
- 

---

## 30. Referensi

- Dokumentasi:
- Paper:
- Repository:
- Dataset card:
- Benchmark:
