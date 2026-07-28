---
type: concept
status: not-started
domain: machine-learning
topic: generalization
level: foundations
order: 9
created: 2026-07-28
---

# Overfitting and Underfitting

## Tujuan

- Membedakan fit training dan generalization.
- Memahami bias–variance trade-off.
- Mendiagnosis melalui learning curve.

## Intuisi

```text
Underfit → terlalu sederhana atau belum belajar
Good fit → pola relevan
Overfit  → hafal noise/training detail
```

## Konsep Dasar

- Training error: error data fit.
- Generalization error: expected error data baru.
- Generalization gap: perbedaan performa train dan validation.

## Kenapa Dibutuhkan?

Tujuan ML adalah performa data baru. Training score sendiri tidak cukup.

## Cara Kerja

### Underfitting

Gejala:

- train buruk
- validation buruk

Penyebab:

- model terlalu sederhana
- feature lemah
- training belum cukup
- optimization gagal
- regularization terlalu kuat

### Overfitting

Gejala:

- train sangat baik
- validation lebih buruk

Penyebab:

- capacity tinggi
- data sedikit/noisy
- leakage model selection
- training terlalu lama
- weak regularization

## Bias dan Variance

- High bias: assumption terlalu sederhana.
- High variance: terlalu sensitif terhadap training sample.

Trade-off bukan hukum bahwa improvement satu selalu merusak yang lain, tetapi mental model diagnosis.

## Learning Curve

Plot metric terhadap:

- training iteration
- training sample size
- model complexity

Interpretasi harus melihat train dan validation bersama.

## Regularization

- L1/L2
- early stopping
- data augmentation
- dropout
- simpler model
- feature selection
- more representative data

Regularization mengubah objective atau effective capacity.

## Implementasi

```python
from sklearn.model_selection import learning_curve

sizes, train_scores, val_scores = learning_curve(
    estimator,
    X,
    y,
    cv=5,
    scoring="f1_macro",
)
```

## Studi Kasus: Small Image Dataset

CNN mencapai 100% train accuracy dan 65% validation. Sebelum menambah augmentation:

- cek duplicate split
- cek label
- cek validation distribution
- cek model selection leakage
- bandingkan pretrained/simple baseline

Overfitting bukan satu-satunya explanation.

## Best Practice

- Pantau train dan validation.
- Gunakan baseline.
- Pisahkan optimization failure dari capacity.
- Tambah data yang representatif.
- Regularize berdasarkan diagnosis.
- Lock test set.

## Kesalahan Umum

- Menyebut semua validation failure overfitting.
- Hanya melihat loss terakhir.
- Menambah complexity pada underfit akibat bug.
- Tuning berulang ke test.
- Data augmentation tidak preserving label.

## Debugging

Sanity ladder:

1. overfit satu batch
2. overfit subset kecil
3. train full data
4. compare train/validation
5. inspect learning curve
6. inspect failure cases

## Ringkasan

- Underfit buruk pada train dan validation.
- Overfit mempunyai generalization gap besar.
- Diagnosis membutuhkan curve, data audit, dan baseline.

## Checklist Pemahaman

- [ ] Bisa membedakan train dan generalization error.
- [ ] Bisa mengenali underfit/overfit.
- [ ] Bisa menjelaskan bias–variance.
- [ ] Bisa membaca learning curve.
- [ ] Bisa memilih intervention.

## Latihan

1. Buat empat skenario curve.
2. Diagnosis masing-masing.
3. Pilih regularization.
4. Jelaskan kapan lebih banyak data membantu.

