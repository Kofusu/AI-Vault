---
type: concept
status: not-started
domain: scientific-computing
topic: pandas
level: foundations
order: 4
created: 2026-07-28
---

# Pandas Fundamentals

## Tujuan

- Memahami Series, DataFrame, Index, dtype, dan alignment.
- Membersihkan, menggabungkan, serta merangkum data.
- Mengaudit metadata dataset AI.

## Intuisi

Pandas mengelola data tabular berlabel:

```text
Index → identitas baris
Columns → variable
Cells → values
```

## Konsep Dasar

- `Series`: data satu dimensi dengan index.
- `DataFrame`: tabel dua dimensi dengan index dan columns.
- `Index`: label axis.
- `dtype`: representasi column.

```python
import pandas as pd

frame = pd.DataFrame({
    "path": ["a.jpg", "b.jpg"],
    "label": ["cat", "dog"],
    "width": [640, 1280],
    "height": [480, 720],
})
```

## Kenapa Dibutuhkan?

Dataset AI mempunyai metadata:

- path
- label
- split
- ukuran
- subject atau scene ID
- annotation count
- quality flag

Pandas membantu audit sebelum data masuk model.

## Cara Kerja Alignment

Pandas menyelaraskan operasi berdasarkan label:

```python
left = pd.Series([1, 2], index=["a", "b"])
right = pd.Series([10, 20], index=["b", "c"])

print(left + right)
```

Hasil mengikuti union index dan dapat menghasilkan missing value. Ini berbeda dari positional operation NumPy.

## Selection

```python
labels = frame["label"]
subset = frame[["path", "label"]]

row_by_label = frame.loc[0]
row_by_position = frame.iloc[0]
```

- `.loc`: label-based
- `.iloc`: position-based

Boolean filter:

```python
large = frame.loc[
    (frame["width"] >= 1000)
    & (frame["height"] >= 700)
]
```

Gunakan `&`, `|`, dan parenthesis untuk Series boolean.

## Missing Data

```python
frame.isna().sum()
frame.dropna(subset=["path", "label"])
frame["width"] = frame["width"].fillna(
    frame["width"].median()
)
```

Jangan fill missing value sebelum memahami penyebab dan maknanya.

## Dtype

```python
frame["label"] = frame["label"].astype("category")
frame["width"] = pd.to_numeric(
    frame["width"],
    errors="coerce",
)
```

Conversion dengan `errors="coerce"` mengubah invalid value menjadi missing; audit jumlahnya.

## GroupBy

Split–apply–combine:

```python
summary = (
    frame.groupby("label", dropna=False)
    .agg(
        samples=("path", "size"),
        mean_width=("width", "mean"),
        mean_height=("height", "mean"),
    )
    .reset_index()
)
```

Built-in aggregation biasanya lebih efisien dan jelas daripada Python UDF lewat `apply`.

## Merge

```python
images = pd.DataFrame({
    "image_id": [1, 2],
    "path": ["a.jpg", "b.jpg"],
})
annotations = pd.DataFrame({
    "image_id": [1, 1, 2],
    "class_id": [3, 5, 3],
})

merged = images.merge(
    annotations,
    on="image_id",
    how="left",
    validate="one_to_many",
    indicator=True,
)
```

`validate` memeriksa cardinality assumption.

## Copy-on-Write dan Chained Assignment

Gunakan explicit `.loc` assignment:

```python
frame.loc[frame["width"] < 1, "width"] = pd.NA
```

Hindari chained assignment yang ambiguity:

```python
frame[frame["width"] < 1]["width"] = pd.NA
```

Pandas modern memakai Copy-on-Write semantics, tetapi explicit operation tetap paling jelas.

## I/O

```python
frame = pd.read_csv("manifest.csv")
frame.to_csv("clean-manifest.csv", index=False)

frame.to_parquet("manifest.parquet", index=False)
```

CSV mudah dipertukarkan tetapi tidak menyimpan dtype dengan kaya. Parquet cocok untuk typed columnar data.

## Implementasi: Dataset Audit

```python
def audit_manifest(
    frame: pd.DataFrame,
) -> pd.DataFrame:
    required = {"path", "label", "width", "height"}
    missing_columns = required - set(frame.columns)

    if missing_columns:
        raise ValueError(
            f"Missing columns: {sorted(missing_columns)}"
        )

    result = frame.copy()
    result["aspect_ratio"] = (
        result["width"] / result["height"]
    )
    result["duplicate_path"] = result["path"].duplicated(
        keep=False
    )
    return result
```

## Studi Kasus: Split Leakage

```python
duplicates = (
    frame.groupby("path")["split"]
    .nunique()
    .loc[lambda values: values > 1]
)
```

Untuk video atau subject data, group berdasarkan `video_id`, `subject_id`, atau scene—not only path.

## Best Practice

- Tetapkan schema dan required columns.
- Audit missing, duplicate, range, dan cardinality.
- Gunakan `.loc` dan `.iloc` secara sadar.
- Validasi merge.
- Hindari row-wise `apply` jika ada built-in vectorized method.
- Simpan clean output terpisah dari raw data.

## Kesalahan Umum

- Menganggap index selalu row number.
- Chained assignment.
- Silent dtype conversion.
- Merge many-to-many tanpa sadar.
- Missing value dihapus tanpa analisis.
- Iterasi row untuk operasi yang vectorizable.

## Debugging

```python
print(frame.shape)
print(frame.dtypes)
print(frame.head())
print(frame.isna().sum())
print(frame.nunique())
print(frame.duplicated().sum())
```

Setelah merge, cek `_merge`, jumlah baris, dan key duplicate.

## Ringkasan

- Pandas mengelola data tabular berlabel.
- Alignment dan index memengaruhi operasi.
- GroupBy merangkum kelompok.
- Merge harus mempunyai cardinality contract.

## Checklist Pemahaman

- [ ] Bisa membedakan Series dan DataFrame.
- [ ] Bisa memakai `.loc` dan `.iloc`.
- [ ] Bisa mengaudit missing serta duplicate.
- [ ] Bisa membuat GroupBy aggregation.
- [ ] Bisa melakukan validated merge.
- [ ] Bisa mendeteksi split leakage.

## Latihan

1. Buat manifest 10 gambar.
2. Tambahkan aspect ratio.
3. Ringkas jumlah sample per class.
4. Cari duplicate path lintas split.

## Referensi

- [pandas User Guide](https://pandas.pydata.org/docs/user_guide/)
- [pandas GroupBy](https://pandas.pydata.org/docs/user_guide/groupby.html)

