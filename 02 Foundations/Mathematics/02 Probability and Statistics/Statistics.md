---
type: concept
status: not-started
domain: mathematics
topic: probability-statistics
level: foundations
order: 2
created: 2026-07-28
---

# Statistics

## Tujuan

- Memahami descriptive dan inferential statistics.
- Menghitung ukuran pusat dan penyebaran.
- Menggunakan statistik untuk audit dataset dan eksperimen.

## Intuisi

Probability bergerak dari model ke kemungkinan data. Statistics bergerak dari data yang diamati untuk menyimpulkan karakteristik populasi.

```text
Population → ambil sample → hitung statistik → buat kesimpulan
```

## Konsep Dasar

- **Population:** seluruh objek target.
- **Sample:** subset yang diamati.
- **Parameter:** karakteristik population, misalnya $\mu$.
- **Statistic:** nilai dari sample, misalnya $\bar{x}$.

## Kenapa Dibutuhkan?

Dataset hanyalah sample dari dunia nyata. Statistics membantu memahami distribusi, mengukur uncertainty eksperimen, mendeteksi bias, membandingkan model secara fair, dan menghindari kesimpulan dari noise.

## Cara Kerja Statistical Reasoning

```text
Pertanyaan
   ↓
Target population
   ↓ sampling process
Observed sample
   ↓ statistic + uncertainty
Kesimpulan terbatas
```

Kesimpulan hanya sebaik sampling dan measurement process.

## Descriptive Statistics

### Mean

$$
\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i
$$

### Median

Nilai tengah setelah data diurutkan. Median lebih tahan terhadap outlier daripada mean.

### Variance

Population variance:

$$
\sigma^2=\frac{1}{N}\sum_{i=1}^{N}(x_i-\mu)^2
$$

Sample variance:

$$
s^2=\frac{1}{n-1}\sum_{i=1}^{n}(x_i-\bar{x})^2
$$

### Standard Deviation

$$
\sigma=\sqrt{\sigma^2}
$$

Satuannya sama dengan data asli sehingga lebih mudah diinterpretasikan.

## Covariance dan Correlation

Covariance menunjukkan apakah dua variabel cenderung bergerak bersama:

$$
\operatorname{Cov}(X,Y)
=
\mathbb{E}[(X-\mu_X)(Y-\mu_Y)]
$$

Correlation menormalisasi covariance ke rentang $[-1,1]$:

$$
\rho_{X,Y}
=
\frac{\operatorname{Cov}(X,Y)}{\sigma_X\sigma_Y}
$$

> [!warning]
> Correlation tidak membuktikan causation.

### Contoh Manual

Untuk $x=[2,4,6]$:

$$
\bar{x}=4
$$

Population variance:

$$
\sigma^2
=
\frac{(2-4)^2+(4-4)^2+(6-4)^2}{3}
=\frac{8}{3}
$$

Sample variance membagi dengan $n-1$ untuk mengoreksi bias estimasi variance population.

## Inferential Statistics

Inferential statistics mencakup:

- estimasi parameter
- confidence interval
- hypothesis testing
- generalisasi dari sample ke populasi

### Standard Error

$$
\operatorname{SE}(\bar{x})
\approx
\frac{s}{\sqrt{n}}
$$

Standard deviation menggambarkan variasi data; standard error menggambarkan uncertainty estimasi mean.

### Confidence Interval

Secara sederhana:

$$
\bar{x}\pm t^*\frac{s}{\sqrt{n}}
$$

### Hypothesis Testing

P-value bukan probability bahwa null hypothesis benar dan bukan ukuran effect size. Pemilihan test harus mengikuti desain eksperimen dan asumsi.

## Implementasi

```python
import numpy as np

x = np.array([1, 2, 3, 4, 100], dtype=float)

print("mean:", x.mean())
print("median:", np.median(x))
print("sample variance:", x.var(ddof=1))
print("std:", x.std(ddof=1))
```

## Relevansi untuk AI dan CV

- Menghitung mean dan standard deviation untuk normalization.
- Memeriksa class imbalance dan distribusi resolusi gambar.
- Membandingkan performa beberapa seed.
- Melaporkan mean $\pm$ standard deviation pada eksperimen.
- Mendeteksi distribution shift dan model drift.

## Studi Kasus: Membandingkan Dua Model

Perbedaan accuracy 91.2% dan 91.5% belum cukup untuk menyimpulkan model kedua lebih baik. Periksa:

- beberapa seed
- paired evaluation
- confidence interval
- sample size
- effect size
- subgroup performance
- test-set reuse

## Data Leakage Statistik

Contoh leakage:

- normalization statistic dihitung dari seluruh data
- frame berdekatan tersebar ke train dan test
- subject yang sama muncul pada split berbeda
- duplicate image melintasi split

## Best Practice

- Visualisasikan distribusi.
- Laporkan mean beserta dispersion.
- Gunakan beberapa seed.
- Pisahkan statistical dan practical significance.
- Audit subgroup dan failure case.
- Tentukan analysis plan sebelum melihat hasil jika riset formal.

## Debugging dan Sanity Check

- Cek missing value dan duplicate.
- Bandingkan distribusi semua split.
- Pastikan `ddof` benar.
- Plot histogram serta boxplot.
- Jangan membulatkan angka terlalu awal.

## Kesalahan Umum

- Hanya melihat mean dan mengabaikan distribusi.
- Memakai statistik test set selama training.
- Melaporkan satu hasil seed sebagai kesimpulan kuat.
- Mengabaikan outlier, sample size, atau selection bias.

## Ringkasan

- Descriptive statistics merangkum data.
- Inferential statistics membantu menyimpulkan populasi dari sample.
- Variance dan standard deviation mengukur penyebaran.

## Checklist Pemahaman

- [ ] Bisa membedakan population, sample, parameter, dan statistic.
- [ ] Bisa menghitung mean dan variance manual.
- [ ] Bisa membedakan standard deviation dan standard error.
- [ ] Bisa menjelaskan correlation bukan causation.
- [ ] Bisa menjelaskan kenapa satu run tidak cukup.
- [ ] Bisa mengenali leakage.

## Hubungan Konsep

- Prasyarat: [[Probability]]
- Parent: [[Probability and Statistics MOC]]
- Lanjutan: [[Bayes Theorem]]
- Digunakan di: [[Dataset]], [[Model Evaluation]], [[Experiment]], [[Data Normalization]]

## Latihan

Untuk data `[2, 2, 4, 8]`, hitung mean dan jelaskan apakah median lebih kecil atau lebih besar dari mean.

2. Jelaskan pengaruh outlier pada mean dan median.
3. Kenapa standard error mengecil saat sample size membesar?
4. Rancang perbandingan fair dua model CV dengan lima seed.
