---
type: concept
status: not-started
domain: mathematics
topic: calculus
level: foundations
order: 2
created: 2026-07-28
---

# Partial Derivative

## Tujuan

- Memahami perubahan fungsi multivariable terhadap satu variabel.
- Menghitung partial derivative sederhana.
- Menghubungkannya dengan parameter neural network.

## Intuisi

Jika fungsi mempunyai banyak input, partial derivative mengubah satu variabel sementara variabel lain dianggap konstan.

$$
f(x,y)=x^2+3xy+y^2
$$

## Konsep Dasar

Untuk:

$$
f:\mathbb{R}^n\rightarrow\mathbb{R}
$$

terdapat satu partial derivative terhadap setiap input. Saat menghitung $\partial f/\partial x_i$, input lain dianggap konstan.

## Kenapa Dibutuhkan?

Loss neural network bergantung pada banyak parameter. Kita perlu mengetahui kontribusi lokal setiap parameter:

$$
\frac{\partial L}{\partial w_1},
\frac{\partial L}{\partial w_2},
\ldots
$$

## Cara Kerja

Bayangkan permukaan $z=f(x,y)$. Partial derivative terhadap $x$ adalah slope ketika bergerak hanya sejajar axis $x$; terhadap $y$ ketika bergerak sejajar axis $y$.

## Perhitungan

Terhadap $x$:

$$
\frac{\partial f}{\partial x}=2x+3y
$$

Terhadap $y$:

$$
\frac{\partial f}{\partial y}=3x+2y
$$

Pada $(x,y)=(1,2)$:

$$
\frac{\partial f}{\partial x}=8,
\qquad
\frac{\partial f}{\partial y}=7
$$

## Notasi

Simbol $\partial$ dipakai karena fungsi bergantung pada lebih dari satu variabel.

```text
df/dx   → derivative fungsi satu variabel
∂f/∂x   → partial derivative fungsi multivariable
```

## Implementasi dengan PyTorch

```python
import torch

x = torch.tensor(1.0, requires_grad=True)
y = torch.tensor(2.0, requires_grad=True)

f = x**2 + 3 * x * y + y**2
f.backward()

print(x.grad)  # 8
print(y.grad)  # 7
```

### Numerical Check

```python
def f(x: float, y: float) -> float:
    return x**2 + 3 * x * y + y**2


h = 1e-5
df_dx = (f(1 + h, 2) - f(1 - h, 2)) / (2 * h)
```

## Relevansi untuk AI

Loss neural network bergantung pada jutaan parameter. Setiap parameter mempunyai partial derivative terhadap loss.

$$
\frac{\partial L}{\partial w_i}
$$

## Studi Kasus: Linear Regression

$$
\hat{y}=wx+b
$$

$$
L=(\hat{y}-y)^2
$$

$$
\frac{\partial L}{\partial w}
=
2(\hat{y}-y)x
$$

$$
\frac{\partial L}{\partial b}
=
2(\hat{y}-y)
$$

Parameter $w$ dan $b$ memengaruhi loss secara berbeda.

## Partial vs Total Derivative

Partial derivative mengubah satu variable independen. Total derivative juga memperhitungkan dependency antar-variable. [[Chain Rule]] menangani dependency tersebut pada computation graph.

## Best Practice

- Tulis variable mana yang dianggap konstan.
- Periksa dependency graph sebelum menurunkan.
- Gunakan autograd untuk model, tetapi pahami rumus lokalnya.
- Lakukan gradient check pada custom operation.

## Kesalahan Umum

- Ikut mendiferensiasikan variabel yang seharusnya dianggap konstan.
- Tertukar antara partial derivative dan total derivative.
- Lupa menghapus gradient lama di training loop PyTorch.

## Debugging

Jika `.grad` bernilai `None`, periksa:

- `requires_grad=True`
- tensor adalah leaf saat `.grad` dibaca
- computation graph tidak terputus oleh `.detach()`, `.item()`, atau conversion
- `backward()` benar-benar dipanggil

## Ringkasan

- Partial derivative mengukur sensitivitas terhadap satu variabel.
- Kumpulan partial derivative membentuk [[Gradient]].
- Autograd menghitung partial derivative melalui computation graph.

## Checklist Pemahaman

- [ ] Bisa menahan variable lain sebagai konstan.
- [ ] Bisa menghitung dua partial derivative manual.
- [ ] Bisa membedakan partial dan total derivative.
- [ ] Bisa menghubungkannya dengan parameter model.
- [ ] Bisa mendiagnosis `.grad is None`.

## Hubungan Konsep

- Prasyarat: [[Derivative]]
- Parent: [[Calculus MOC]]
- Lanjutan: [[Chain Rule]], [[Gradient]]
- Digunakan di: [[Backpropagation]], [[PyTorch Autograd]]

## Latihan

Untuk $f(x,y)=xy+2y^2$, hitung $\partial f/\partial x$ dan $\partial f/\partial y$.

2. Turunkan loss linear regression terhadap $w$ dan $b$.
3. Verifikasi hasil dengan PyTorch autograd.
