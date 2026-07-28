---
type: concept
status: not-started
domain: machine-learning
topic: data
level: foundations
order: 6
created: 2026-07-28
---

# Dataset and Data Quality

## Tujuan

- Memahami dataset sebagai sample dari target population.
- Mengaudit schema, label, missing, duplicate, bias, dan provenance.
- Membuat dataset version yang reproducible.

## Intuisi

Model belajar dari data yang diberikan, bukan dari dunia ideal.

```text
World
 ↓ collection process
Dataset
 ↓ model
Learned behavior
```

Collection process menentukan apa yang mungkin dipelajari.

## Konsep Dasar

- population
- sampling frame
- sample
- feature
- label
- metadata
- annotation guideline
- provenance
- license

## Kenapa Dibutuhkan?

Model yang baik tidak dapat memperbaiki target salah, leakage, atau population gap secara otomatis.

## Cara Kerja Dataset Lifecycle

```text
Define population
 ↓ collect
Raw data
 ↓ validate/annotate
Versioned dataset
 ↓ split
Train/val/test
 ↓ monitor
Feedback dan update
```

## Quality Dimensions

- completeness
- correctness
- consistency
- uniqueness
- validity
- representativeness
- timeliness

## Label Quality

Audit:

- ambiguous class
- annotator disagreement
- missing annotation
- boundary consistency
- taxonomy drift

Gold label tetap dapat mempunyai uncertainty.

## Duplicate

Duplicate dapat berupa:

- exact file
- resized/re-encoded copy
- adjacent video frame
- same subject/scene

Hash file hanya mendeteksi exact duplicate. Perceptual similarity membutuhkan pendekatan lain.

## Data Bias

- geographic
- demographic
- device/camera
- lighting
- background
- temporal
- selection
- annotation

Bias harus dianalisis terhadap intended use.

## Implementasi Audit

```python
required = {"path", "label", "split"}

if missing := required - set(frame.columns):
    raise ValueError(f"Missing: {sorted(missing)}")

print(frame.isna().sum())
print(frame["label"].value_counts(dropna=False))
print(frame["path"].duplicated().sum())
```

## Dataset Versioning

Simpan:

- source
- license
- collection date
- schema
- class mapping
- split IDs
- checksum/manifest
- transformation history

## Studi Kasus: Smart Fitness App

Population target beragam:

- body shape
- clothing
- camera angle
- device
- lighting
- mobility condition

Dataset studio yang sempit dapat menghasilkan high test score tetapi gagal di rumah.

## Best Practice

- Buat dataset card.
- Simpan raw data read-only.
- Version manifest dan split.
- Visualisasikan random sample.
- Audit subgroup.
- Catat exclusion criteria.
- Perlakukan privacy/license sebagai requirement.

## Kesalahan Umum

- Dataset dianggap netral.
- Hapus missing tanpa alasan.
- Duplicate lintas split.
- Class ID berubah diam-diam.
- Raw data ditimpa.
- License tidak diperiksa.

## Debugging

- Cek count sebelum/sesudah cleaning.
- Sampling visual.
- Validasi file decode.
- Cek class mapping.
- Cek duplicate dan group overlap.
- Bandingkan distribution split.

## Ringkasan

- Dataset adalah hasil sampling dan measurement process.
- Quality, bias, provenance, dan license menentukan validity.
- Versioning membuat experiment dapat direproduksi.

## Checklist Pemahaman

- [ ] Bisa membedakan population dan sample.
- [ ] Bisa mengaudit quality dimension.
- [ ] Bisa mengenali duplicate non-exact.
- [ ] Bisa membuat dataset card.
- [ ] Bisa menjelaskan bias terhadap intended use.

## Latihan

1. Buat dataset card.
2. Audit missing dan duplicate.
3. Definisikan subgroup.
4. Tulis limitation dataset.

