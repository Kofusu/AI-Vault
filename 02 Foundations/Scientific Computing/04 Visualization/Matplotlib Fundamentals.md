---
type: concept
status: not-started
domain: scientific-computing
topic: matplotlib
level: foundations
order: 5
created: 2026-07-28
---

# Matplotlib Fundamentals

## Tujuan

- Memahami Figure, Axes, Axis, dan Artist.
- Membuat plot yang jelas dan reproducible.
- Memvisualisasikan distribusi, training curve, dan image grid.

## Intuisi

```text
Figure
└── Axes
    ├── Axis x dan y
    ├── Line
    ├── Image
    ├── Text
    └── Legend
```

`Axes` adalah area plot; `Axis` adalah satu arah coordinate beserta ticks dan label.

## Konsep Dasar

Gunakan object-oriented API:

```python
import matplotlib.pyplot as plt

figure, axis = plt.subplots(
    figsize=(8, 5),
    layout="constrained",
)

axis.plot([1, 2, 3], [0.8, 0.5, 0.3])
axis.set(
    title="Training Loss",
    xlabel="Epoch",
    ylabel="Loss",
)

plt.show()
```

## Kenapa Dibutuhkan?

Visualization membantu:

- memahami distribusi
- menemukan outlier
- memeriksa learning curve
- membandingkan experiment
- melihat prediction dan failure case
- berkomunikasi secara jujur

## Cara Kerja

Plotting method menambahkan Artist ke Axes. Backend merender Figure ke layar atau file.

```text
Data
 ↓ plotting call
Artists pada Axes
 ↓ backend
PNG, SVG, PDF, atau window
```

## Plot Dasar

### Line Plot

```python
axis.plot(epochs, train_loss, label="train")
axis.plot(epochs, val_loss, label="validation")
axis.legend()
```

### Scatter

```python
axis.scatter(widths, heights, alpha=0.4)
```

### Histogram

```python
axis.hist(aspect_ratios, bins=30)
```

Bin choice memengaruhi tampilan distribusi.

### Bar Plot

```python
axis.bar(class_names, class_counts)
axis.tick_params(axis="x", rotation=45)
```

Bar chart dimulai dari zero kecuali ada alasan kuat dan disclosure jelas.

## Image Display

```python
axis.imshow(rgb_image)
axis.axis("off")
```

Grayscale:

```python
axis.imshow(
    gray_image,
    cmap="gray",
    vmin=0,
    vmax=255,
)
```

`vmin` dan `vmax` menjaga mapping intensitas konsisten.

## Image Grid

```python
figure, axes = plt.subplots(
    2,
    4,
    figsize=(12, 6),
    layout="constrained",
)

for axis, image, label in zip(
    axes.flat,
    images,
    labels,
):
    axis.imshow(image)
    axis.set_title(label)
    axis.axis("off")
```

Tangani axes yang tidak terpakai jika jumlah image lebih sedikit.

## Implementasi: Training Curves

```python
def plot_training_history(
    train_loss: list[float],
    val_loss: list[float],
):
    epochs = range(1, len(train_loss) + 1)
    figure, axis = plt.subplots(
        figsize=(8, 5),
        layout="constrained",
    )

    axis.plot(epochs, train_loss, label="train")
    axis.plot(epochs, val_loss, label="validation")
    axis.set(
        xlabel="Epoch",
        ylabel="Loss",
        title="Training History",
    )
    axis.legend()
    axis.grid(alpha=0.2)
    return figure, axis
```

Returning object membuat function lebih testable dan composable.

## Saving

```python
figure.savefig(
    "training-history.png",
    dpi=200,
    bbox_inches="tight",
)
```

Simpan sebelum menutup Figure.

## Studi Kasus: Class Imbalance

Satu bar chart count per class dapat menunjukkan imbalance, tetapi lengkapi dengan:

- total sample
- split
- log scale jika range ekstrem
- percentage jika relevan

Jangan menggabungkan train dan test jika distribusi split perlu dibandingkan.

## Misleading Visualization

- axis terpotong
- cherry-picked range
- colormap tidak sesuai
- warna tidak accessible
- aggregation menyembunyikan variance
- aspect ratio plot mengubah persepsi slope
- image autoscaling berbeda per sample

## Best Practice

- Label axis dan satuan.
- Gunakan title yang menyatakan isi, bukan kesimpulan berlebihan.
- Tampilkan uncertainty.
- Konsisten warna antar-plot.
- Gunakan colormap perceptually appropriate.
- Tutup Figure pada batch generation.
- Simpan source data plot.

## Kesalahan Umum

- Mengandalkan global pyplot state pada project besar.
- `plt.show()` di loop tanpa cleanup.
- BGR ditampilkan sebagai RGB.
- Grayscale tanpa `cmap`.
- Membandingkan image dengan auto-range berbeda.
- Plot cantik tetapi tidak menjawab pertanyaan.

## Debugging

```python
print(image.shape, image.dtype)
print(axis.get_xlim(), axis.get_ylim())
```

Jika blank:

- cek data empty atau NaN
- cek limit
- cek backend
- cek Figure sudah ditutup

Jika warna salah, periksa color convention.

## Ringkasan

- Figure adalah container utama.
- Axes adalah area visualisasi.
- Plot harus jujur, berlabel, dan reproducible.
- Visualization adalah alat diagnosis, bukan pengganti statistik.

## Checklist Pemahaman

- [ ] Bisa membedakan Figure, Axes, dan Axis.
- [ ] Bisa membuat line, scatter, histogram, dan bar.
- [ ] Bisa menampilkan image grid.
- [ ] Bisa menyimpan Figure.
- [ ] Bisa mengenali misleading plot.
- [ ] Bisa menjaga intensity range konsisten.

## Latihan

1. Plot distribusi image width.
2. Buat bar class count per split.
3. Buat image grid delapan sample.
4. Plot train/validation loss dan tulis interpretasi terbatas.

## Referensi

- [Matplotlib Figures](https://matplotlib.org/stable/users/explain/figure/index.html)
- [Matplotlib Axes](https://matplotlib.org/stable/users/explain/axes/index.html)

