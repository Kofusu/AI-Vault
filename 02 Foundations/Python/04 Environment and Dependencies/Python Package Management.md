---
type: concept
status: not-started
domain: python
topic: environment
level: foundations
order: 10
created: 2026-07-28
---

# Python Package Management

## Tujuan

- Memahami package, dependency, version, dan package index.
- Menggunakan pip atau Conda secara disiplin.
- Membuat setup project lebih reproducible.

## Intuisi

Package manager mengambil package beserta dependency-nya dan memasangnya ke environment.

```text
Project requirement
        ↓
Package resolver
        ↓
Package index atau channel
        ↓
Installed environment
```

## Konsep Dasar

- **Package:** unit distribusi kode.
- **Dependency:** package lain yang dibutuhkan.
- **Direct dependency:** dipilih project.
- **Transitive dependency:** dibutuhkan dependency lain.
- **Resolver:** mencari kombinasi version yang kompatibel.
- **Index/channel:** sumber package.
- **Lock:** hasil resolution yang dibuat reproducible oleh tooling tertentu.

## Kenapa Dibutuhkan?

Tanpa dependency management, project sulit dipasang ulang, hasil eksperimen berubah diam-diam, dan production dapat memakai stack berbeda dari development.

## Cara Kerja Resolution

```text
Project constraints
      ↓
Resolver membaca dependency graph
      ↓
Pilih versions yang kompatibel
      ↓
Download wheel/source
      ↓
Install ke environment
```

Installation dari source dapat membutuhkan compiler dan system library.

## pip

Install:

```bash
python -m pip install pillow
```

Inspect:

```bash
python -m pip show pillow
python -m pip list
```

Uninstall:

```bash
python -m pip uninstall pillow
```

## Version Specifier

```text
package==1.2.3    exact version
package>=1.2      minimum version
package~=1.2.0    compatible release
```

Pinning meningkatkan repeatability, tetapi semua dependency yang dipin tanpa strategi update dapat menyulitkan maintenance.

## Requirements File

```text
numpy
pillow
opencv-python
```

Install:

```bash
python -m pip install -r requirements.txt
```

`pip freeze` merekam seluruh environment, termasuk transitive dependency. Hasilnya berguna untuk snapshot, tetapi belum tentu menjadi specification project yang paling bersih.

## `pyproject.toml`

Project modern dapat mendeklarasikan metadata dan dependency:

```toml
[project]
name = "vision-project"
version = "0.1.0"
dependencies = [
  "pillow",
]
```

Exact format untuk lock dan development dependency bergantung tooling yang dipilih. Jangan mencampur beberapa source of truth.

## Conda

```bash
conda install numpy
conda list
conda env export
```

Conda berguna saat project membutuhkan package native atau stack scientific tertentu.

## pip dan Conda Bersama

Jika harus dicampur:

1. Buat Conda environment.
2. Install dependency Conda terlebih dahulu.
3. Install package yang tidak tersedia menggunakan pip.
4. Catat prosesnya.

Perubahan lanjutan melalui Conda dapat mengganti dependency yang sebelumnya dipasang pip.

## Dependency Conflict

Conflict terjadi ketika dua package membutuhkan versi dependency yang tidak kompatibel.

Langkah diagnosis:

- baca pesan resolver
- cek versi Python
- cek package yang menyebabkan constraint
- mulai dari environment bersih
- hindari upgrade acak

## Implementasi Workflow Reproducible

```text
1. Buat environment bersih
2. Install dari specification
3. Jalankan test
4. Rekam Python dan system requirements
5. Uji ulang pada environment baru
```

```bash
python -m pip check
```

`pip check` mendeteksi sebagian incompatibility package yang sudah terpasang.

## Reproducibility

Catat:

- versi Python
- direct dependency
- lock atau environment file jika digunakan
- CUDA dan platform requirement
- langkah installation

## Keamanan

- Periksa nama package untuk mencegah typo-squatting.
- Utamakan source resmi.
- Jangan mengeksekusi installation command acak tanpa memahami isinya.
- Audit dependency untuk project production.

## Studi Kasus: PyTorch dan CUDA

Package Python, CUDA runtime yang dibundel atau dibutuhkan, GPU driver, dan OS compatibility harus dipahami sebagai stack. “PyTorch sudah ter-install” belum menjamin GPU tersedia.

Verifikasi runtime:

```python
import torch

print(torch.__version__)
print(torch.cuda.is_available())
print(torch.version.cuda)
```

## Best Practice Tambahan

- Miliki satu source of truth dependency.
- Pisahkan direct dependency dari snapshot jika tooling mendukung.
- Review dependency update.
- Jalankan tests setelah upgrade.
- Catat platform-specific installation.
- Hindari dependency yang tidak perlu.

## Best Practice

- Install di [[Python Virtual Environment]].
- Gunakan `python -m pip`.
- Pisahkan runtime dan development dependency bila project membesar.
- Commit dependency specification.
- Uji installation dari environment kosong.

## Kesalahan Umum

- Menggunakan pip global.
- Tidak mencatat versi Python.
- Mencampur resolver tanpa dokumentasi.
- Menjalankan `pip freeze` lalu menganggap semuanya direct dependency.
- Upgrade package di tengah eksperimen tanpa mencatat perubahan.

## Debugging

- Baca resolver conflict dari package paling spesifik.
- Cek Python version constraint.
- Jalankan `python -m pip check`.
- Reproduce di environment bersih.
- Inspect `python -m pip show package-name`.
- Untuk GPU, pisahkan diagnosis package, CUDA runtime, dan driver.

## Checklist Pemahaman

- [ ] Bisa membedakan direct dan transitive dependency.
- [ ] Bisa menjelaskan resolver.
- [ ] Bisa memakai requirements atau `pyproject.toml`.
- [ ] Bisa menguji installation dari environment kosong.
- [ ] Bisa mendiagnosis conflict dasar.
- [ ] Bisa menjelaskan stack PyTorch–CUDA–driver.

## Ringkasan

- Package manager mengelola installation dan dependency.
- Environment terisolasi dan specification diperlukan untuk reproducibility.
- pip dan Conda punya scope berbeda dan harus dipakai dengan sadar.

## Hubungan Konsep

- Prasyarat: [[Python Virtual Environment]]
- Parent: [[Python MOC]]
- Terkait: [[Reproducibility]], [[Docker]], [[MLOps MOC]]

## Latihan

Buat environment baru, install satu package, simpan dependency specification, lalu buktikan environment dapat dibuat ulang.

2. Jalankan `pip check`.
3. Tambahkan dependency ke `pyproject.toml`.
4. Dokumentasikan langkah setup untuk device tanpa GPU.
