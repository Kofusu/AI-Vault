---
type: concept
status: not-started
domain: scientific-computing
topic: scikit-learn
level: foundations
order: 8
created: 2026-07-28
---

# Scikit-Learn Fundamentals

## Tujuan

- Memahami estimator, transformer, pipeline, split, fit, predict, dan score.
- Membuat baseline yang fair tanpa data leakage.
- Mengevaluasi classification dengan metric yang sesuai.

## Intuisi

Scikit-Learn memberi interface konsisten:

```text
Data
 ↓ split
Train / Validation / Test
 ↓ fit preprocessing + model pada train
Pipeline
 ↓ transform/predict
Evaluation
```

## Konsep Dasar

- **Estimator:** object yang belajar melalui `fit`.
- **Transformer:** estimator dengan `transform`.
- **Predictor:** estimator dengan `predict`.
- **Pipeline:** rangkaian transformer dan estimator.

## Kenapa Dibutuhkan?

Scikit-Learn cocok untuk:

- baseline cepat
- classical ML
- preprocessing
- cross-validation
- hyperparameter search
- metrics
- tabular atau engineered feature

Baseline sederhana penting sebelum memakai model kompleks.

## Cara Kerja Estimator API

```python
model.fit(X_train, y_train)
predictions = model.predict(X_test)
```

`fit` mempelajari parameter dari data. `predict` menggunakan state yang sudah dipelajari.

Transformer:

```python
scaler.fit(X_train)
X_train_scaled = scaler.transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

Jangan `fit` scaler pada test data.

## Train–Validation–Test

- Train: belajar parameter.
- Validation: memilih model/hyperparameter.
- Test: estimasi akhir setelah keputusan selesai.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y,
)
```

Random split tidak cocok untuk semua data. Gunakan group/time-aware split jika subject, scene, atau waktu dapat bocor.

## Pipeline

```python
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler

pipeline = make_pipeline(
    StandardScaler(),
    LogisticRegression(max_iter=1000),
)

pipeline.fit(X_train, y_train)
predictions = pipeline.predict(X_test)
```

Pipeline memastikan preprocessing yang dipelajari hanya fit pada training fold saat cross-validation.

## Data Leakage

Leakage terjadi ketika informasi yang tidak tersedia saat prediction ikut membangun model.

Contoh:

- scaling sebelum split
- feature selection memakai seluruh data
- duplicate subject lintas split
- target bocor ke feature
- preprocessing statistic dari test

## Cross-Validation

```python
from sklearn.model_selection import cross_validate

results = cross_validate(
    pipeline,
    X,
    y,
    cv=5,
    scoring=["accuracy", "f1_macro"],
    return_train_score=True,
)
```

Fold strategy harus sesuai struktur data.

## Classification Metrics

Confusion matrix:

```text
                 Predicted
               +          -
Actual +       TP         FN
Actual -       FP         TN
```

$$
\text{precision}
=
\frac{TP}{TP+FP}
$$

$$
\text{recall}
=
\frac{TP}{TP+FN}
$$

$$
F_1
=
2\frac{PR}{P+R}
$$

Accuracy dapat misleading pada class imbalance.

## Implementasi End-to-End

```python
from sklearn.datasets import load_iris
from sklearn.metrics import classification_report
from sklearn.model_selection import train_test_split

X, y = load_iris(return_X_y=True)

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y,
)

pipeline.fit(X_train, y_train)
predictions = pipeline.predict(X_test)

print(classification_report(y_test, predictions))
```

## Hyperparameter Search

```python
from sklearn.model_selection import GridSearchCV

search = GridSearchCV(
    pipeline,
    param_grid={
        "logisticregression__C": [0.1, 1.0, 10.0],
    },
    scoring="f1_macro",
    cv=5,
)

search.fit(X_train, y_train)
```

Test set tetap tidak dipakai selama search.

## Model Persistence

Serialized model dapat terikat pada library version dan berisiko jika sumber tidak terpercaya. Simpan:

- preprocessing + model sebagai satu pipeline
- dependency version
- feature schema
- training metadata

Jangan load pickle/joblib dari sumber tidak dipercaya.

## Studi Kasus: Image Feature Baseline

Sebelum CNN:

```text
Images
 ↓ fixed preprocessing
HOG atau engineered feature
 ↓
Scaler + linear classifier
 ↓
Cross-validation
```

Baseline membantu mengetahui apakah task sudah dapat diselesaikan dengan feature sederhana.

## Best Practice

- Split sebelum learned preprocessing.
- Gunakan Pipeline.
- Pilih split berdasarkan unit independen.
- Mulai dengan dummy baseline.
- Pilih metric sesuai cost error.
- Laporkan cross-validation dispersion.
- Simpan test untuk evaluasi akhir.
- Catat feature schema.

## Kesalahan Umum

- Fit scaler pada seluruh data.
- Memakai test untuk tuning.
- Random split pada subject/video.
- Accuracy untuk imbalance.
- Membandingkan model dengan fold berbeda.
- Menyimpan model tanpa preprocessing.
- Load serialized model tidak terpercaya.

## Debugging

- Cek `X.shape`, `y.shape`, dtype, dan missing.
- Overfit sample kecil sebagai sanity check.
- Bandingkan dummy classifier.
- Inspect confusion matrix.
- Gunakan `pipeline.get_params()` untuk nama hyperparameter.
- Cek class distribution pada setiap split.

## Ringkasan

- Estimator belajar melalui `fit`.
- Pipeline menggabungkan preprocessing dan model.
- Split strategy adalah bagian dari problem definition.
- Leakage menghasilkan evaluasi terlalu optimistis.

## Checklist Pemahaman

- [ ] Bisa membedakan estimator dan transformer.
- [ ] Bisa membuat Pipeline.
- [ ] Bisa memilih split strategy.
- [ ] Bisa menjelaskan leakage.
- [ ] Bisa menghitung precision dan recall.
- [ ] Bisa membuat baseline dan cross-validation.

## Latihan

1. Buat Pipeline scaler + logistic regression.
2. Bandingkan dengan dummy classifier.
3. Evaluasi accuracy dan macro F1.
4. Demonstrasikan leakage lalu perbaiki.

## Referensi

- [Scikit-Learn Getting Started](https://scikit-learn.org/stable/getting_started.html)
- [Scikit-Learn Common Pitfalls](https://scikit-learn.org/stable/common_pitfalls.html)
- [train_test_split](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)

