---
type: concept
status: not-started
domain: machine-learning
topic: evaluation
level: foundations
order: 11
created: 2026-07-28
---

# Machine Learning Evaluation Metrics

## Tujuan

- Memilih metric berdasarkan task dan cost error.
- Menghitung classification dan regression metric dasar.
- Memahami threshold, averaging, calibration, dan uncertainty.

## Intuisi

Metric menjawab pertanyaan tertentu. Tidak ada satu metric terbaik untuk semua task.

## Konsep Dasar

Confusion matrix:

```text
                 Predicted
               +          -
Actual +       TP         FN
Actual -       FP         TN
```

## Kenapa Dibutuhkan?

Model selection mengikuti metric. Metric yang salah dapat mendorong behavior yang salah meskipun angkanya meningkat.

## Cara Kerja Classification Metrics

Accuracy:

$$
\frac{TP+TN}{TP+TN+FP+FN}
$$

Precision:

$$
\frac{TP}{TP+FP}
$$

Recall:

$$
\frac{TP}{TP+FN}
$$

Specificity:

$$
\frac{TN}{TN+FP}
$$

F1:

$$
2\frac{PR}{P+R}
$$

## Threshold

Probability menjadi class melalui threshold. Mengubah threshold menukar precision dan recall.

Threshold harus dipilih pada validation set berdasarkan cost atau constraint, bukan test set.

## ROC dan Precision–Recall

- ROC: TPR vs FPR.
- PR: precision vs recall.

Pada positive class sangat rare, PR curve sering lebih informatif tentang positive retrieval.

AUC merangkum ranking lintas threshold, tetapi tidak memberi threshold deployment.

## Multiclass Averaging

- macro: rata-rata class sama bobot
- weighted: bobot sesuai support
- micro: agregasi count global

Macro metric menonjolkan minority class.

## Regression Metrics

MAE:

$$
\frac{1}{n}\sum_i|y_i-\hat{y}_i|
$$

MSE:

$$
\frac{1}{n}\sum_i(y_i-\hat{y}_i)^2
$$

RMSE:

$$
\sqrt{\operatorname{MSE}}
$$

$R^2$ membandingkan dengan mean baseline, tetapi dapat negatif pada evaluation data.

## Calibration

Model calibrated: prediction confidence 0.8 benar sekitar 80% pada kelompok yang relevan.

Calibration berbeda dari discrimination/ranking.

## Implementasi

```python
from sklearn.metrics import (
    classification_report,
    confusion_matrix,
    precision_recall_curve,
)

print(confusion_matrix(y_true, y_pred))
print(classification_report(y_true, y_pred))

precision, recall, thresholds = precision_recall_curve(
    y_true,
    positive_scores,
)
```

## Clustering Evaluation

- silhouette
- adjusted Rand index jika reference tersedia
- domain usefulness
- stability

Internal metric tidak membuktikan cluster semantic.

## Studi Kasus: Defect Detection

Jika false negative sangat mahal:

- prioritaskan recall constraint
- ukur precision pada recall target
- report false negative count
- evaluate per defect type
- calibrate threshold pada realistic prevalence

## Confidence Interval

Metric adalah estimate dari sample. Gunakan:

- bootstrap
- repeated CV
- analytical interval jika assumptions cocok

Jangan melaporkan decimal precision berlebihan.

## Best Practice

- Tetapkan primary metric sebelum eksperimen.
- Laporkan confusion matrix.
- Sertakan per-class metric.
- Pilih threshold pada validation.
- Laporkan uncertainty.
- Audit subgroup.
- Hubungkan metric ke cost.

## Kesalahan Umum

- Accuracy pada imbalance.
- AUC dianggap deployment performance.
- Threshold dipilih pada test.
- Weighted average menyembunyikan minority failure.
- Metric tanpa baseline.
- Membandingkan test set berbeda.

## Debugging

- Cek positive label.
- Cek order argument `y_true, y_pred`.
- Cek score vs hard prediction.
- Cek class mapping.
- Recompute manual pada sample kecil.
- Inspect confusion matrix.

## Ringkasan

- Metric menyatakan tujuan evaluation tertentu.
- Threshold memengaruhi precision/recall.
- Averaging menentukan bobot class.
- Metric perlu uncertainty dan context.

## Checklist Pemahaman

- [ ] Bisa menghitung precision, recall, dan F1.
- [ ] Bisa memilih metric imbalance.
- [ ] Bisa menjelaskan macro/micro/weighted.
- [ ] Bisa memilih threshold.
- [ ] Bisa membedakan calibration dan ranking.
- [ ] Bisa melaporkan uncertainty.

## Latihan

1. Hitung metric dari confusion matrix.
2. Pilih threshold untuk recall minimum.
3. Bandingkan macro dan weighted F1.
4. Buat bootstrap confidence interval.

