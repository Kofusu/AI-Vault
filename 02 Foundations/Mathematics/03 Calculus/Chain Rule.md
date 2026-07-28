---
type: concept
status: not-started
domain: mathematics
topic: calculus
level: foundations
order: 3
created: 2026-07-28
---

# Chain Rule

## Tujuan

- Memahami derivative fungsi yang tersusun.
- Mengikuti aliran derivative dari output ke input.
- Menghubungkannya langsung dengan backpropagation.

## Intuisi

Jika $y$ bergantung pada $u$, dan $u$ bergantung pada $x$, perubahan $x$ memengaruhi $y$ melalui $u$.

```text
x → u → y
```

## Konsep Dasar

Chain rule mengalikan sensitivitas lokal sepanjang dependency path. Untuk beberapa tahap:

$$
\frac{dy}{dx}
=
\frac{dy}{du}
\frac{du}{dv}
\frac{dv}{dx}
$$

## Kenapa Dibutuhkan?

Neural network adalah komposisi function bertingkat. Tanpa chain rule, derivative loss terhadap parameter awal tidak dapat dihitung secara sistematis.

## Cara Kerja Backward

Setiap node menyimpan atau dapat menghitung local derivative. Backward pass mengalikan upstream gradient dengan local gradient:

$$
\text{gradient ke input}
=
\text{gradient dari output}
\times
\text{local derivative}
$$

## Rumus

Jika:

$$
y=f(u),\qquad u=g(x)
$$

maka:

$$
\frac{dy}{dx}
=
\frac{dy}{du}
\frac{du}{dx}
$$

## Contoh Manual

$$
y=(3x+1)^2
$$

Misalkan:

$$
u=3x+1,\qquad y=u^2
$$

Maka:

$$
\frac{dy}{du}=2u,
\qquad
\frac{du}{dx}=3
$$

$$
\frac{dy}{dx}=2u(3)=6(3x+1)
$$

## Computational Graph

```text
x ──×3──> 3x ──+1──> u ──square──> y
                                     │
             gradient mengalir <─────┘
```

Jika satu variable memengaruhi output melalui beberapa path, kontribusi gradient **dijumlahkan**:

$$
\frac{dy}{dx}
=
\sum_{\text{path }p}
\prod_{\text{edge }e\in p}
\frac{\partial e_{\text{out}}}
{\partial e_{\text{in}}}
$$

Backpropagation menerapkan chain rule dari loss menuju setiap parameter.

## Implementasi PyTorch

```python
import torch

x = torch.tensor(2.0, requires_grad=True)
u = 3 * x + 1
y = u**2

y.backward()
print(x.grad)  # 42
```

## Relevansi untuk Deep Learning

Neural network adalah komposisi banyak fungsi:

$$
\hat{y}=f_3(f_2(f_1(x)))
$$

Chain rule memungkinkan gradient dihitung layer demi layer secara efisien.

## Studi Kasus: Neuron Sederhana

$$
z=wx+b,\qquad
a=\sigma(z),\qquad
L=(a-y)^2
$$

$$
\frac{\partial L}{\partial w}
=
\frac{\partial L}{\partial a}
\frac{\partial a}{\partial z}
\frac{\partial z}{\partial w}
$$

Setiap faktor berasal dari satu operasi lokal.

## Vanishing dan Exploding Gradient

Perkalian banyak local derivative dapat:

- mendekati nol → vanishing gradient
- membesar drastis → exploding gradient

Activation, initialization, normalization, residual connection, dan gradient clipping membantu mengelola masalah ini.

## Best Practice

- Gambar computation graph untuk rumus kompleks.
- Hitung derivative lokal satu per satu.
- Jumlahkan kontribusi jika path bercabang.
- Hindari operasi yang memutus graph.
- Gradient-check custom layer.

## Kesalahan Umum

- Lupa mengalikan derivative bagian luar dan dalam.
- Menghitung urutan forward, tetapi kehilangan dependency saat backward.
- Melakukan operasi yang memutus computation graph tanpa sadar.

## Debugging

Gunakan hook atau inspect gradient per layer:

```python
for name, parameter in model.named_parameters():
    if parameter.grad is not None:
        print(name, parameter.grad.norm())
```

Norm mendekati nol pada banyak layer atau melonjak ekstrem memberi petunjuk masalah aliran gradient.

## Ringkasan

- Chain rule menangani fungsi komposisi.
- Gradient lokal dikalikan sepanjang jalur computation graph.
- Backpropagation adalah penerapan chain rule yang efisien.

## Checklist Pemahaman

- [ ] Bisa memecah fungsi komposisi.
- [ ] Bisa mengalikan local derivative.
- [ ] Tahu kontribusi multi-path dijumlahkan.
- [ ] Bisa menjelaskan backpropagation.
- [ ] Bisa menjelaskan vanishing dan exploding gradient.

## Hubungan Konsep

- Prasyarat: [[Derivative]], [[Partial Derivative]]
- Parent: [[Calculus MOC]]
- Lanjutan: [[Gradient]], [[Backpropagation]]
- Digunakan di: [[Neural Network]], [[PyTorch Autograd]]

## Latihan

Hitung derivative $y=(2x-3)^3$ menggunakan substitusi fungsi dalam dan fungsi luar.

2. Turunkan $\partial L/\partial w$ pada neuron sederhana di atas.
3. Buat computation graph untuk $y=(x^2+1)^3$.
