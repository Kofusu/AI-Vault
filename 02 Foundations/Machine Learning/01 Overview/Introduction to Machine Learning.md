---
type: concept
status: not-started
domain: machine-learning
topic: foundations
level: foundations
order: 1
created: 2026-07-28
---

# Introduction to Machine Learning

## Tujuan

- Memahami definisi dan komponen sistem ML.
- Membedakan training dan inference.
- Memformulasikan problem, input, target, model, loss, serta metric.

## Intuisi

Machine Learning membuat program belajar pola dari data, bukan menuliskan seluruh aturan secara manual.

```text
Traditional programming:
Rules + Data → Output

Machine Learning:
Data + Desired output → Learned model
Model + New data → Prediction
```

## Konsep Dasar

Komponen:

- sample
- feature $X$
- target $y$
- model $f_\theta$
- prediction $\hat{y}$
- loss $L$
- optimizer atau learning algorithm
- evaluation metric

$$
\hat{y}=f_\theta(X)
$$

Training mencari parameter:

$$
\theta^*
=
\arg\min_\theta
\frac{1}{n}\sum_{i=1}^{n}
L(f_\theta(x_i),y_i)
$$

## Kenapa Dibutuhkan?

ML cocok ketika:

- aturan sulit ditulis manual
- data mempunyai pola
- outcome dapat didefinisikan
- performa dapat diukur
- distribution deployment cukup berkaitan dengan training

## Cara Kerja

```text
Problem definition
  ↓ collect/label data
Dataset
  ↓ split
Training data
  ↓ learning algorithm
Model
  ↓ validation
Model selection
  ↓ final test
Performance estimate
  ↓ deployment
Monitoring
```

## Training vs Inference

- Training mempelajari parameter dari data.
- Inference memakai parameter tetap untuk prediction baru.

Training biasanya lebih mahal; inference mempunyai constraint latency, memory, throughput, atau energy.

## Learning Paradigm

- [[Supervised Learning]]
- [[Unsupervised Learning]]
- [[Semi-Supervised Learning]]
- [[Self-Supervised Learning]]

## Task

```text
Classification → class
Regression     → continuous value
Clustering     → groups
Detection      → class + location
Segmentation   → label per-pixel
Ranking        → ordered items
Generation     → new sample
```

## Loss vs Metric

Loss dipakai learning algorithm dan biasanya differentiable atau mudah dioptimalkan. Metric dipakai menilai outcome bisnis/riset.

Contoh:

```text
Training loss → cross-entropy
Reported metric → macro F1
```

Keduanya boleh berbeda.

## Baseline

Baseline adalah titik pembanding:

- rule sederhana
- majority class
- linear model
- model existing

Tanpa baseline, improvement sulit diinterpretasikan.

## Implementasi Konseptual

```python
model.fit(X_train, y_train)
predictions = model.predict(X_validation)
score = metric(y_validation, predictions)
```

## Studi Kasus: Defect Detection

Pertanyaan:

- Unit prediction: image atau product?
- Positive class: defect?
- Cost false negative?
- Distribution antar-camera?
- Apakah label konsisten?
- Latency target?

Model choice datang setelah problem dan evaluation protocol jelas.

## Best Practice

- Definisikan unit prediction.
- Tulis target dan success criteria.
- Mulai dari baseline.
- Pisahkan loss dan metric.
- Audit data sebelum modeling.
- Dokumentasikan asumsi deployment.
- Monitor distribution shift.

## Kesalahan Umum

- Memulai dari model populer.
- Target ambigu.
- Metric tidak sesuai cost.
- Data split setelah preprocessing.
- Menganggap test score menjamin production.
- Mengabaikan non-ML solution.

## Debugging

Jika model gagal:

1. validasi label
2. overfit sample kecil
3. cek feature/target alignment
4. bandingkan dummy baseline
5. inspect failure case
6. cek distribution shift

## Ringkasan

- ML belajar mapping atau structure dari data.
- Problem definition mendahului model.
- Training, validation, testing, dan inference mempunyai peran berbeda.
- Baseline dan metric membuat kemajuan dapat dinilai.

## Checklist Pemahaman

- [ ] Bisa menjelaskan $X$, $y$, $\theta$, loss, dan metric.
- [ ] Bisa membedakan training dan inference.
- [ ] Bisa memilih task.
- [ ] Bisa menjelaskan baseline.
- [ ] Bisa memformulasikan satu problem CV.

## Latihan

1. Formulasikan food classification.
2. Tentukan unit prediction dan metric.
3. Buat non-ML baseline.
4. Jelaskan risiko deployment shift.

