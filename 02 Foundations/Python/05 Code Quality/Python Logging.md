---
type: concept
status: not-started
domain: python
topic: code-quality
level: foundations
order: 12
created: 2026-07-28
---

# Python Logging

## Tujuan

- Merekam kejadian program secara terstruktur.
- Memilih log level yang tepat.
- Menghindari penggunaan `print()` sebagai sistem observability.

## Intuisi

Logging adalah rekam jejak program:

```text
Program event
    ↓
Logger
    ↓
Formatter dan Handler
    ↓
Console atau File
```

## Konsep Dasar

Komponen logging:

- **Logger:** menerima event.
- **Level:** severity.
- **LogRecord:** data event.
- **Handler:** tujuan output.
- **Formatter:** bentuk message.
- **Filter:** aturan tambahan.

## Kenapa Dibutuhkan?

Program AI berjalan lama dan sering jauh dari terminal developer. Logging membantu:

- mengetahui progress
- mereproduksi failure
- mengaudit input/config
- memonitor latency
- menemukan data bermasalah
- menghubungkan event dengan experiment atau request

## Cara Kerja

```text
logger.info(...)
      ↓ create LogRecord
Level/filter check
      ↓
One or more handlers
      ↓ formatter
Console, file, atau collector
```

## Kenapa Bukan Hanya `print()`?

Logging mendukung:

- severity level
- timestamp
- module name
- output ke beberapa tujuan
- filtering
- integrasi monitoring

## Log Level

```text
DEBUG    detail untuk diagnosis
INFO     alur normal penting
WARNING  kondisi tidak ideal tetapi proses lanjut
ERROR    operasi gagal
CRITICAL kegagalan sistem serius
```

## Setup Dasar

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s | %(levelname)s | %(name)s | %(message)s",
)

logger = logging.getLogger(__name__)

logger.info("Training started")
```

## Implementasi Konfigurasi

```python
import logging
from pathlib import Path


def configure_logging(log_path: Path | None = None) -> None:
    handlers: list[logging.Handler] = [
        logging.StreamHandler()
    ]

    if log_path is not None:
        log_path.parent.mkdir(parents=True, exist_ok=True)
        handlers.append(logging.FileHandler(log_path))

    logging.basicConfig(
        level=logging.INFO,
        format=(
            "%(asctime)s | %(levelname)s | "
            "%(name)s | %(message)s"
        ),
        handlers=handlers,
        force=True,
    )
```

`force=True` berguna pada application entry point tertentu, tetapi library tidak boleh mengubah global configuration pengguna.

## Logging Exception

```python
try:
    load_dataset()
except OSError:
    logger.exception("Failed to load dataset")
    raise
```

`logger.exception()` menyertakan traceback dan dipakai di dalam exception handler.

## Parameterized Logging

Gunakan:

```python
logger.info("Epoch %d | loss %.4f", epoch, loss)
```

Daripada:

```python
logger.info(f"Epoch {epoch} | loss {loss:.4f}")
```

Parameterized logging menunda formatting sampai message benar-benar diproses.

## Logging Training

```python
logger.info(
    "epoch=%d train_loss=%.4f val_accuracy=%.4f",
    epoch,
    train_loss,
    val_accuracy,
)
```

Log application berbeda dari experiment tracker. W&B, MLflow, atau TensorBoard cocok untuk metric, artifact, config, dan visualisasi eksperimen.

## Structured Logging

Machine-readable field lebih mudah dicari:

```text
event=train_epoch epoch=5 loss=0.231 val_accuracy=0.914
```

Pada sistem besar, JSON logging dan correlation ID membantu menghubungkan event lintas component.

## Studi Kasus: Inference Service

Log yang relevan:

- request ID
- model version
- input metadata aman
- latency
- prediction summary
- error category

Jangan log image mentah atau personal data tanpa dasar, consent, dan policy.

## Logging vs Metric vs Trace

- Log: event diskrit dengan context.
- Metric: angka agregat dari waktu ke waktu.
- Trace: perjalanan satu request melewati component.

Ketiganya saling melengkapi.

## Informasi Sensitif

Jangan log:

- password
- API key
- access token
- data pribadi
- full user input sensitif

Redact jika informasi dibutuhkan untuk diagnosis.

## Best Practice

- Gunakan `logging.getLogger(__name__)`.
- Library tidak seharusnya mengatur global logging secara paksa.
- Pilih level secara konsisten.
- Sertakan context seperti experiment ID atau dataset version.
- Gunakan log rotation untuk file jangka panjang.
- Jangan mencetak setiap batch pada skala besar tanpa kebutuhan.

## Kesalahan Umum

- Semua message memakai `INFO`.
- Logging terlalu banyak di hot loop.
- Menulis secret ke log.
- Menangkap exception, log, lalu diam tanpa keputusan recovery.
- Logging error yang sama berkali-kali pada layer berbeda.

## Debugging

Log tidak muncul:

- cek logger level
- cek handler level
- cek propagation
- cek apakah configuration sudah dijalankan
- cek notebook yang sudah memiliki handler

Log duplikat:

- handler terpasang beberapa kali
- child logger propagate ke root
- configuration dipanggil berulang

## Checklist Pemahaman

- [ ] Bisa menjelaskan logger, handler, formatter, dan level.
- [ ] Bisa memilih level yang tepat.
- [ ] Bisa log exception dengan traceback.
- [ ] Bisa menghindari secret dan personal data.
- [ ] Bisa membedakan log, metric, dan trace.
- [ ] Bisa mendiagnosis log hilang atau duplikat.

## Ringkasan

- Logging menyediakan observability yang dapat dikonfigurasi.
- Level menunjukkan severity.
- Traceback dan context membuat error lebih mudah direproduksi.
- Logging melengkapi, bukan menggantikan experiment tracking.

## Hubungan Konsep

- Prasyarat: [[Python Exception Handling]], [[Python File Handling]], [[Python Type Hints]]
- Parent: [[Python MOC]]
- Digunakan di: [[Training Pipeline]], [[Experiment]], [[MLOps MOC]], [[Deployment MOC]]

## Latihan

Tambahkan logging pada image organizer: jumlah file ditemukan, file invalid, dan lokasi output.

2. Tambahkan file handler.
3. Sertakan run ID pada setiap message.
4. Buat satu error dan pastikan traceback tercatat tepat sekali.
