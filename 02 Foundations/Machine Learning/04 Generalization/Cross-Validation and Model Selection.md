---
type: concept
status: not-started
domain: machine-learning
topic: generalization
level: foundations
order: 10
created: 2026-07-28
---

# Cross-Validation and Model Selection

## Tujuan

- Memahami K-fold dan variasinya.
- Memilih model/hyperparameter tanpa memakai test.
- Menghindari selection bias dan nested tuning error.

## Intuisi

Cross-validation merotasi validation subset:

```text
Fold 1: V T T T T
Fold 2: T V T T T
Fold 3: T T V T T
...
```

## Konsep Dasar

K-fold:

1. bagi training data menjadi $K$ fold
2. fit pada $K-1$
3. evaluate pada satu
4. ulangi
5. ringkas mean dan dispersion

## Kenapa Dibutuhkan?

Satu validation split dapat tidak stabil, terutama dataset kecil. CV memakai data lebih efisien untuk model selection.

## Cara Kerja

Estimator baru harus fit pada setiap fold. Learned preprocessing juga fit ulang di training fold.

## Variasi

- KFold
- StratifiedKFold
- GroupKFold
- TimeSeriesSplit
- repeated CV
- nested CV

Pilih berdasarkan dependency data.

## Model vs Hyperparameter

- Parameter dipelajari oleh `fit`.
- Hyperparameter dipilih di luar fit.

Contoh hyperparameter:

- regularization strength
- tree depth
- number of neighbors

## Implementasi

```python
from sklearn.model_selection import cross_validate

result = cross_validate(
    pipeline,
    X,
    y,
    cv=5,
    scoring=["accuracy", "f1_macro"],
    return_train_score=True,
)
```

## Hyperparameter Search

```python
from sklearn.model_selection import GridSearchCV

search = GridSearchCV(
    pipeline,
    param_grid={"model__C": [0.1, 1.0, 10.0]},
    scoring="f1_macro",
    cv=5,
)
```

## Nested CV

Outer loop estimate generalization; inner loop memilih hyperparameter. Berguna ketika dataset kecil dan claim performance membutuhkan separation ketat.

## Studi Kasus: Subject Data

Gunakan group-aware CV agar satu subject tidak muncul pada train dan validation fold.

Random stratified K-fold masih bocor jika banyak sample per subject.

## Multiple Comparisons

Mencoba banyak model pada validation yang sama dapat overfit validation. Catat search space dan gunakan test final sekali.

## Best Practice

- Gunakan Pipeline.
- Pilih splitter sesuai data.
- Laporkan fold scores.
- Tetapkan metric utama.
- Simpan search space.
- Jangan tuning test.
- Gunakan nested CV jika perlu.

## Kesalahan Umum

- Preprocessing sebelum CV.
- Fold bocor subject.
- Memilih model dari test.
- Hanya melaporkan fold terbaik.
- Membandingkan model pada fold berbeda.
- Search budget tidak fair.

## Debugging

- Inspect index setiap fold.
- Assert group disjoint.
- Cek class count per fold.
- Cek pipeline step.
- Set `error_score="raise"` saat diagnosis.
- Simpan per-fold result.

## Ringkasan

- CV mengestimasi performa dengan beberapa split.
- Splitter harus mengikuti dependency data.
- Test set tetap terpisah dari selection.
- Nested CV memisahkan tuning dan estimation.

## Checklist Pemahaman

- [ ] Bisa menjelaskan K-fold.
- [ ] Bisa memilih stratified/group/time split.
- [ ] Bisa membedakan parameter dan hyperparameter.
- [ ] Bisa menjelaskan nested CV.
- [ ] Bisa mencegah preprocessing leakage.

## Latihan

1. Buat stratified CV.
2. Ubah menjadi group CV.
3. Bandingkan fold dispersion.
4. Desain nested CV.

