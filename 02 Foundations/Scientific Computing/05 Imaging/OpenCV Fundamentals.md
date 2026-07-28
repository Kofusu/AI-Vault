---
type: concept
status: not-started
domain: scientific-computing
topic: opencv
level: foundations
order: 7
created: 2026-07-28
---

# OpenCV Fundamentals

## Tujuan

- Memahami image OpenCV sebagai NumPy array.
- Menguasai image I/O, BGR/RGB, dtype, geometry, drawing, dan video loop dasar.
- Mencegah bug convention antar-library.

## Intuisi

OpenCV menyediakan algorithm Computer Vision berperforma tinggi dengan Python binding:

```text
Python API
   ↓
OpenCV native implementation
   ↓
Image/video processing
```

## Konsep Dasar

```python
import cv2 as cv

image = cv.imread("image.jpg", cv.IMREAD_COLOR)

if image is None:
    raise FileNotFoundError("Could not decode image")

print(image.shape)
print(image.dtype)
```

Color image biasanya:

```text
shape: (height, width, channels)
order: BGR
dtype: uint8
```

## Kenapa Dibutuhkan?

OpenCV digunakan untuk:

- image/video I/O
- color conversion
- geometry
- filtering
- feature extraction
- calibration
- classical CV
- real-time camera pipeline

Bab CV berikutnya akan membahas algoritmanya; lesson ini fokus pada data contract.

## Cara Kerja I/O

`cv.imread()` dapat mengembalikan `None` jika path atau decoding gagal. `cv.imwrite()` mengembalikan status boolean.

```python
success = cv.imwrite("output.png", image)

if not success:
    raise OSError("Failed to write image")
```

## BGR vs RGB

OpenCV color image memakai BGR. Matplotlib dan Pillow umumnya memakai RGB.

```python
rgb = cv.cvtColor(image, cv.COLOR_BGR2RGB)
```

Atau:

```python
rgb = image[..., ::-1].copy()
```

Conversion eksplisit lebih jelas untuk pembaca.

## Grayscale

```python
gray = cv.cvtColor(image, cv.COLOR_BGR2GRAY)
```

Grayscale conversion bukan sekadar mean tiga channel; weighting mengikuti conversion definition.

## Indexing

```python
height, width = image.shape[:2]
pixel_bgr = image[10, 20]
crop = image[50:150, 100:300]
```

NumPy indexing memakai `[y, x]`, bukan `(x, y)`.

## Resize

```python
resized = cv.resize(
    image,
    (224, 224),
    interpolation=cv.INTER_LINEAR,
)
```

Parameter size memakai `(width, height)`.

Untuk downsampling, `INTER_AREA` sering menjadi pilihan awal; untuk upsampling, linear/cubic bergantung quality dan cost. Tetap evaluasi task.

## Drawing

```python
annotated = image.copy()

cv.rectangle(
    annotated,
    (x_min, y_min),
    (x_max, y_max),
    color=(0, 255, 0),
    thickness=2,
)

cv.putText(
    annotated,
    "cat 0.92",
    (x_min, max(0, y_min - 8)),
    cv.FONT_HERSHEY_SIMPLEX,
    0.6,
    (0, 255, 0),
    2,
)
```

Drawing default mengubah array in-place.

## Dtype dan Arithmetic

`uint8` overflow:

```python
value = np.array([250], dtype=np.uint8)
print(value + 10)
```

Untuk arithmetic:

```python
float_image = image.astype(np.float32) / 255.0
```

OpenCV juga menyediakan saturated operations seperti `cv.add`; pahami perbedaannya dari NumPy arithmetic.

## Video Capture

```python
capture = cv.VideoCapture(0)

try:
    if not capture.isOpened():
        raise RuntimeError("Camera cannot be opened")

    while True:
        success, frame = capture.read()
        if not success:
            break

        cv.imshow("camera", frame)

        if cv.waitKey(1) & 0xFF == ord("q"):
            break
finally:
    capture.release()
    cv.destroyAllWindows()
```

GUI behavior bergantung environment; headless server tidak selalu mendukung `imshow`.

## Implementasi: Library Boundary

```python
from PIL import Image
import numpy as np


def bgr_to_pillow(image_bgr: np.ndarray) -> Image.Image:
    image_rgb = cv.cvtColor(
        image_bgr,
        cv.COLOR_BGR2RGB,
    )
    return Image.fromarray(image_rgb)
```

## Studi Kasus: Detection Annotation

Checklist:

- box convention
- clipping ke image boundary
- integer coordinate
- color BGR
- copy sebelum drawing
- label tidak keluar canvas

## Best Practice

- Periksa hasil `imread` dan `imwrite`.
- Dokumentasikan BGR/RGB.
- Pisahkan original dan annotated copy.
- Validate shape, dtype, dan range.
- Release camera/video resource.
- Jangan memakai GUI API di headless pipeline.
- Ukur latency per stage untuk real-time system.

## Kesalahan Umum

- Warna merah dan biru tertukar.
- Size resize WH tertukar dengan shape HW.
- Index coordinate XY tertukar dengan array YX.
- Drawing merusak image original.
- `uint8` overflow.
- Resource video tidak dilepas.
- Menganggap FPS source sama dengan throughput pipeline.

## Debugging

```python
print(image.shape, image.dtype)
print(image.min(), image.max())
print(image[0, 0])
```

Jika warna salah, tampilkan satu patch warna known. Jika video gagal, cek device index, permission, codec, dan `isOpened()`.

## Ringkasan

- OpenCV image Python adalah NumPy array.
- Color default BGR.
- Shape HWC tetapi many API coordinate memakai XY/WH.
- Dtype dan in-place behavior harus dipantau.

## Checklist Pemahaman

- [ ] Bisa memeriksa load/write failure.
- [ ] Bisa mengonversi BGR ↔ RGB.
- [ ] Bisa membedakan shape HW dan coordinate XY.
- [ ] Bisa resize dan crop.
- [ ] Bisa membuat video loop dengan cleanup.
- [ ] Bisa menjelaskan overflow dan in-place drawing.

## Latihan

1. Load, convert RGB, dan tampilkan dengan Matplotlib.
2. Crop area tengah.
3. Gambar bounding box tanpa mengubah original.
4. Catat latency video loop.

## Referensi

- [OpenCV Image Tutorials](https://docs.opencv.org/4.x/d2/d96/tutorial_py_table_of_contents_imgproc.html)
- [OpenCV Color Spaces](https://docs.opencv.org/4.x/df/d9d/tutorial_py_colorspaces.html)

