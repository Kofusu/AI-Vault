---
type: concept
status: not-started
domain: scientific-computing
topic: pillow
level: foundations
order: 6
created: 2026-07-28
---

# Pillow Fundamentals

## Tujuan

- Memahami `Image`, mode, band, size, EXIF orientation, dan lazy decoding.
- Membaca, mengubah, resize, serta menyimpan raster image.
- Mengonversi Pillow ↔ NumPy ↔ PyTorch secara aman.

## Intuisi

Pillow adalah library Python untuk raster image:

```text
Encoded file
   ↓ decoder
Pillow Image
   ↓ convert/resize/crop
Pillow Image
   ↓ encoder
Output file
```

## Konsep Dasar

Attribute penting:

```python
from PIL import Image

with Image.open("image.jpg") as image:
    print(image.format)
    print(image.mode)
    print(image.size)
    print(image.getbands())
```

- `format`: source format
- `mode`: channel dan pixel representation
- `size`: `(width, height)`
- bands: channel names

## Kenapa Dibutuhkan?

Pillow cocok untuk:

- image I/O
- thumbnail
- format conversion
- EXIF handling
- crop/resize
- dataset validation
- integration dengan torchvision

## Cara Kerja Lazy Decoding

`Image.open()` membaca header dan metadata yang diperlukan, tetapi raster dapat baru didecode saat dibutuhkan.

Jika image harus hidup setelah file ditutup:

```python
with Image.open("image.jpg") as source:
    image = source.convert("RGB").copy()
```

## Mode

Mode umum:

```text
1     → bilevel
L     → 8-bit grayscale
RGB   → 3 channel
RGBA  → RGB + alpha
P     → palette
I     → signed integer
F     → float
```

Mode berbeda dari file format. JPEG dapat didecode menjadi mode RGB atau L.

## Coordinate System

Origin di kiri atas. Size memakai `(width, height)`, sedangkan NumPy shape image biasanya `(height, width, channels)`.

Crop box:

```python
left, top, right, bottom = (10, 20, 110, 120)
cropped = image.crop((left, top, right, bottom))
```

Right dan bottom merupakan boundary, bukan pixel terakhir yang ikut.

## EXIF Orientation

Camera dapat menyimpan orientation sebagai metadata tanpa memutar raster.

```python
from PIL import ImageOps

with Image.open("photo.jpg") as source:
    oriented = ImageOps.exif_transpose(source)
    rgb = oriented.convert("RGB")
```

Jika diabaikan, image dapat terlihat benar di viewer tetapi salah di pipeline.

## Resize dan Aspect Ratio

```python
resized = image.resize(
    (224, 224),
    resample=Image.Resampling.BILINEAR,
)
```

Resize langsung dapat mendistorsi aspect ratio. Alternatif:

```python
contained = ImageOps.contain(image, (224, 224))
fitted = ImageOps.fit(image, (224, 224))
padded = ImageOps.pad(image, (224, 224))
```

Pilihan harus sesuai training convention.

## `thumbnail()`

```python
thumbnail = image.copy()
thumbnail.thumbnail((224, 224))
```

`thumbnail()` mengubah object in-place dan mempertahankan aspect ratio dalam bounding size.

## Pillow ke NumPy

```python
import numpy as np

rgb = image.convert("RGB")
array = np.asarray(rgb)

print(array.shape)  # H, W, C
print(array.dtype)
```

Gunakan copy jika data harus writable atau ownership terpisah:

```python
array = np.array(rgb, copy=True)
```

## NumPy ke Pillow

```python
array = np.zeros((224, 224, 3), dtype=np.uint8)
image = Image.fromarray(array, mode="RGB")
```

Pastikan dtype dan shape sesuai mode.

## Implementasi: Safe Loader

```python
from pathlib import Path
from PIL import Image, ImageOps


def load_rgb(path: Path) -> Image.Image:
    with Image.open(path) as source:
        oriented = ImageOps.exif_transpose(source)
        return oriented.convert("RGB").copy()
```

## Saving dan Quality

```python
image.save(
    "output.jpg",
    quality=90,
    optimize=True,
)
```

JPEG bersifat lossy dan tidak mendukung alpha. PNG cocok untuk lossless raster dan transparency. Re-encoding berulang dapat menurunkan kualitas.

## Studi Kasus: Dataset Thumbnail

Pipeline:

```text
validate file size
  ↓ open
EXIF transpose
  ↓ convert RGB
resize dengan policy konsisten
  ↓ strip metadata bila perlu
save ke output terpisah
```

## Keamanan

Image input tidak terpercaya dapat:

- sangat besar setelah decode
- membawa metadata berbahaya
- mengeksploitasi decoder yang usang

Batasi ukuran file/pixel, update dependency, batasi format, sanitasi metadata, dan proses di boundary aman.

## Best Practice

- Gunakan context manager.
- Terapkan EXIF orientation.
- Convert mode eksplisit.
- Dokumentasikan resize policy.
- Jangan overwrite raw dataset.
- Validasi pixel count.
- Perlakukan metadata sebagai untrusted.

## Kesalahan Umum

- Tertukar `size` WH dan NumPy shape HWC.
- Lupa EXIF orientation.
- Menyimpan RGBA ke JPEG.
- Distorsi aspect ratio.
- Mengira `thumbnail()` menghasilkan object baru.
- Menahan lazy image setelah file ditutup.

## Debugging

```python
print(image.format, image.mode, image.size)
print(image.getbands())
print(image.getexif())
```

Gunakan `image.verify()` untuk basic integrity check, lalu buka ulang untuk decode/use.

## Ringkasan

- Pillow `Image` mempunyai mode, size, bands, dan metadata.
- Decode dapat lazy.
- EXIF orientation dan resize policy penting.
- Interoperability membutuhkan perhatian pada shape serta dtype.

## Checklist Pemahaman

- [ ] Bisa membaca mode, size, dan bands.
- [ ] Bisa menjelaskan lazy decoding.
- [ ] Bisa menerapkan EXIF orientation.
- [ ] Bisa memilih resize policy.
- [ ] Bisa mengonversi Pillow dan NumPy.
- [ ] Bisa menjelaskan keamanan image input.

## Latihan

1. Buat loader RGB yang menangani EXIF.
2. Bandingkan contain, fit, dan pad.
3. Buat thumbnail dataset tanpa overwrite raw image.
4. Simpan manifest mode dan size.

## Referensi

- [Pillow Concepts](https://pillow.readthedocs.io/en/stable/handbook/concepts.html)
- [Pillow Tutorial](https://pillow.readthedocs.io/en/stable/handbook/tutorial.html)
- [Pillow Security](https://pillow.readthedocs.io/en/stable/handbook/security.html)

