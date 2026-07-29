---
type: concept
status: not-started
domain: software-engineering
topic: testing
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# Testing AI Code with Pytest

## Tujuan

Menguji deterministic behavior pada data pipeline, metric, model interface, dan edge case tanpa bergantung pada full training.

## Testing Pyramid untuk AI

```text
sedikit end-to-end tests
      integration tests
   banyak unit tests cepat
```

Training penuh lambat dan stochastic. Mayoritas test harus memeriksa contract kecil.

## Arrange Act Assert

```python
import numpy as np

def test_normalize_uint8_image() -> None:
    image = np.array([0, 255], dtype=np.uint8)

    result = normalize_image(image)

    np.testing.assert_allclose(result, [0.0, 1.0])
    assert result.dtype == np.float32
```

## Fixture

```python
import pytest

@pytest.fixture
def tiny_batch():
    return {
        "images": np.zeros((2, 32, 32, 3), dtype=np.uint8),
        "labels": np.array([0, 1]),
    }
```

## Parameterized Test

```python
@pytest.mark.parametrize("height,width", [(1, 1), (32, 32), (48, 64)])
def test_resize_shape(height: int, width: int) -> None:
    ...
```

## Apa yang Perlu Diuji?

- shape dan dtype
- range output
- split tidak overlap
- mapping class konsisten
- metric pada contoh manual
- save/load round trip
- error untuk input invalid
- inference pada tiny batch

## Floating Point

Gunakan approximate comparison:

```python
assert score == pytest.approx(0.75, abs=1e-6)
```

## Test Reproducibility

Seed dapat membuat run lebih repeatable, tetapi determinism juga bergantung hardware dan library. Test invariant yang penting daripada exact weight full model.

## Integration Test

Jalankan pipeline kecil pada synthetic dataset:

```text
load → preprocess → train 1 epoch → predict → metric
```

Tujuannya memverifikasi komponen tersambung, bukan mengejar akurasi.

## Best Practice

- Test cepat dan isolated.
- Gunakan tiny/synthetic data.
- Lindungi bug dengan regression test.
- Jalankan test di CI.
- Pisahkan slow test dengan marker.

## Kesalahan Umum

- Menguji implementation detail.
- Membutuhkan dataset besar.
- Menetapkan exact floating-point output tanpa tolerance.
- Menganggap seed menjamin determinism universal.
- Hanya menguji happy path.

## Ringkasan

Testing AI berfokus pada data contract, transformasi, metric, interface, dan integrasi kecil; bukan mengulang full training setiap saat.

## Hubungan Konsep

- [[Python Exception Handling]]
- [[Clean Code for AI]]
- [[Git and GitHub Fundamentals]]

## Checklist Pemahaman

- [ ] Bisa menulis unit test pytest
- [ ] Bisa memakai fixture dan parameterization
- [ ] Bisa menguji floating point
- [ ] Bisa merancang tiny integration test

## Latihan

1. Test normalisasi pixel.
2. Test bahwa train/validation indices tidak overlap.
3. Test precision dan recall pada confusion matrix kecil.

