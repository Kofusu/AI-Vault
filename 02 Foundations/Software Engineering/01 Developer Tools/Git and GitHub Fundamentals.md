---
type: concept
status: not-started
domain: software-engineering
topic: version-control
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# Git and GitHub Fundamentals

## Tujuan

Memahami version control lokal dengan Git dan kolaborasi remote melalui GitHub.

## Intuisi

Git menyimpan riwayat perubahan sebagai rangkaian snapshot. GitHub menyimpan repository remote dan menyediakan collaboration workflow.

```text
Working tree → Staging area → Commit history → Remote
```

## Konsep Dasar

- repository: project yang dilacak
- working tree: file yang sedang diedit
- staging area: perubahan yang dipilih untuk commit berikutnya
- commit: snapshot dengan identitas dan pesan
- branch: jalur perkembangan independen
- remote: repository di server
- merge/rebase: mengintegrasikan riwayat

## Workflow Dasar

```bash
git status
git add path/to/file.py
git commit -m "Add dataset validation"
git push
```

Periksa diff sebelum commit:

```bash
git diff
git diff --staged
```

## Branch Workflow

```bash
git switch -c feature/data-validation
git add src/data.py tests/test_data.py
git commit -m "Validate corrupt image files"
git push -u origin feature/data-validation
```

## Commit yang Baik

- satu tujuan logis
- pesan imperatif dan spesifik
- tidak mencampur refactor dengan perubahan behavior
- tidak menyimpan secret, dataset besar, atau model checkpoint

## Git untuk AI

Track:

- source code
- config
- test
- small metadata
- environment lock file

Jangan track langsung:

- API key
- raw dataset besar
- cache
- model weights besar
- experiment artifacts

Gunakan DVC, artifact store, atau Git LFS bila sesuai.

## `.gitignore`

```gitignore
.venv/
__pycache__/
.env
data/
artifacts/
checkpoints/
*.pyc
```

## GitHub

Workflow umum:

1. push feature branch
2. buka pull request
3. jalankan CI
4. code review
5. merge setelah check lulus

## Best Practice

- Jangan commit secret yang pernah terekspos; rotate secret tersebut.
- Pull/rebase sebelum mengirim perubahan besar.
- Tag release model atau pipeline yang dipakai produksi.
- Hubungkan experiment dengan commit hash.

## Kesalahan Umum

- `git add .` tanpa meninjau perubahan.
- Commit dataset atau `.env`.
- Pesan commit seperti `fix` atau `update`.
- Mengubah banyak hal tidak terkait dalam satu commit.

## Debugging

Gunakan `git status`, `git log --oneline --graph`, dan `git diff` untuk memahami state sebelum mengambil tindakan. Hindari perintah destructive jika belum memahami targetnya.

## Ringkasan

Git menjaga riwayat dan reproducibility kode; GitHub menambahkan remote collaboration, review, issue, dan automation.

## Hubungan Konsep

- [[AI Project Structure]]
- [[Testing AI Code with Pytest]]
- [[End-to-End Machine Learning Baseline]]

## Checklist Pemahaman

- [ ] Paham working tree, staging, dan commit
- [ ] Bisa membuat branch dan pull request
- [ ] Bisa menulis `.gitignore`
- [ ] Tahu file AI yang tidak seharusnya masuk Git

## Latihan

1. Buat repository latihan dengan tiga commit atomik.
2. Buat branch dan gabungkan melalui pull request.
3. Simulasikan conflict kecil lalu selesaikan dengan membaca kedua perubahan.

