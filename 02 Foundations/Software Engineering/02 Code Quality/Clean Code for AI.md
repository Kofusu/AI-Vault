---
type: concept
status: not-started
domain: software-engineering
topic: code-quality
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# Clean Code for AI

## Tujuan

Menulis kode AI yang jelas, modular, dapat diuji, dan tidak menyembunyikan eksperimen.

## Prinsip Utama

Clean code bukan kode paling pendek. Tujuannya mengurangi beban memahami intent dan behavior.

## Nama yang Menjelaskan Intent

Buruk:

```python
x2 = f(x)
```

Lebih jelas:

```python
normalized_images = normalize_images(raw_images)
```

## Single Responsibility

Function sebaiknya memiliki satu alasan utama untuk berubah.

```python
def load_image(path: Path) -> np.ndarray:
    ...

def preprocess_image(image: np.ndarray) -> np.ndarray:
    ...

def predict(model, image: np.ndarray) -> np.ndarray:
    ...
```

## Pure Function

Pure function tidak mengubah hidden state dan hasilnya ditentukan input. Ini memudahkan testing preprocessing dan metric.

## Explicit Dependencies

Hindari membaca global config dari dalam fungsi. Pass dependency melalui parameter atau object yang jelas.

## Data Contract

Dokumentasikan:

- shape
- dtype
- range
- coordinate convention
- color order
- unit

```python
def normalize_rgb(image: np.ndarray) -> np.ndarray:
    """Convert uint8 RGB image [H, W, 3] to float32 in [0, 1]."""
    ...
```

## Error Handling

Fail fast pada invalid input dengan pesan yang membantu. Jangan menangkap `Exception` lalu mengabaikannya.

## Duplication

Hilangkan duplication yang merepresentasikan aturan sama. Jangan membuat abstraction prematur untuk dua baris yang kebetulan mirip.

## Notebook Hygiene

- restart dan run all
- hindari ketergantungan execution order tersembunyi
- pindahkan logic stabil ke `src/`
- simpan insight, bukan output raksasa

## Best Practice

- Function kecil tetapi kohesif.
- Type hints pada public API.
- Docstring menjelaskan contract dan keputusan non-obvious.
- Formatter dan linter dijalankan otomatis.
- Refactor hanya setelah behavior terlindungi test.

## Kesalahan Umum

- `utils.py` berisi semua hal.
- Boolean argument ambigu.
- Magic number tanpa nama.
- Mutation input tanpa dokumentasi.
- Over-abstraction dan design pattern berlebihan.

## Ringkasan

Clean code AI menjadikan data contract, dependency, dan transformasi eksplisit. Ini mengurangi bug diam-diam pada shape, dtype, range, dan preprocessing.

## Hubungan Konsep

- [[Python Functions]]
- [[Python Type Hints]]
- [[Testing AI Code with Pytest]]
- [[AI Project Structure]]

## Checklist Pemahaman

- [ ] Bisa memberi nama berdasarkan intent
- [ ] Bisa memisahkan responsibility
- [ ] Bisa mendokumentasikan data contract
- [ ] Bisa mengenali hidden state

## Latihan

1. Refactor function yang melakukan load, resize, normalize, dan save sekaligus.
2. Tambahkan contract shape/dtype pada preprocessing.
3. Hapus global config dari training function.

