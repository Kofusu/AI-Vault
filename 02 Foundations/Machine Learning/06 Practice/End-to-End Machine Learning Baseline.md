---
type: concept
status: not-started
domain: machine-learning
topic: practice
level: foundations
order: 12
created: 2026-07-28
---

# End-to-End Machine Learning Baseline

## Tujuan

- Menggabungkan problem, data, split, pipeline, training, evaluation, dan error analysis.
- Membuat baseline reproducible.
- Menentukan next experiment berdasarkan evidence.

## Intuisi

Baseline bukan model asal. Baseline adalah implementation sederhana, valid, dan dapat direproduksi yang menjadi pembanding.

## Konsep Dasar

Artifact minimum:

- problem statement
- dataset version
- split manifest
- preprocessing Pipeline
- dummy baseline
- model baseline
- evaluation report
- config dan environment
- error analysis

## Kenapa Dibutuhkan?

Model kompleks tanpa baseline menyulitkan diagnosis. Baseline membantu membedakan:

- signal data
- bug pipeline
- leakage
- improvement nyata
- compute trade-off

## Cara Kerja

```text
1. Define task dan metric
2. Load dan validate data
3. Freeze split
4. Build dummy baseline
5. Build simple pipeline
6. Cross-validate
7. Select dengan validation
8. Evaluate test sekali
9. Error analysis
10. Record artifacts
```

## Implementasi

```python
from sklearn.dummy import DummyClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import classification_report
from sklearn.model_selection import train_test_split
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y,
)

dummy = DummyClassifier(strategy="prior")
dummy.fit(X_train, y_train)

baseline = make_pipeline(
    StandardScaler(),
    LogisticRegression(max_iter=1000),
)
baseline.fit(X_train, y_train)

predictions = baseline.predict(X_test)
print(classification_report(y_test, predictions))
```

Untuk project nyata, gunakan validation atau CV untuk selection sebelum test.

## Project Structure

```text
ml-baseline/
├── README.md
├── pyproject.toml
├── configs/
├── data/
│   └── manifests/
├── src/
│   ├── data.py
│   ├── features.py
│   ├── train.py
│   └── evaluate.py
├── tests/
└── reports/
```

Raw data tidak perlu masuk Git.

## Sanity Check

- jumlah row
- feature/target alignment
- class distribution
- missing/duplicate
- split overlap
- dummy score
- overfit subset kecil
- prediction distribution

## Cross-Validation

Gunakan splitter sesuai unit. Simpan:

- fold score
- mean/std
- training time
- config
- seed

## Error Analysis

Buat tabel:

```text
sample_id
true_label
predicted_label
confidence
subgroup
notes
```

Kelompokkan error:

- label issue
- feature issue
- minority class
- domain shift
- ambiguous case

## Studi Kasus: Image Metadata Baseline

Gunakan feature width, height, aspect ratio, file size, atau simple histogram untuk menguji apakah dataset mempunyai shortcut.

Jika metadata saja memprediksi label sangat baik, bisa jadi ada collection bias.

## Reproducibility

Catat:

- Python/package version
- code commit
- dataset/split version
- config
- seed
- metric implementation
- hardware bila relevan

## Best Practice

- Mulai dummy.
- Gunakan Pipeline.
- Test hanya sekali setelah selection.
- Simpan split.
- Tulis limitation.
- Jadikan next experiment satu perubahan utama.
- Bandingkan accuracy dan cost.

## Kesalahan Umum

- Baseline terlalu kompleks.
- Tidak ada dummy comparator.
- Test dipakai iteratif.
- Manual preprocessing berbeda.
- Hanya menyimpan model.
- Tidak melakukan error analysis.

## Debugging

Jika score suspiciously tinggi:

- shuffle label
- hapus suspicious feature
- group split
- cek duplicate
- cek target-derived feature

Jika score setara dummy:

- cek label alignment
- cek feature variance
- overfit subset
- inspect preprocessing output

## Ringkasan

- Baseline valid adalah fondasi experiment.
- Pipeline dan split mencegah leakage.
- Error analysis menentukan next action.
- Reproducibility mencakup data, code, config, dan environment.

## Checklist Pemahaman

- [ ] Bisa merancang baseline.
- [ ] Bisa membuat dummy comparator.
- [ ] Bisa menggunakan Pipeline.
- [ ] Bisa menyimpan split dan config.
- [ ] Bisa melakukan error analysis.
- [ ] Bisa menentukan next experiment.

## Latihan

1. Bangun baseline Iris.
2. Tambahkan dummy classifier.
3. Simpan per-sample error.
4. Tulis satu next experiment.

## Referensi

- [Scikit-Learn Getting Started](https://scikit-learn.org/stable/getting_started.html)
- [Scikit-Learn Common Pitfalls](https://scikit-learn.org/stable/common_pitfalls.html)

