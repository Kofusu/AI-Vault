---
type: concept
status: not-started
domain: mathematics
topic: optimization
level: intermediate
order: 2
created: 2026-07-28
---

# Convex Optimization

## Tujuan

- Memahami fungsi dan optimization problem yang convex.
- Menjelaskan perbedaan local dan global minimum.
- Mengetahui keterbatasannya untuk Deep Learning.

## Intuisi

Fungsi convex berbentuk seperti mangkuk: garis yang menghubungkan dua titik pada grafik tidak berada di bawah grafik.

```text
Loss
 ^
 | \       /
 |  \_____/
 +----------> parameter
       minimum global
```

## Konsep Dasar

Optimization problem mempunyai:

- decision variable
- objective function
- optional constraints
- feasible set

Problem convex memerlukan objective convex untuk minimization dan feasible set convex.

## Kenapa Dibutuhkan?

Convexity memberikan jaminan yang kuat:

- local minimum adalah global minimum
- stopping criteria dapat dianalisis
- banyak algoritma memiliki convergence guarantee
- solution quality lebih mudah diverifikasi

Konsep ini menjadi baseline untuk memahami kesulitan optimization non-convex.

## Cara Kerja dan Mengecek Convexity

Pendekatan umum:

- gunakan definisi garis chord
- periksa second derivative $f''(x)\geq0$
- periksa Hessian positive semidefinite
- gunakan composition rules

## Definisi Fungsi Convex

Fungsi $f$ convex jika untuk semua $x,y$ dan $\lambda\in[0,1]$:

$$
f(\lambda x+(1-\lambda)y)
\leq
\lambda f(x)+(1-\lambda)f(y)
$$

## Kenapa Penting?

Pada masalah convex:

- setiap local minimum adalah global minimum
- teori convergence lebih kuat
- solusi lebih mudah dianalisis

Contoh umum:

- linear regression dengan squared error
- logistic regression dengan objective convex tertentu
- support vector machine

## Convex Set

Sebuah set convex jika garis di antara dua titik mana pun tetap berada di dalam set tersebut.

Constraint optimization sering ditulis:

$$
\min_x f(x)
$$

dengan:

$$
g_i(x)\leq0
$$

Contoh convex set:

- interval
- affine subspace
- Euclidean ball
- intersection dari convex sets

Union dua convex sets belum tentu convex.

## Hubungan dengan Hessian

Untuk fungsi dua kali differentiable, Hessian positive semidefinite di seluruh domain merupakan tanda fungsi convex:

$$
\nabla^2f(x)\succeq0
$$

Jika Hessian positive definite, fungsi strictly convex secara lokal pada kondisi sesuai. Strong convexity memberi lower curvature bound dan convergence yang lebih kuat.

## Deep Learning Bersifat Non-convex

Loss surface neural network umumnya non-convex karena:

- banyak layer dan nonlinear activation
- simetri parameter
- interaksi parameter yang kompleks

Artinya, jaminan convex optimization tidak langsung berlaku. Namun, konsep gradient, local geometry, constraint, dan regularization tetap relevan.

Non-convex tidak berarti tidak dapat dioptimalkan. Neural network besar memiliki struktur, overparameterization, stochasticity, dan banyak solusi yang performanya baik.

## Implementasi Visual Sederhana

```python
import numpy as np

x = np.linspace(-4, 4, 100)
convex_loss = x**2
non_convex_loss = x**4 - 3 * x**2
```

Plot kedua fungsi untuk membandingkan satu minimum global dengan beberapa basin.

## Studi Kasus: Linear Regression

Squared-error objective:

$$
L(\mathbf{w})
=
\|X\mathbf{w}-\mathbf{y}\|_2^2
$$

adalah convex terhadap $\mathbf{w}$. Jika $X$ mempunyai rank yang sesuai, solusi dapat unik. Regularization:

$$
L(\mathbf{w})+\lambda\|\mathbf{w}\|_2^2
$$

tetap convex untuk $\lambda\geq0$.

## Convex vs Non-convex

```text
Convex:
local minimum = global minimum

Non-convex:
local minimum, saddle point,
flat region, dan multiple basin
```

## Best Practice

- Nyatakan variable, objective, dan constraint.
- Cek domain function.
- Bedakan convex set dan convex function.
- Gunakan solver yang mengeksploitasi struktur problem.
- Jangan mengklaim global optimum pada neural network tanpa dasar.
- Bedakan optimization success dan generalization.

## Kesalahan Umum

- Menganggap fungsi yang terlihat seperti mangkuk selalu convex tanpa memeriksa domain.
- Menganggap neural network mempunyai objective convex.
- Mengira non-convex berarti optimization mustahil.
- Menyamakan convex function dengan convex set.

## Debugging dan Sanity Check

- Plot fungsi satu atau dua dimensi.
- Periksa Hessian atau second derivative.
- Uji midpoint inequality sebagai sanity check, bukan bukti lengkap.
- Periksa feasibility constraint.
- Bandingkan solusi dari beberapa initialization pada problem non-convex.

## Ringkasan

- Convexity memberi struktur dan jaminan optimization yang kuat.
- Pada fungsi convex, local minimum juga global minimum.
- Deep Learning umumnya non-convex, tetapi konsep convex tetap menjadi fondasi penting.

## Checklist Pemahaman

- [ ] Bisa menjelaskan definisi convex function.
- [ ] Bisa membedakan convex function dan convex set.
- [ ] Bisa menjelaskan local vs global minimum.
- [ ] Bisa memakai second derivative atau Hessian.
- [ ] Bisa menjelaskan kenapa Deep Learning non-convex.
- [ ] Tahu non-convex bukan berarti mustahil.

## Hubungan Konsep

- Prasyarat: [[Derivative]], [[Gradient]], [[Matrix]]
- Parent: [[Optimization MOC]]
- Terkait: [[Gradient Descent]], [[Loss Function]], [[Hessian Matrix]]
- Digunakan di: [[Linear Regression]], [[Logistic Regression]], [[Support Vector Machine]]

## Latihan

Jelaskan kenapa menemukan local minimum cukup untuk menyelesaikan optimization jika objective-nya convex.

2. Buktikan $f(x)=x^2$ convex dengan second derivative.
3. Apakah $f(x)=x^4-3x^2$ convex di seluruh real line?
4. Jelaskan kenapa L2-regularized linear regression tetap convex.
