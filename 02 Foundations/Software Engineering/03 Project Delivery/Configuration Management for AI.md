---
type: concept
status: not-started
domain: software-engineering
topic: configuration
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# Configuration Management for AI

## Tujuan

Memisahkan parameter eksperimen dari source code dan merekam config final agar run dapat direproduksi.

## Intuisi

Hyperparameter yang tersebar di banyak file sulit dilacak. Configuration menjadi kontrak eksplisit satu run.

## Contoh YAML

```yaml
seed: 42

data:
  root: data/processed
  image_size: 224
  batch_size: 32

model:
  name: resnet18
  pretrained: true
  num_classes: 10

training:
  epochs: 50
  learning_rate: 0.001
  weight_decay: 0.0001
```

## Validation

Config harus divalidasi sebelum training:

- tipe data
- required field
- range nilai
- kombinasi parameter yang valid
- path yang tersedia

Gunakan dataclass, Pydantic, Hydra/OmegaConf, atau schema lain sesuai kompleksitas.

## Structured Config

```python
from dataclasses import dataclass

@dataclass(frozen=True)
class TrainingConfig:
    epochs: int
    learning_rate: float
    batch_size: int

    def __post_init__(self) -> None:
        if self.epochs <= 0:
            raise ValueError("epochs must be positive")
```

## Precedence

Tentukan urutan override yang jelas, misalnya:

```text
default config
  ↓ experiment config
  ↓ environment-specific config
  ↓ explicit CLI override
```

## Secret

API key dan credential bukan config biasa. Simpan di environment variable atau secret manager dan jangan log nilainya.

## Reproducibility

Simpan resolved config bersama:

- Git commit
- dataset version
- dependency version
- seed
- metrics
- artifact

## Best Practice

- Satu sumber kebenaran untuk setiap parameter.
- Config immutable setelah run dimulai.
- Simpan resolved config, bukan hanya default.
- Gunakan nama dan unit eksplisit seperti `learning_rate` atau `timeout_seconds`.

## Kesalahan Umum

- Hardcode path lokal.
- Parameter sama memiliki nilai berbeda di notebook dan source.
- Mengubah config saat training berjalan.
- Menyimpan secret dalam YAML yang di-commit.
- Tidak mencatat default tersembunyi library.

## Ringkasan

Configuration management membuat eksperimen eksplisit, dapat divalidasi, dibandingkan, dan dijalankan ulang.

## Hubungan Konsep

- [[AI Project Structure]]
- [[Bash Fundamentals]]
- [[Experimental Note]]
- [[MLflow]]

## Checklist Pemahaman

- [ ] Bisa memisahkan config dari code
- [ ] Bisa memvalidasi config
- [ ] Paham precedence override
- [ ] Tahu cara menangani secret

## Latihan

1. Pindahkan hyperparameter script ke YAML.
2. Tambahkan validation untuk learning rate dan epoch.
3. Simpan resolved config pada output run.
