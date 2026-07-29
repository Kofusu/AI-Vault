---
type: project
status: idea
domain:
  - python
  - software-engineering
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# PRJ - Command-Line Image Organizer

## Problem Statement

Folder dataset gambar sering bercampur format, file rusak, nama duplikat, dan struktur tidak konsisten.

## Tujuan

Membuat CLI yang menginspeksi dan mengorganisasi gambar secara aman sambil menerapkan Python modular, logging, exception handling, testing, dan packaging.

## Functional Requirements

- scan directory secara recursive
- validasi format gambar
- baca width, height, mode, dan ukuran file
- deteksi file rusak
- kelompokkan berdasarkan extension atau resolusi
- dry-run sebelum memindahkan file
- output manifest CSV/JSON
- logging terstruktur

## Safety Requirements

- Default tidak mengubah file.
- Collision tidak boleh menimpa file diam-diam.
- Source dan destination harus divalidasi.
- Operasi material memiliki manifest untuk audit.

## CLI

```text
image-organizer scan INPUT --manifest report.csv
image-organizer organize INPUT OUTPUT --by extension --dry-run
image-organizer organize INPUT OUTPUT --by resolution --apply
```

## Struktur Project

```text
image-organizer/
├── pyproject.toml
├── README.md
├── src/image_organizer/
│   ├── cli.py
│   ├── scanner.py
│   ├── validator.py
│   ├── organizer.py
│   └── models.py
└── tests/
```

## Milestone

- [ ] M1 — scanner dan metadata
- [ ] M2 — validation serta error report
- [ ] M3 — planning dry-run
- [ ] M4 — apply dengan collision handling
- [ ] M5 — CLI dan logging
- [ ] M6 — tests serta packaging

## Acceptance Criteria

- File valid/rusak terklasifikasi benar pada fixture.
- Dry-run tidak mengubah filesystem.
- Collision menghasilkan nama aman atau error eksplisit.
- Unsupported format tidak menghentikan seluruh scan.
- CLI memiliki help text dan exit code benar.
- Unit serta integration test lulus.

## Error Cases

- permission denied
- symlink loop
- truncated image
- nama file sama
- destination berada di dalam source
- file berubah saat scan

## Deliverable

- installable package
- README setup dan usage
- sample manifest
- test report

## Materi Terkait

- [[Python Functions]]
- [[Object-Oriented Programming in Python]]
- [[Python Exception Handling]]
- [[Python File Handling]]
- [[Python Logging]]
- [[Python Type Hints]]
- [[Python Packaging]]
- [[Testing AI Code with Pytest]]

## Next Action

- [ ] Definisikan data model metadata image dan test fixture.

