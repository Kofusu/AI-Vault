---
type: concept
status: not-started
domain: mathematics
topic: probability-statistics
level: intermediate
order: 3
created: 2026-07-28
---

# Bayes Theorem

## Tujuan

- Memahami cara memperbarui keyakinan dengan evidence baru.
- Membedakan prior, likelihood, evidence, dan posterior.
- Menghindari base-rate fallacy.

## Intuisi

Bayes Theorem menjawab:

> Setelah melihat bukti baru, seberapa besar keyakinan kita terhadap suatu hipotesis?

## Konsep Dasar

Bayes berasal dari dua penulisan joint probability:

$$
P(H,E)=P(E|H)P(H)=P(H|E)P(E)
$$

## Kenapa Dibutuhkan?

Evidence yang sama dapat bermakna berbeda tergantung base rate. Bayes memasukkan keyakinan awal, kualitas evidence, alternatif hypothesis, dan normalization.

## Cara Kerja

```text
1. Tentukan hypothesis H
2. Tentukan prior P(H)
3. Tentukan likelihood P(E|H)
4. Hitung evidence P(E)
5. Normalisasi
6. Dapatkan posterior P(H|E)
```

## Rumus

$$
P(H\mid E)
=
\frac{P(E\mid H)P(H)}{P(E)}
$$

- $P(H)$: prior, keyakinan sebelum melihat evidence.
- $P(E|H)$: likelihood, peluang evidence jika hipotesis benar.
- $P(E)$: evidence atau normalization constant.
- $P(H|E)$: posterior, keyakinan setelah melihat evidence.

## Contoh Manual

Misalkan:

- 1% produk benar-benar cacat.
- Detector menemukan 90% produk cacat.
- Detector salah menandai 5% produk normal sebagai cacat.

$$
P(D)=0.01
$$

$$
P(+|D)=0.90
$$

$$
P(+|\neg D)=0.05
$$

Evidence:

$$
P(+)=0.90(0.01)+0.05(0.99)=0.0585
$$

Posterior:

$$
P(D|+)=\frac{0.90(0.01)}{0.0585}\approx0.154
$$

Walaupun detector cukup sensitif, peluang produk benar-benar cacat setelah hasil positif hanya sekitar 15.4% karena defect sangat jarang.

### Frequency View

Untuk 10.000 produk:

```text
Defect: 100 → positif benar: 90
Normal: 9.900 → positif palsu: 495

Total positif: 585
Defect sebenarnya: 90 / 585 ≈ 15.4%
```

## Implementasi

```python
prior_defect = 0.01
sensitivity = 0.90
false_positive_rate = 0.05

evidence_positive = (
    sensitivity * prior_defect
    + false_positive_rate * (1 - prior_defect)
)

posterior = sensitivity * prior_defect / evidence_positive
print(posterior)
```

## Relevansi untuk AI dan CV

- Sensor fusion dalam robotics.
- Bayesian filtering dan tracking.
- Diagnostic systems.
- Updating uncertainty setelah observasi baru.
- Naive Bayes classification.

## Studi Kasus: Object Tracking

```text
Previous state
     ↓ motion prediction
Prior current state
     + new detection likelihood
     ↓ Bayesian update
Posterior current state
```

Kalman filter adalah estimator Bayesian untuk asumsi linear-Gaussian.

## Bayesian Update Berulang

Posterior satu tahap menjadi prior tahap berikutnya:

```text
Prior₀ + evidence₁ → Posterior₁
Posterior₁ + evidence₂ → Posterior₂
```

Asumsi conditional independence antar-evidence harus diperiksa.

## Odds Form

$$
\text{posterior odds}
=
\text{prior odds}
\times
\text{likelihood ratio}
$$

## Best Practice

- Jelaskan sumber prior.
- Lakukan sensitivity analysis terhadap prior.
- Modelkan alternative hypothesis.
- Evaluasi calibration posterior.
- Jangan memaksakan independence assumption.
- Update prior jika deployment domain berubah.

## Debugging dan Sanity Check

- Posterior seluruh hypothesis mutually exclusive harus berjumlah 1.
- Evidence tidak boleh nol.
- Jika likelihood sama untuk semua hypothesis, posterior harus sama dengan prior.
- Uji rumus dengan frequency table.

## Kesalahan Umum

- Menukar $P(H|E)$ dengan $P(E|H)$.
- Mengabaikan prior atau base rate.
- Menganggap output confidence model selalu posterior yang terkalibrasi.
- Menggunakan prior yang tidak sesuai domain deployment.

## Ringkasan

- Bayes menggabungkan prior dengan likelihood untuk menghasilkan posterior.
- Base rate dapat sangat memengaruhi hasil.
- Conditional probability tidak boleh dibalik tanpa perhitungan.

## Checklist Pemahaman

- [ ] Bisa menjelaskan prior, likelihood, evidence, dan posterior.
- [ ] Bisa menurunkan Bayes dari joint probability.
- [ ] Bisa menghitung contoh defect detector.
- [ ] Bisa menjelaskan base-rate fallacy.
- [ ] Bisa menjelaskan update berulang.
- [ ] Bisa memberi contoh pada tracking.

## Hubungan Konsep

- Prasyarat: [[Probability]], [[Statistics]]
- Parent: [[Probability and Statistics MOC]]
- Digunakan di: [[Object Tracking]], [[Sensor Fusion]], [[Robotics]], [[Uncertainty]]

## Latihan

Jelaskan dengan kata-kata sendiri perbedaan $P(\text{positif}|\text{sakit})$ dan $P(\text{sakit}|\text{positif})$.

2. Ulangi contoh defect jika prior defect menjadi 10%.
3. Apa yang terjadi jika likelihood sama pada semua hypothesis?
4. Kenapa prior satu pabrik belum tentu cocok untuk pabrik lain?
