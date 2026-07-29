---
type: concept
status: not-started
domain: software-engineering
topic: project-architecture
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# AI Project Structure

## Tujuan

Memisahkan data, configuration, source code, tests, artifacts, dan entry point agar project mudah dipahami serta direproduksi.

## Intuisi

Notebook bagus untuk eksplorasi. Saat workflow mulai stabil, logic reusable harus pindah ke module yang dapat di-import dan diuji.

## Struktur Rekomendasi

```text
project/
├── README.md
├── pyproject.toml
├── .gitignore
├── configs/
│   ├── train.yaml
│   └── evaluate.yaml
├── data/
│   ├── raw/
│   ├── interim/
│   └── processed/
├── notebooks/
├── src/
│   └── project_name/
│       ├── __init__.py
│       ├── data.py
│       ├── features.py
│       ├── model.py
│       ├── train.py
│       └── evaluate.py
├── tests/
├── artifacts/
└── scripts/
```

## Tanggung Jawab

- `configs/`: parameter yang berubah antar-run
- `data/`: data berdasarkan tahap pemrosesan
- `notebooks/`: eksplorasi, bukan source of truth produksi
- `src/`: reusable application code
- `tests/`: verifikasi behavior
- `artifacts/`: model, plot, metrics, prediction
- `scripts/`: entry point automation tipis

## Data Flow

```text
raw data
  ↓ validation
interim data
  ↓ preprocessing
processed data
  ↓ training
model artifact
  ↓ evaluation
metrics + report
```

## Entry Point

```python
def main() -> None:
    config = load_config()
    dataset = build_dataset(config)
    model = train(dataset, config)
    save_artifacts(model, config)


if __name__ == "__main__":
    main()
```

## Dependency Direction

Domain logic sebaiknya tidak bergantung pada notebook atau CLI. Entry point memanggil module, bukan sebaliknya.

## Reproducibility

Setiap run idealnya merekam:

- config final
- seed
- dependency version
- Git commit
- dataset version
- metrics
- artifact path

## Best Practice

- Gunakan nama package unik dan jelas.
- Pisahkan training dari evaluation.
- Hindari path absolut dalam source code.
- Jangan membuat satu `utils.py` raksasa.
- Simpan README dengan cara setup dan run.

## Kesalahan Umum

- Semua logic berada di notebook.
- Data, model, dan source code bercampur.
- Global state tersembunyi.
- Function melakukan loading, training, plotting, dan saving sekaligus.

## Ringkasan

Project structure membuat batas tanggung jawab dan data flow eksplisit sehingga workflow AI lebih mudah diuji, direproduksi, dan dikembangkan.

## Hubungan Konsep

- [[Clean Code for AI]]
- [[Testing AI Code with Pytest]]
- [[Configuration Management for AI]]
- [[Python Packaging]]

## Checklist Pemahaman

- [ ] Bisa menjelaskan fungsi setiap directory
- [ ] Bisa memindahkan logic notebook ke module
- [ ] Bisa menggambar data flow
- [ ] Tahu metadata reproducibility yang perlu direkam

## Latihan

1. Refactor notebook sederhana ke struktur `src/`.
2. Pisahkan load, preprocess, train, dan evaluate.
3. Tambahkan README berisi setup serta command.

