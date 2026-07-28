---
type: concept
status: not-started
domain: machine-learning
topic: data
level: foundations
order: 8
created: 2026-07-28
---

# Feature Engineering and Preprocessing

## Tujuan

- Memahami feature sebagai representasi input.
- Mengenal scaling, encoding, imputation, transformation, dan selection.
- Mencegah preprocessing leakage.

## Intuisi

Model hanya melihat representation:

```text
Raw observation
 ↓ preprocessing
Features
 ↓ model
Prediction
```

Representation menentukan pola yang mudah dipelajari.

## Konsep Dasar

Feature engineering menggunakan domain knowledge untuk membentuk variable informatif. Preprocessing membuat data sesuai contract model.

## Kenapa Dibutuhkan?

Classical model sering sensitif terhadap:

- scale
- missing value
- categorical encoding
- skew
- irrelevant feature
- nonlinearity

## Cara Kerja

### Scaling

Standardization:

$$
z=\frac{x-\mu}{\sigma}
$$

$\mu$ dan $\sigma$ dihitung dari train.

### Categorical Encoding

- one-hot
- ordinal jika urutan nyata
- target encoding dengan leakage-safe procedure

### Missing Value

- imputation
- missing indicator
- model native handling

Missingness dapat membawa informasi tetapi juga bias.

### Transformation

- log transform
- polynomial feature
- aggregation
- domain ratio

## Feature CV Klasik

- color histogram
- HOG
- edge count
- keypoint descriptor
- shape statistic

Deep Learning mempelajari feature, tetapi preprocessing dan input representation tetap penting.

## Implementasi: Pipeline

```python
from sklearn.impute import SimpleImputer
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler

pipeline = make_pipeline(
    SimpleImputer(strategy="median"),
    StandardScaler(),
    model,
)
```

## Column Transformer

```python
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder

preprocessor = ColumnTransformer(
    transformers=[
        ("numeric", StandardScaler(), numeric_columns),
        (
            "categorical",
            OneHotEncoder(handle_unknown="ignore"),
            categorical_columns,
        ),
    ]
)
```

## Feature Selection

- filter method
- wrapper method
- embedded method

Selection harus dilakukan di dalam training fold.

## Studi Kasus: Lifestyle App

Raw pose keypoint dapat diubah menjadi:

- joint angle
- normalized distance
- symmetry
- temporal velocity

Normalization harus memperhatikan scale tubuh dan camera geometry.

## Best Practice

- Mulai dari raw/simple feature baseline.
- Fit transform hanya train.
- Simpan transform bersama model.
- Dokumentasikan unit.
- Audit feature availability saat inference.
- Gunakan Pipeline.

## Kesalahan Umum

- Scaling seluruh dataset.
- Ordinal encoding untuk nominal category.
- Feature tersedia hanya setelah outcome.
- Manual preprocessing berbeda saat production.
- Terlalu banyak feature tanpa validation.

## Debugging

- Cek output shape dan feature names.
- Inspect statistic fitted.
- Test unseen category.
- Uji missing value.
- Bandingkan pipeline training/inference.
- Ablate feature group.

## Ringkasan

- Feature adalah representation input model.
- Preprocessing harus fit pada training data.
- Pipeline menjaga consistency dan mengurangi leakage.

## Checklist Pemahaman

- [ ] Bisa menjelaskan scaling dan encoding.
- [ ] Bisa membuat Pipeline.
- [ ] Bisa mengenali target leakage.
- [ ] Bisa mendesain feature domain.
- [ ] Bisa melakukan ablation.

## Latihan

1. Buat mixed-column preprocessor.
2. Tambahkan missing value.
3. Uji unseen category.
4. Rancang pose feature.
