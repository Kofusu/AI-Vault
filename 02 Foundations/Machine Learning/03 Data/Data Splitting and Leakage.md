---
type: concept
status: not-started
domain: machine-learning
topic: data
level: foundations
order: 7
created: 2026-07-28
---

# Data Splitting and Leakage

## Tujuan

- Memahami fungsi train, validation, dan test.
- Memilih random, stratified, group, atau temporal split.
- Mendeteksi data dan target leakage.

## Intuisi

Evaluation harus mensimulasikan data masa depan yang benar-benar belum terlihat.

## Konsep Dasar

- Train: fit parameter.
- Validation: model/hyperparameter selection.
- Test: final estimate.

Test kehilangan status “unseen” jika dipakai berulang untuk keputusan.

## Kenapa Dibutuhkan?

Tanpa split yang benar, metric terlalu optimistis dan gagal memprediksi deployment.

## Cara Kerja

```text
Raw dataset
 ↓ define independent unit
Split IDs
 ├── train
 ├── validation
 └── test
 ↓
Fit hanya pada train
```

## Split Strategy

### Random

Cocok jika sample IID cukup masuk akal.

### Stratified

Menjaga proporsi label.

### Group

Semua sample subject/video/scene yang sama berada di satu split.

### Temporal

Train pada masa lalu, evaluate masa depan.

### Geographic atau Domain Holdout

Menguji generalization lintas lokasi/device.

## Data Leakage

Informasi evaluation masuk training:

- duplicate lintas split
- preprocessing sebelum split
- feature selection seluruh data
- patient sama lintas split
- adjacent frame lintas split

## Target Leakage

Feature memuat informasi target yang tidak tersedia saat inference:

- field pasca-outcome
- filename berisi label
- annotation-derived statistic
- timestamp yang membocorkan event

## Implementasi Group Split

```python
from sklearn.model_selection import GroupShuffleSplit

splitter = GroupShuffleSplit(
    n_splits=1,
    test_size=0.2,
    random_state=42,
)

train_idx, test_idx = next(
    splitter.split(X, y, groups=subject_ids)
)
```

## Preprocessing

Benar:

```text
split
 ↓
fit scaler pada train
 ↓
transform train/val/test
```

Gunakan Pipeline untuk cross-validation.

## Studi Kasus: Video

Random frame split hampir pasti bocor karena adjacent frame sangat mirip. Split berdasarkan video atau recording session.

## Best Practice

- Definisikan independent unit.
- Simpan split ID.
- Split sebelum learned preprocessing.
- Deduplicate sebelum atau dengan awareness split.
- Lock test set.
- Uji deployment-like holdout.

## Kesalahan Umum

- Random split sebagai default universal.
- Test untuk tuning.
- Scaling sebelum split.
- Subject overlap.
- Menghapus duplicate setelah split tanpa audit.

## Debugging

```python
assert not set(train_ids) & set(test_ids)
assert not set(train_groups) & set(test_groups)
```

- Cek duplicate.
- Train model pada suspicious feature.
- Audit unexpectedly high score.
- Bandingkan random vs group split.

## Ringkasan

- Split mendefinisikan claim generalization.
- Unit independen lebih penting daripada row.
- Leakage membuat estimate tidak valid.

## Checklist Pemahaman

- [ ] Bisa menjelaskan peran tiap split.
- [ ] Bisa memilih split strategy.
- [ ] Bisa membedakan data dan target leakage.
- [ ] Bisa membuat group split.
- [ ] Bisa menjaga test set.

## Latihan

1. Rancang split video dataset.
2. Cari target leakage example.
3. Bandingkan random dan group split.
4. Tulis assertion overlap.

