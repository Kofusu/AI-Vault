---
type: concept
status: not-started
domain: machine-learning
topic: learning-paradigms
level: foundations
order: 2
created: 2026-07-28
---

# Supervised Learning

## Tujuan

- Memahami belajar dari pasangan input–target.
- Membedakan classification dan regression.
- Memahami empirical risk dan generalization.

## Intuisi

Teacher menyediakan jawaban:

```text
(image, label)
(feature, target value)
        ↓
Model belajar mapping
```

## Konsep Dasar

Dataset:

$$
\mathcal{D}
=
\{(x_i,y_i)\}_{i=1}^{n}
$$

Model belajar:

$$
f_\theta:x\mapsto\hat{y}
$$

Empirical risk:

$$
\hat{R}(\theta)
=
\frac{1}{n}
\sum_iL(f_\theta(x_i),y_i)
$$

Tujuan sebenarnya adalah performa pada data baru, bukan sekadar empirical risk rendah.

## Kenapa Dibutuhkan?

Supervised learning efektif jika label target tersedia dan prediction task jelas.

Contoh:

- image classification
- house price regression
- object detection
- segmentation
- OCR

## Cara Kerja

```text
Labeled train data
  ↓ fit
Model parameters
  ↓ validation
Hyperparameter selection
  ↓
Final test
```

## Classification

Target categorical:

```text
cat / dog
normal / defect
class 0 ... K-1
```

Output dapat berupa class atau probability distribution.

## Regression

Target continuous:

```text
depth
age
temperature
bounding-box coordinate
```

## Label Noise

Noise:

- annotator disagreement
- ambiguous class
- wrong label
- stale target

Model dapat belajar noise jika capacity tinggi.

## Class Imbalance

Jika positive class jarang, accuracy dapat tinggi walaupun model gagal menemukan positive. Gunakan sampling, weighting, threshold analysis, dan metric yang sesuai.

## Implementasi

```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression(max_iter=1000)
model.fit(X_train, y_train)
predictions = model.predict(X_validation)
probabilities = model.predict_proba(X_validation)
```

## Studi Kasus: Medical Imaging

Pertimbangkan:

- patient-level split
- label source
- disease prevalence
- false-negative cost
- calibration
- external validation

Random image split dapat membocorkan patient identity.

## Best Practice

- Tentukan label schema.
- Ukur inter-annotator agreement jika relevan.
- Split berdasarkan independent unit.
- Mulai dengan simple baseline.
- Audit imbalance.
- Laporkan uncertainty.

## Kesalahan Umum

- Train dan test duplicate.
- Target leakage.
- Accuracy untuk rare event.
- Memperlakukan noisy label sebagai ground truth absolut.
- Tuning pada test.

## Debugging

- Cek alignment jumlah row `X` dan `y`.
- Visualisasikan sample per class.
- Overfit subset kecil.
- Cek confusion matrix.
- Inspect high-confidence error.
- Bandingkan class prior baseline.

## Ringkasan

- Supervised learning memakai labeled pairs.
- Classification dan regression berbeda target.
- Training loss rendah belum menjamin generalization.
- Label quality dan split menentukan validity.

## Checklist Pemahaman

- [ ] Bisa menulis dataset $(x_i,y_i)$.
- [ ] Bisa membedakan classification dan regression.
- [ ] Bisa menjelaskan empirical risk.
- [ ] Bisa menjelaskan label noise.
- [ ] Bisa memilih split dan metric.

## Latihan

1. Ubah defect detection menjadi supervised task.
2. Identifikasi kemungkinan label noise.
3. Pilih metric untuk rare defect.
4. Rancang patient-level split.

