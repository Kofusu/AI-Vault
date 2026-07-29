---
type: concept
status: not-started
domain: software-engineering
topic: developer-tools
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# Linux Command Line

## Tujuan

Mampu menavigasi filesystem, menginspeksi file/proses, dan menjalankan workflow AI pada terminal dengan aman.

## Intuisi

Command line adalah antarmuka teks terhadap operating system. Ia penting karena server training, container, cloud VM, dan edge device sering tidak memakai GUI.

## Filesystem

- `/`: root filesystem
- `.`: directory saat ini
- `..`: parent directory
- absolute path dimulai dari `/`
- relative path dihitung dari directory saat ini

## Navigasi dan Inspeksi

```bash
pwd
ls -la
cd path/to/project
find src -type f
rg "DataLoader" src tests
```

## File dan Directory

```bash
mkdir -p data/raw
cp config.example.yaml config.yaml
mv old_name.py new_name.py
```

Perintah hapus bersifat destructive. Pastikan path eksplisit dan periksa target sebelum menjalankannya.

## Membaca File

```bash
less README.md
head -n 20 metrics.csv
tail -f logs/train.log
```

## Pipe dan Redirection

Pipe mengirim output satu proses menjadi input proses lain:

```bash
find . -name "*.py" | sort
```

Redirection menulis output ke file. Hati-hati karena `>` menimpa file.

## Process

```bash
ps aux
top
kill PID
```

Gunakan signal termination normal sebelum force kill.

## Permissions

```text
r = read
w = write
x = execute
```

Jangan menggunakan permission terlalu luas seperti `chmod 777` sebagai solusi default.

## Environment Variables

```bash
export DATASET_ROOT=/data/images
python -c 'import os; print(os.environ["DATASET_ROOT"])'
```

Secret sebaiknya dikelola oleh secret manager atau environment lokal yang tidak masuk Git.

## Best Practice

- Quote path yang mengandung spasi.
- Gunakan tab completion.
- Baca `--help` sebelum opsi yang belum dikenal.
- Jalankan command dari directory yang dipahami.
- Jangan menyalin command destructive tanpa membaca artinya.

## Kesalahan Umum

- Bingung absolute dan relative path.
- Menjalankan command dari directory salah.
- Memasukkan secret ke command history.
- Menggunakan privilege admin tanpa kebutuhan.

## Ringkasan

Linux command line adalah fondasi menjalankan, menginspeksi, dan mengotomasi workload AI pada lingkungan lokal maupun server.

## Hubungan Konsep

- [[Bash Fundamentals]]
- [[Python Virtual Environment]]
- [[Docker]]

## Checklist Pemahaman

- [ ] Bisa menavigasi filesystem
- [ ] Bisa mencari file dan teks
- [ ] Paham pipe serta redirection
- [ ] Bisa menginspeksi proses
- [ ] Paham risiko command destructive

## Latihan

1. Cari seluruh file Python pada project dan urutkan hasilnya.
2. Pantau log training dengan `tail`.
3. Buat environment variable untuk lokasi dataset dan baca dari Python.

