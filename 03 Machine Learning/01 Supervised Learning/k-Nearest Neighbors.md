---
type: concept
status: not-started
domain: machine-learning
topic: supervised-learning
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# k-Nearest Neighbors

## Tujuan

Memahami instance-based learning yang memprediksi berdasarkan tetangga terdekat.

## Intuisi

Data yang berdekatan diasumsikan memiliki target serupa:

```text
Sample baru → hitung distance → ambil k tetangga → voting/rata-rata
```

## Cara Kerja

1. Simpan training data.
2. Hitung distance dari query ke setiap training sample.
3. Pilih $k$ distance terkecil.
4. Classification memakai majority vote; regression memakai rata-rata.

## Distance

Euclidean:

$$
d(\mathbf{x},\mathbf{z})
=
\sqrt{\sum_j(x_j-z_j)^2}
$$

Lihat [[Vector Geometry]].

## Pemilihan k

- $k$ terlalu kecil → sensitif noise, variance tinggi
- $k$ terlalu besar → boundary terlalu halus, bias tinggi

Pilih $k$ memakai cross-validation.

## Implementasi

```python
from sklearn.neighbors import KNeighborsClassifier
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler

model = Pipeline([
    ("scale", StandardScaler()),
    ("knn", KNeighborsClassifier(
        n_neighbors=5,
        weights="distance",
        metric="minkowski",
        p=2,
    )),
])

model.fit(X_train, y_train)
prediction = model.predict(X_test)
```

## Kompleksitas

Implementasi brute-force:

- training: sekitar $O(1)$ selain menyimpan data
- prediction per query: $O(nd)$
- memory: $O(nd)$

KD-tree atau Ball-tree dapat membantu pada dimensi rendah, tetapi manfaatnya turun pada dimensi tinggi.

## Curse of Dimensionality

Saat dimensi tinggi, distance antarpoint menjadi kurang diskriminatif dan ruang menjadi sangat sparse. Feature selection, PCA, atau embedding sering dibutuhkan.

## Kelebihan

- sederhana
- tidak membuat asumsi boundary linear
- mudah menjadi baseline

## Kekurangan

- inference lambat
- memory besar
- sensitif terhadap skala dan fitur tidak relevan
- buruk pada dimensi tinggi

## Best Practice

- Scale fitur.
- Pilih metric sesuai domain.
- Gunakan validation untuk memilih $k$.
- Pertimbangkan approximate nearest neighbor untuk data besar.

## Kesalahan Umum

- Tidak melakukan scaling.
- Memilih $k$ berdasarkan test score.
- Memakai identifier atau fitur noise dalam distance.
- Menganggap training murah berarti seluruh sistem murah.

## Ringkasan

k-NN memindahkan biaya dari training ke inference dan sangat bergantung pada representasi fitur serta distance metric.

## Hubungan Konsep

- [[Vector Geometry]]
- [[Principal Component Analysis]]
- [[Cross-Validation and Model Selection]]

## Checklist Pemahaman

- [ ] Bisa menjelaskan proses voting
- [ ] Paham pengaruh k
- [ ] Paham pentingnya scaling
- [ ] Bisa menjelaskan curse of dimensionality

## Latihan

1. Implementasikan k-NN sederhana dengan NumPy.
2. Plot validation score untuk beberapa nilai $k$.
3. Bandingkan performa sebelum dan sesudah StandardScaler.

