---
type: concept
status: not-started
domain: python
topic: reliability
level: foundations
order: 8
created: 2026-07-28
---

# Python File Handling

## Tujuan

- Membaca dan menulis file dengan aman.
- Menggunakan `pathlib` dan context manager.
- Mengenal text, JSON, CSV, binary file, dan directory traversal.

## Intuisi

File handling menghubungkan program dengan data persisten:

```text
Disk → read → Python object → process → write → Disk
```

## Konsep Dasar

File adalah sequence bytes. Text file menafsirkan bytes menggunakan encoding. Format seperti JSON, JPEG, atau checkpoint menentukan bagaimana bytes disusun.

## Kenapa Dibutuhkan?

Project AI harus membaca dataset, config, checkpoint, dan annotation; lalu menulis metric, prediction, artifact, serta report. File I/O yang salah dapat merusak data atau reproducibility.

## Cara Kerja I/O

```text
Path
  ↓ open dengan mode
File object / stream
  ↓ read atau write
Buffer
  ↓ encode/decode atau parse
Python object
```

Context manager memastikan cleanup ketika normal maupun exception.

## Path dengan `pathlib`

```python
from pathlib import Path

dataset_root = Path("data")
image_path = dataset_root / "images" / "cat.jpg"

print(image_path.exists())
print(image_path.suffix)
print(image_path.name)
```

`pathlib` lebih jelas dan portable daripada menyusun path dengan string manual.

## Membaca dan Menulis Text

```python
from pathlib import Path

path = Path("notes.txt")
path.write_text("experiment completed\n", encoding="utf-8")
content = path.read_text(encoding="utf-8")
```

Untuk file besar, baca secara streaming:

```python
with path.open("r", encoding="utf-8") as file:
    for line in file:
        print(line.rstrip())
```

## Mode File

```text
r  → read
w  → write dan truncate
a  → append
x  → create, gagal jika sudah ada
b  → binary mode
```

> [!warning]
> Mode `w` menimpa isi file yang sudah ada.

## JSON

```python
import json
from pathlib import Path

config = {
    "batch_size": 32,
    "learning_rate": 0.001,
}

path = Path("config.json")

with path.open("w", encoding="utf-8") as file:
    json.dump(config, file, indent=2)

with path.open("r", encoding="utf-8") as file:
    loaded_config = json.load(file)
```

## CSV

```python
import csv

with open("metrics.csv", "w", newline="", encoding="utf-8") as file:
    writer = csv.DictWriter(
        file,
        fieldnames=["epoch", "loss"],
    )
    writer.writeheader()
    writer.writerow({"epoch": 1, "loss": 0.42})
```

## Binary File

Gambar, checkpoint, dan compressed data dibaca sebagai bytes:

```python
image_bytes = Path("cat.jpg").read_bytes()
```

Gunakan library format terkait untuk interpretasi isi file.

## Implementasi: Dataset Manifest

```python
import json
from pathlib import Path


def build_manifest(root: Path) -> list[dict[str, str]]:
    extensions = {".jpg", ".jpeg", ".png"}
    return [
        {"path": path.relative_to(root).as_posix()}
        for path in sorted(root.rglob("*"))
        if path.is_file()
        and path.suffix.lower() in extensions
    ]


def save_manifest(
    records: list[dict[str, str]],
    output_path: Path,
) -> None:
    output_path.parent.mkdir(parents=True, exist_ok=True)
    output_path.write_text(
        json.dumps(records, indent=2),
        encoding="utf-8",
    )
```

## Menjelajah Directory

```python
image_paths = sorted(
    path
    for path in Path("data/images").rglob("*")
    if path.suffix.lower() in {".jpg", ".jpeg", ".png"}
)
```

## Atomic Write

Untuk file penting, tulis ke temporary file lalu rename agar file lama tidak rusak jika proses gagal di tengah jalan.

```python
from pathlib import Path
import tempfile


def atomic_write_text(path: Path, content: str) -> None:
    path.parent.mkdir(parents=True, exist_ok=True)

    with tempfile.NamedTemporaryFile(
        mode="w",
        encoding="utf-8",
        dir=path.parent,
        delete=False,
    ) as temporary:
        temporary.write(content)
        temporary_path = Path(temporary.name)

    temporary_path.replace(path)
```

## Keamanan

- Jangan percaya extension saja; validasi isi file.
- Jangan gabungkan user input ke path tanpa validasi.
- Waspadai path traversal seperti `../../secret`.
- Jangan memuat file `pickle` dari sumber tidak terpercaya karena dapat mengeksekusi kode.

## Relevansi untuk AI/CV

- Membaca annotation dan metadata.
- Menyimpan config dan metric.
- Menemukan image paths.
- Mengelola checkpoint.
- Menulis prediction results.

## Studi Kasus: Checkpoint

Checkpoint ideal menyimpan:

- model state
- optimizer state
- scheduler state
- epoch atau step
- config reference
- metric
- code/version identifier jika tersedia

Jangan load serialized object dari sumber tidak dipercaya. Untuk portability, dokumentasikan framework dan version.

## Best Practice

- Gunakan `pathlib`.
- Tetapkan encoding text.
- Pakai context manager.
- Validasi schema setelah parsing.
- Gunakan checksum untuk artifact penting.
- Tulis secara atomic.
- Simpan relative path dalam manifest jika memungkinkan.

## Kesalahan Umum

- Lupa encoding.
- Membuka file tanpa context manager.
- Menimpa file dengan mode `w`.
- Memuat seluruh file besar ke memori.
- Mengandalkan relative path tanpa memahami working directory.
- Memercayai file serialized dari sumber tidak dikenal.

## Debugging

- Print `Path.cwd()` untuk memahami relative path.
- Gunakan `path.resolve()` untuk diagnosis, bukan selalu disimpan permanen.
- Cek permission dan free disk space.
- Bedakan parse error dari I/O error.
- Jika file kosong atau parsial, periksa write interruption dan flush.
- Validasi beberapa sample secara visual setelah loading.

## Checklist Pemahaman

- [ ] Bisa membedakan bytes, text, encoding, dan format.
- [ ] Bisa memakai `pathlib` dan context manager.
- [ ] Bisa membaca/menulis JSON serta CSV.
- [ ] Bisa menjelaskan atomic write.
- [ ] Bisa membuat dataset manifest.
- [ ] Paham risiko untrusted serialization.

## Ringkasan

- `pathlib` adalah pilihan utama untuk path.
- Context manager memastikan resource ditutup.
- Pilih format dan mode berdasarkan jenis data.
- Validasi file dari sumber luar.

## Hubungan Konsep

- Prasyarat: [[Python Exception Handling]]
- Parent: [[Python MOC]]
- Lanjutan: [[Python Logging]]
- Digunakan di: [[Dataset]], [[Checkpoint]], [[Experiment]]

## Latihan

Buat script yang mencari semua gambar secara recursive dan menyimpan daftar relative path ke JSON.

2. Tambahkan ukuran file dan checksum.
3. Tulis manifest secara atomic.
4. Validasi ulang bahwa seluruh path pada manifest benar-benar ada.
