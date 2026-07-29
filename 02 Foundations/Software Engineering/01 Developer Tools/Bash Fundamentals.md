---
type: concept
status: not-started
domain: software-engineering
topic: automation
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# Bash Fundamentals

## Tujuan

Membuat automation script kecil yang aman untuk setup, training, evaluation, dan batch processing.

## Intuisi

Bash menggabungkan command menjadi workflow yang bisa diulang. Ia cocok sebagai orchestration tipis, bukan tempat business logic kompleks.

## Script Dasar

```bash
#!/usr/bin/env bash
set -euo pipefail

project_dir="${1:-.}"
echo "Running project at: ${project_dir}"
python "${project_dir}/src/train.py"
```

`set -euo pipefail` membantu menghentikan script saat command gagal, variable belum diset, atau salah satu bagian pipe gagal.

## Variable

```bash
dataset_root="/data/images"
run_name="baseline"
```

Gunakan `"${variable}"` agar whitespace dan glob tidak menimbulkan behavior tak terduga.

## Argument

```bash
config_path="${1:?config path is required}"
python src/train.py --config "${config_path}"
```

## Condition

```bash
if [[ ! -f "${config_path}" ]]; then
  echo "Config not found: ${config_path}" >&2
  exit 1
fi
```

## Loop

```bash
for seed in 1 2 3; do
  python src/train.py --seed "${seed}"
done
```

## Function

```bash
run_experiment() {
  local config_path="$1"
  python src/train.py --config "${config_path}"
}
```

## Kapan Tidak Memakai Bash

Pindahkan logic ke Python jika membutuhkan:

- struktur data kompleks
- parsing/validation rumit
- test unit yang kaya
- cross-platform behavior
- error handling domain-specific

## Best Practice

- Validasi argument dan file sebelum eksekusi.
- Quote variable.
- Gunakan exit code bermakna.
- Tulis output penting ke log.
- Lint script dengan ShellCheck.

## Kesalahan Umum

- Tidak quote path.
- Mengandalkan current directory tanpa validasi.
- Menyimpan secret langsung dalam script.
- Menjalankan loop training tanpa merekam config/seed.
- Membuat Bash script besar yang sulit diuji.

## Ringkasan

Bash efektif untuk orchestration command sederhana. Jaga script kecil, eksplisit, aman, dan pindahkan logic kompleks ke Python.

## Hubungan Konsep

- [[Linux Command Line]]
- [[Configuration Management for AI]]
- [[AI Project Structure]]

## Checklist Pemahaman

- [ ] Bisa membaca argument
- [ ] Bisa menulis condition dan loop
- [ ] Paham quoting variable
- [ ] Tahu kapan logic perlu dipindah ke Python

## Latihan

1. Buat script yang menjalankan training pada tiga seed.
2. Tambahkan validasi config dan directory output.
3. Rekam command serta timestamp ke file log.

