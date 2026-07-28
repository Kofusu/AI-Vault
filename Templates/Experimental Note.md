---
type: experiment
status: planned
project:
experiment_id:
date: {{date}}
model:
dataset:
task:
seed:
tags:
  - experiment
---

# {{title}}

## 1. Tujuan Eksperimen

Apa yang mau diuji?

Contoh:

> Menguji apakah AdamW menghasilkan validation accuracy lebih tinggi dibanding SGD pada model ResNet-18.

---

## 2. Research Question

Pertanyaan utama eksperimen:

- Apakah perubahan ini meningkatkan performa?
- Apakah peningkatannya konsisten?
- Apa trade-off-nya?

---

## 3. Hipotesis

Tuliskan dugaan sebelum eksperimen dijalankan.

Contoh:

> AdamW diperkirakan menghasilkan convergence yang lebih stabil karena weight decay diterapkan secara terpisah dari gradient update.

---

## 4. Baseline

Eksperimen pembanding yang dijadikan titik awal.

- Experiment ID:
- Model:
- Dataset:
- Config:
- Metric utama:
- Hasil baseline:

Link:

- [[EXP-000 Baseline]]

---

## 5. Variabel Eksperimen

### Independent Variable

Hal yang sengaja diubah:

- Optimizer
- Learning rate
- Augmentation
- Model architecture
- Loss function

### Controlled Variables

Hal yang harus tetap sama:

- Dataset split
- Random seed
- Batch size
- Number of epochs
- Preprocessing
- Evaluation metric

### Dependent Variable

Hasil yang diamati:

- Accuracy
- F1-score
- mAP
- mIoU
- Training time
- Inference latency
- Memory usage

---

## 6. Perubahan yang Diuji

Jelaskan satu perubahan utama.

> [!warning]
> Sebisa mungkin ubah hanya satu faktor dalam satu eksperimen supaya penyebab perubahan hasil bisa dianalisis dengan jelas.

---

## 7. Konfigurasi

```yaml
experiment:
  id:
  name:
  seed: 42

dataset:
  name:
  version:
  train_split:
  validation_split:
  test_split:

model:
  name:
  pretrained:
  num_classes:

training:
  epochs:
  batch_size:
  learning_rate:
  optimizer:
  weight_decay:
  scheduler:
  loss_function:

augmentation:
  resize:
  horizontal_flip:
  random_crop:
  color_jitter:

evaluation:
  primary_metric:
  secondary_metrics:
```

---

## 8. Environment

| Komponen             | Detail |
| -------------------- | ------ |
| OS                   |        |
| Python               |        |
| PyTorch / TensorFlow |        |
| CUDA                 |        |
| cuDNN                |        |
| GPU                  |        |
| RAM                  |        |
| Git commit           |        |
| Docker image         |        |

---

## 9. Cara Menjalankan

```bash
python train.py \
  --config configs/experiment.yaml \
  --seed 42
```

---

## 10. Tracking

- Repository:
- Branch:
- Commit:
- W&B run:
- MLflow run:
- TensorBoard:
- Checkpoint:
- Config file:

---

## 11. Hasil Training

### Metric Utama

| Metric | Baseline | Experiment | Selisih |
|---|---:|---:|---:|
| Accuracy | | | |
| Precision | | | |
| Recall | | | |
| F1-score | | | |
| Loss | | | |

### Efisiensi

| Metric | Baseline | Experiment | Selisih |
|---|---:|---:|---:|
| Training time | | | |
| Inference latency | | | |
| GPU memory | | | |
| Model size | | | |

---

## 12. Training Curve

Tambahkan:

- Training loss
- Validation loss
- Training metric
- Validation metric

```md
![[training-loss.png]]

![[validation-metric.png]]
```

---

## 13. Qualitative Results

Tambahkan contoh prediction:

```md
![[prediction-example-01.png]]
```

Catat:

- Prediction yang benar
- False positive
- False negative
- Kasus sulit
- Pola kegagalan model

---

## 14. Observasi

Apa yang terlihat selama eksperimen?

Contoh:

- Validation loss mulai naik setelah epoch 20.
- Training lebih stabil dibanding baseline.
- Model gagal pada objek berukuran kecil.
- Augmentation terlalu agresif merusak detail gambar.

---

## 15. Analisis

Jelaskan kenapa hasil tersebut terjadi.

Bahas:

- Apakah hipotesis terbukti?
- Apakah hasilnya signifikan?
- Apakah ada indikasi overfitting?
- Apakah perbandingannya fair?
- Apakah hasil mungkin dipengaruhi seed?
- Apa trade-off performa dan efisiensi?

---

## 16. Error dan Anomali

Catat masalah selama eksperimen:

- NaN loss
- Out of memory
- Data leakage
- Class imbalance
- Metric tidak konsisten
- Training interrupted

Link ke debugging note:

- [[BUG-001 Nama Error]]

---

## 17. Kesimpulan

Ringkas hasil eksperimen dalam 2–5 kalimat.

---

## 18. Keputusan

- [ ] Gunakan perubahan ini
- [ ] Tolak perubahan ini
- [ ] Jalankan ulang dengan seed berbeda
- [ ] Butuh eksperimen lanjutan
- [ ] Jadikan baseline baru

Alasan keputusan:

---

## 19. Next Experiment

Eksperimen berikutnya:

- [[EXP-002 Nama Eksperimen]]

---

## 20. Reproducibility Checklist

- [ ] Random seed dicatat
- [ ] Dataset version dicatat
- [ ] Dataset split disimpan
- [ ] Config disimpan
- [ ] Dependency version dicatat
- [ ] Git commit dicatat
- [ ] Checkpoint disimpan
- [ ] Metric dan log disimpan
- [ ] Hardware dicatat
- [ ] Eksperimen bisa dijalankan ulang

Masalah sebelumnya terjadi karena ada **code fence di dalam code fence**. Solusinya, wrapper luar memakai empat backtick, sementara blok YAML dan Bash di dalam tetap tiga backtick.