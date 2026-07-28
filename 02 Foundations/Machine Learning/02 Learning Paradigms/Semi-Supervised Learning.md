---
type: concept
status: not-started
domain: machine-learning
topic: learning-paradigms
level: foundations
order: 4
created: 2026-07-28
---

# Semi-Supervised Learning

## Tujuan

- Memahami kombinasi sedikit labeled dan banyak unlabeled data.
- Mengenal pseudo-labeling serta consistency regularization.
- Mengenali confirmation bias dan evaluation requirement.

## Intuisi

```text
Sedikit labeled data
      +
Banyak unlabeled data
      ↓
Model lebih baik jika asumsi terpenuhi
```

## Konsep Dasar

Dataset:

$$
\mathcal{D}_L=\{(x_i,y_i)\}
$$

$$
\mathcal{D}_U=\{x_j\}
$$

Objective umum:

$$
L=L_{\text{supervised}}
+\lambda L_{\text{unsupervised}}
$$

## Kenapa Dibutuhkan?

Label CV mahal karena membutuhkan:

- expert
- bounding box
- mask pixel
- quality control

Unlabeled image/video sering jauh lebih banyak.

## Cara Kerja

### Pseudo-Labeling

1. Train pada labeled data.
2. Predict unlabeled data.
3. Pilih prediction confidence tinggi.
4. Perlakukan sebagai pseudo-label.
5. Retrain.

### Consistency Regularization

Prediction seharusnya konsisten pada augmentation yang tidak mengubah semantic class.

$$
f(x)\approx f(\operatorname{augment}(x))
$$

## Assumption

- smoothness assumption
- cluster assumption
- low-density separation

Jika unlabeled distribution tidak relevan, manfaat dapat hilang atau merusak.

## Implementasi Konseptual

```python
probabilities = model.predict_proba(X_unlabeled)
confidence = probabilities.max(axis=1)
pseudo_labels = probabilities.argmax(axis=1)

mask = confidence >= 0.95
X_selected = X_unlabeled[mask]
y_selected = pseudo_labels[mask]
```

Threshold perlu divalidasi, bukan dipilih arbitrer.

## Studi Kasus: Defect Detection

Labeled defect sedikit, tetapi unlabeled production image banyak. Risiko:

- model awal bias ke normal class
- rare defect diberi pseudo-label normal
- camera baru berbeda domain
- confidence tidak calibrated

Human review pada uncertain/important sample dapat dikombinasikan dengan active learning.

## Best Practice

- Pertahankan labeled validation/test bersih.
- Bandingkan supervised-only baseline.
- Audit pseudo-label per class.
- Pantau distribution mismatch.
- Gunakan teacher confidence secara kritis.
- Laporkan jumlah unlabeled dan selection rate.

## Kesalahan Umum

- Evaluasi pada pseudo-label.
- Menganggap confidence sebagai kebenaran.
- Unlabeled data berasal dari domain berbeda.
- Confirmation bias.
- Test data masuk unlabeled pool.

## Debugging

- Plot confidence distribution.
- Inspect pseudo-label manual.
- Bandingkan class distribution.
- Turunkan/naikkan threshold.
- Jalankan ablation tanpa unlabeled data.
- Cek calibration teacher.

## Ringkasan

- Semi-supervised memakai labeled dan unlabeled data.
- Unlabeled data membantu hanya dengan asumsi dan objective yang tepat.
- Pseudo-label dapat memperkuat kesalahan model.

## Checklist Pemahaman

- [ ] Bisa membedakan labeled dan unlabeled objective.
- [ ] Bisa menjelaskan pseudo-labeling.
- [ ] Bisa menjelaskan consistency.
- [ ] Bisa mengenali confirmation bias.
- [ ] Bisa merancang evaluasi fair.

## Latihan

1. Desain pseudo-label experiment.
2. Tentukan supervised baseline.
3. Audit selection per class.
4. Tulis risiko domain mismatch.

