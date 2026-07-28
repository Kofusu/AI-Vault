---
type: concept
status: not-started
domain: mathematics
topic: calculus
level: foundations
order: 4
created: 2026-07-28
---

# Gradient

## Tujuan

- Memahami gradient sebagai vector partial derivative.
- Menafsirkan arah dan magnitude gradient.
- Menghubungkannya dengan optimization neural network.

## Intuisi

Gradient menunjukkan arah kenaikan tercepat suatu fungsi. Arah negatif gradient menunjukkan penurunan tercepat secara lokal.

## Konsep Dasar

Gradient menggabungkan semua [[Partial Derivative]] fungsi scalar-valued menjadi satu [[Vector]]. Shape gradient terhadap parameter sama dengan shape parameter tersebut.

## Kenapa Dibutuhkan?

Satu model dapat mempunyai jutaan parameter. Gradient merangkum sensitivitas loss terhadap semuanya dan memberi arah lokal untuk optimization.

## Cara Kerja

Directional derivative pada unit vector $\mathbf{u}$:

$$
D_{\mathbf{u}}f
=
\nabla f\cdot\mathbf{u}
$$

Nilai maksimum terjadi ketika $\mathbf{u}$ searah gradient. Nilai minimum terjadi pada arah negative gradient.

## Rumus

Untuk fungsi:

$$
f(x_1,x_2,\ldots,x_n)
$$

gradient-nya:

$$
\nabla f=
\begin{bmatrix}
\frac{\partial f}{\partial x_1}\\
\frac{\partial f}{\partial x_2}\\
\vdots\\
\frac{\partial f}{\partial x_n}
\end{bmatrix}
$$

## Contoh Manual

$$
f(x,y)=x^2+2y^2
$$

$$
\nabla f=
\begin{bmatrix}
2x\\
4y
\end{bmatrix}
$$

Pada $(x,y)=(1,2)$:

$$
\nabla f(1,2)=
\begin{bmatrix}
2\\
8
\end{bmatrix}
$$

Komponen $y$ lebih besar, sehingga fungsi lebih sensitif terhadap perubahan $y$ di titik tersebut.

## Gradient dan Level Curve

Gradient tegak lurus terhadap level curve dan menunjuk ke arah kenaikan tercepat.

```text
       ↑ ∇f
   ----•----  level curve
       ↓ -∇f
```

Jika gradient nol:

$$
\nabla f=0
$$

titik tersebut disebut stationary point dan dapat berupa minimum, maximum, atau saddle point.

## Implementasi PyTorch

```python
import torch

x = torch.tensor(1.0, requires_grad=True)
y = torch.tensor(2.0, requires_grad=True)

f = x**2 + 2 * y**2
f.backward()

print(x.grad, y.grad)  # tensor(2.), tensor(8.)
```

## Relevansi untuk AI

Untuk loss $L(\theta)$:

$$
\nabla_\theta L
$$

memberi sensitivitas loss terhadap seluruh parameter $\theta$. Optimizer memakai informasi ini untuk memperbarui parameter.

## Gradient, Jacobian, dan Hessian

- Gradient: scalar output terhadap vector input.
- Jacobian: vector output terhadap vector input.
- Hessian: matrix second partial derivatives dari scalar function.

$$
H_{ij}
=
\frac{\partial^2f}
{\partial x_i\partial x_j}
$$

Hessian menggambarkan curvature lokal.

## Studi Kasus: Gradient Checking

Analytical gradient dari autograd dibandingkan dengan finite difference:

```python
def finite_difference(f, x, index, epsilon=1e-6):
    x_plus = x.copy()
    x_minus = x.copy()
    x_plus[index] += epsilon
    x_minus[index] -= epsilon
    return (f(x_plus) - f(x_minus)) / (2 * epsilon)
```

Gunakan pada model kecil dan double precision.

## Gradient Norm

$$
\|\nabla_\theta L\|_2
$$

Gradient norm membantu memonitor vanishing atau exploding gradient. Gradient clipping membatasi norm, tetapi tidak memperbaiki root cause semua instability.

## Best Practice

- Monitor gradient norm.
- Standardize feature jika scale sangat berbeda.
- Gunakan gradient check untuk operasi custom.
- Bedakan zero gradient di optimum dari zero gradient akibat saturation atau graph terputus.
- Jangan mengubah `.grad` tanpa alasan jelas.

## Kesalahan Umum

- Mengira gradient adalah satu scalar.
- Menganggap negative gradient selalu langsung menuju global minimum.
- Mengabaikan skala feature yang membuat arah optimization tidak seimbang.
- Tidak melakukan `optimizer.zero_grad()` saat dibutuhkan.

## Debugging

Jika gradient `NaN` atau `Inf`:

- periksa loss sebelum backward
- cari division by zero atau `log(0)`
- turunkan learning rate
- periksa data dan target
- aktifkan anomaly detection secara sementara
- inspect norm per layer

## Ringkasan

- Gradient adalah vector berisi seluruh partial derivative.
- Gradient menunjukkan kenaikan tercepat.
- Negative gradient menjadi arah update dasar pada [[Gradient Descent]].

## Checklist Pemahaman

- [ ] Bisa membentuk gradient dari partial derivative.
- [ ] Bisa menjelaskan arah gradient.
- [ ] Bisa membedakan gradient, Jacobian, dan Hessian.
- [ ] Bisa menjelaskan stationary point.
- [ ] Bisa melakukan gradient check sederhana.
- [ ] Bisa mendiagnosis gradient `NaN`.

## Hubungan Konsep

- Prasyarat: [[Vector]], [[Partial Derivative]], [[Chain Rule]]
- Parent: [[Calculus MOC]]
- Lanjutan: [[Gradient Descent]]
- Digunakan di: [[Backpropagation]], [[Optimizer]], [[Loss Function]]

## Latihan

Hitung gradient $f(x,y)=3x^2+xy+y^2$ pada titik $(1,2)$.

2. Tentukan directional derivative pada arah unit $[1,0]$.
3. Apakah gradient nol selalu berarti minimum? Beri counterexample.
