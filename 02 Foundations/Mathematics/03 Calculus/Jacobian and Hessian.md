---
type: concept
status: not-started
domain: mathematics
topic: calculus
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# Jacobian and Hessian

## Tujuan

Memahami turunan fungsi vector dan informasi kelengkungan fungsi scalar multivariable.

## Prasyarat

- [[Derivative]]
- [[Partial Derivative]]
- [[Gradient]]
- [[Matrix]]

## Jacobian

Untuk fungsi:

$$
\mathbf{f}:\mathbb{R}^n\rightarrow\mathbb{R}^m
$$

Jacobian adalah matrix seluruh first-order partial derivatives:

$$
\mathbf{J}_{\mathbf{f}}(\mathbf{x})
=
\begin{bmatrix}
\frac{\partial f_1}{\partial x_1} & \cdots & \frac{\partial f_1}{\partial x_n}\\
\vdots & \ddots & \vdots\\
\frac{\partial f_m}{\partial x_1} & \cdots & \frac{\partial f_m}{\partial x_n}
\end{bmatrix}
$$

Shape Jacobian adalah $m\times n$.

## Contoh Jacobian

$$
\mathbf{f}(x,y)=
\begin{bmatrix}
x^2y\\
\sin x+y
\end{bmatrix}
$$

$$
\mathbf{J}_{\mathbf{f}}
=
\begin{bmatrix}
2xy & x^2\\
\cos x & 1
\end{bmatrix}
$$

Jacobian adalah local linear approximation:

$$
\mathbf{f}(\mathbf{x}+\Delta\mathbf{x})
\approx
\mathbf{f}(\mathbf{x})
+
\mathbf{J}_{\mathbf{f}}(\mathbf{x})\Delta\mathbf{x}
$$

## Hessian

Untuk fungsi scalar:

$$
f:\mathbb{R}^n\rightarrow\mathbb{R}
$$

Hessian adalah matrix second-order partial derivatives:

$$
\mathbf{H}_f(\mathbf{x})
=
\begin{bmatrix}
\frac{\partial^2f}{\partial x_1^2} & \cdots & \frac{\partial^2f}{\partial x_1\partial x_n}\\
\vdots & \ddots & \vdots\\
\frac{\partial^2f}{\partial x_n\partial x_1} & \cdots & \frac{\partial^2f}{\partial x_n^2}
\end{bmatrix}
$$

Hessian menjelaskan curvature.

- positive definite → local minimum candidate
- negative definite → local maximum candidate
- indefinite → saddle point

## Contoh Hessian

$$
f(x,y)=x^2+3xy+2y^2
$$

$$
\nabla f=
\begin{bmatrix}
2x+3y\\
3x+4y
\end{bmatrix}
$$

$$
\mathbf{H}_f=
\begin{bmatrix}
2&3\\
3&4
\end{bmatrix}
$$

## Chain Rule dengan Jacobian

Untuk $\mathbf{y}=\mathbf{f}(\mathbf{x})$ dan $\mathbf{z}=\mathbf{g}(\mathbf{y})$:

$$
\mathbf{J}_{\mathbf{g}\circ\mathbf{f}}
=
\mathbf{J}_{\mathbf{g}}
\mathbf{J}_{\mathbf{f}}
$$

Urutan perkalian penting karena matrix multiplication tidak commutative.

## Implementasi PyTorch

```python
import torch

def vector_function(x: torch.Tensor) -> torch.Tensor:
    return torch.stack((x[0] ** 2 * x[1], torch.sin(x[0]) + x[1]))

def scalar_function(x: torch.Tensor) -> torch.Tensor:
    return x[0] ** 2 + 3 * x[0] * x[1] + 2 * x[1] ** 2

x = torch.tensor([1.0, 2.0], requires_grad=True)
jacobian = torch.autograd.functional.jacobian(vector_function, x)
hessian = torch.autograd.functional.hessian(scalar_function, x)
```

## Kenapa Full Jacobian Jarang Dibentuk?

Neural network dapat memiliki jutaan input dan output. Full Jacobian sangat mahal. Framework autograd memakai:

- vector-Jacobian product (VJP) pada reverse-mode autodiff
- Jacobian-vector product (JVP) pada forward-mode autodiff

Backpropagation pada dasarnya menyusun VJP tanpa menyimpan full Jacobian.

## Kompleksitas

Full Jacobian memerlukan $O(mn)$ elemen. Full Hessian memerlukan $O(n^2)$ memori, sehingga second-order optimization penuh sulit untuk model besar.

## Studi Kasus

- Jacobian pada camera projection dan robotics
- sensitivity analysis model
- Hessian untuk memahami curvature loss landscape
- Newton method dan second-order optimization
- adversarial robustness

## Best Practice

- Selalu cek shape input-output sebelum menghitung Jacobian.
- Gunakan JVP/VJP bila hanya butuh perkalian dengan vector.
- Hindari full Hessian pada network besar.
- Gunakan finite-difference hanya untuk gradient checking, bukan training.

## Kesalahan Umum

- Menyamakan gradient dengan Jacobian untuk semua fungsi.
- Salah urutan matrix pada chain rule.
- Menganggap Hessian positive definite hanya dari diagonal positif.
- Membentuk full Hessian tanpa mempertimbangkan memori.

## Ringkasan

Jacobian menggeneralisasi derivative untuk fungsi vector. Hessian menyimpan second derivative fungsi scalar dan menjelaskan curvature.

## Hubungan Konsep

- [[Chain Rule]]
- [[Backpropagation]]
- [[Gradient Descent]]
- [[Convex Optimization]]
- [[Camera Calibration]]

## Checklist Pemahaman

- [ ] Bisa menentukan shape Jacobian
- [ ] Bisa menghitung Jacobian fungsi sederhana
- [ ] Bisa menjelaskan informasi Hessian
- [ ] Paham alasan VJP/JVP digunakan

## Latihan

1. Hitung Jacobian $\mathbf{f}(x,y)=[x+y,xy]^\top$.
2. Hitung Hessian $f(x,y)=x^2+y^2$.
3. Jelaskan kenapa Hessian model dengan satu juta parameter tidak praktis dibentuk.

