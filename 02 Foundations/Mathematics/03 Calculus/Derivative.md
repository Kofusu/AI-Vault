---
type: concept
status: not-started
domain: mathematics
topic: calculus
level: foundations
order: 1
created: 2026-07-28
---

# Derivative

## Tujuan

- Memahami derivative sebagai laju perubahan dan kemiringan lokal.
- Menghitung derivative fungsi sederhana.
- Menghubungkannya dengan perubahan loss.

## Intuisi

Derivative menjawab:

> Jika input berubah sedikit, seberapa besar output ikut berubah?

Untuk fungsi $y=f(x)$, derivative adalah slope garis tangent di satu titik.

## Konsep Dasar

Derivative mempunyai beberapa interpretasi:

- slope grafik
- instantaneous rate of change
- sensitivitas output terhadap input
- local linear approximation

$$
f(x+\Delta x)
\approx
f(x)+f'(x)\Delta x
$$

## Kenapa Dibutuhkan?

Training model membutuhkan jawaban terhadap pertanyaan:

> Jika parameter diubah sedikit, apakah loss naik atau turun, dan seberapa besar?

Derivative menyediakan informasi lokal tersebut.

## Cara Kerja

Secant slope memakai dua titik. Saat jarak $h$ mengecil menuju nol, secant mendekati tangent slope.

```text
Finite difference
      ↓ h → 0
Limit
      ↓
Derivative
```

## Definisi

$$
f'(x)
=
\lim_{h\to0}
\frac{f(x+h)-f(x)}{h}
$$

## Contoh

Untuk:

$$
f(x)=x^2
$$

Derivative:

$$
f'(x)=2x
$$

Pada $x=3$:

$$
f'(3)=6
$$

Artinya, di sekitar $x=3$, kenaikan kecil $\Delta x$ menghasilkan perubahan kira-kira:

$$
\Delta f\approx6\Delta x
$$

## Aturan Dasar

$$
\frac{d}{dx}c=0
$$

$$
\frac{d}{dx}x^n=nx^{n-1}
$$

$$
\frac{d}{dx}[f(x)+g(x)]
=f'(x)+g'(x)
$$

Product rule:

$$
(fg)'=f'g+fg'
$$

Quotient rule:

$$
\left(\frac{f}{g}\right)'
=
\frac{f'g-fg'}{g^2}
$$

Derivative activation penting:

$$
\frac{d}{dx}\operatorname{ReLU}(x)
=
\begin{cases}
0,&x<0\\
1,&x>0
\end{cases}
$$

Pada $x=0$, ReLU tidak differentiable secara klasik; framework memilih subgradient convention.

## Implementasi Numerik

```python
def f(x: float) -> float:
    return x**2


x = 3.0
h = 1e-5
numerical_derivative = (f(x + h) - f(x)) / h

print(numerical_derivative)
```

Central difference biasanya lebih akurat:

```python
central_difference = (f(x + h) - f(x - h)) / (2 * h)
```

Jika $h$ terlalu besar, approximation kasar. Jika terlalu kecil, floating-point cancellation dapat mendominasi.

## Relevansi untuk AI

Derivative menunjukkan bagaimana loss berubah terhadap satu parameter. Informasi ini menentukan arah update parameter.

## Studi Kasus: Squared Error

$$
L(w)=(wx-y)^2
$$

Dengan [[Chain Rule]]:

$$
\frac{dL}{dw}
=
2(wx-y)x
$$

Gradient besar berarti loss sensitif terhadap perubahan $w$ pada titik tersebut.

## Differentiability

Tidak semua fungsi differentiable di semua titik. Deep Learning tetap dapat memakai:

- derivative hampir di semua titik
- subgradient
- smooth approximation

Optimization juga dapat gagal jika operasi diskrit memutus dependency gradient.

## Best Practice

- Bedakan nilai fungsi dan derivative.
- Gunakan symbolic derivative untuk pemahaman.
- Gunakan autograd untuk model besar.
- Gunakan finite difference untuk gradient check kecil.
- Periksa skala input karena memengaruhi magnitude derivative.

## Debugging

Bandingkan analytical dan numerical derivative:

$$
\text{relative error}
=
\frac{|g_a-g_n|}
{\max(1,|g_a|,|g_n|)}
$$

Jalankan gradient check dengan double precision dan ukuran input kecil.

## Kesalahan Umum

- Menganggap derivative sama dengan nilai fungsi.
- Lupa bahwa derivative bersifat lokal.
- Memakai nilai `h` terlalu besar atau terlalu kecil pada numerical differentiation.

## Ringkasan

- Derivative mengukur laju perubahan lokal.
- Tanda derivative menunjukkan arah naik atau turun.
- Derivative adalah fondasi gradient dan optimization.

## Checklist Pemahaman

- [ ] Bisa menjelaskan derivative sebagai slope dan sensitivitas.
- [ ] Bisa memakai power, product, dan quotient rule.
- [ ] Bisa menghitung finite difference.
- [ ] Bisa menjelaskan local linear approximation.
- [ ] Tahu derivative ReLU di titik nol membutuhkan convention.

## Hubungan Konsep

- Parent: [[Calculus MOC]]
- Lanjutan: [[Partial Derivative]], [[Chain Rule]], [[Gradient]]
- Digunakan di: [[Loss Function]], [[Backpropagation]]

## Latihan

Hitung derivative $f(x)=3x^2+2x-5$, lalu evaluasi pada $x=2$.

2. Turunkan $f(x)=x^2\sin x$ dengan product rule.
3. Bandingkan forward dan central difference pada $f(x)=x^3$.
