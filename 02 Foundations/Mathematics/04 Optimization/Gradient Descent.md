---
type: concept
status: not-started
domain: mathematics
topic: optimization
level: foundations
order: 1
created: 2026-07-28
---

# Gradient Descent

## Tujuan

- Memahami cara parameter diperbarui untuk menurunkan loss.
- Menjelaskan pengaruh learning rate.
- Membedakan batch, stochastic, dan mini-batch gradient descent.

## Intuisi

Bayangkan berada di lereng berkabut dan ingin mencapai titik rendah. Gradient menunjukkan arah naik tercepat, sehingga kita bergerak ke arah sebaliknya.

## Konsep Dasar

Optimization problem:

$$
\theta^*
=
\arg\min_\theta L(\theta)
$$

$\theta^*$ adalah parameter yang meminimalkan objective. Gradient descent adalah iterative first-order method karena memakai derivative pertama.

## Kenapa Dibutuhkan?

Model tidak mengetahui parameter terbaik secara langsung. Training mengulang:

```text
predict → measure error → compute gradient → update parameter
```

## Cara Kerja Step-by-Step

1. Inisialisasi parameter.
2. Lakukan forward pass.
3. Hitung scalar loss.
4. Backpropagate gradient.
5. Update parameter.
6. Ulangi sampai stopping condition.

## Rumus Update

$$
\theta_{t+1}
=
\theta_t-\eta\nabla_\theta L(\theta_t)
$$

- $\theta_t$: parameter saat ini.
- $\eta$: learning rate.
- $L$: loss function.
- $\nabla_\theta L$: gradient loss terhadap parameter.

## Contoh Manual

$$
L(w)=(w-3)^2
$$

$$
\frac{dL}{dw}=2(w-3)
$$

Jika $w_0=0$ dan $\eta=0.1$:

$$
w_1=0-0.1[2(0-3)]=0.6
$$

Parameter bergerak mendekati minimum $w=3$.

## Pengaruh Learning Rate

```text
Terlalu kecil → stabil tetapi lambat
Tepat         → convergence efisien
Terlalu besar → overshoot atau divergen
```

## Variasi

- **Batch GD:** seluruh training set per update.
- **Stochastic GD:** satu sample per update.
- **Mini-batch GD:** sejumlah kecil sample per update; standar Deep Learning.

Mini-batch gradient bersifat noisy, tetapi noise dapat membantu eksplorasi loss landscape dan efisien untuk hardware parallel.

## Implementasi dari Nol

```python
w = 0.0
learning_rate = 0.1

for _ in range(20):
    gradient = 2 * (w - 3)
    w = w - learning_rate * gradient

print(w)
```

## Implementasi PyTorch

```python
optimizer.zero_grad()
prediction = model(inputs)
loss = criterion(prediction, targets)
loss.backward()
optimizer.step()
```

## Relevansi untuk AI

Gradient descent menjadi fondasi SGD, Momentum, RMSProp, Adam, dan AdamW. Optimizer modern mengubah cara magnitude serta riwayat gradient digunakan, tetapi prinsip dasarnya tetap update untuk menurunkan objective.

## Faktor yang Memengaruhi Convergence

- learning rate
- initialization
- feature scaling
- batch size
- curvature
- noise gradient
- optimizer state
- scheduler
- regularization

## Momentum

Momentum mengakumulasi arah update:

$$
v_t=\beta v_{t-1}+\nabla L(\theta_t)
$$

$$
\theta_{t+1}=\theta_t-\eta v_t
$$

Ini dapat mengurangi oscillation dan mempercepat gerak pada arah konsisten.

## Studi Kasus Training Loop

```python
for images, targets in train_loader:
    optimizer.zero_grad()
    predictions = model(images)
    loss = criterion(predictions, targets)
    loss.backward()
    optimizer.step()
```

Urutannya penting. Validation tidak boleh memanggil `optimizer.step()`.

## Stopping dan Generalization

Optimization loss training bukan satu-satunya tujuan. Pantau validation metric dan gunakan early stopping atau model selection yang tidak menyentuh test set.

## Best Practice

- Mulai dari baseline optimizer dan learning rate wajar.
- Log loss, metric, learning rate, dan gradient norm.
- Bandingkan optimizer dengan budget yang fair.
- Simpan config, seed, dan checkpoint.
- Gunakan scheduler berdasarkan alasan, bukan kebiasaan.
- Jangan memilih hyperparameter menggunakan test set.

## Kesalahan Umum

- Learning rate terlalu besar atau terlalu kecil.
- Lupa mengosongkan accumulated gradient.
- Membandingkan optimizer dengan setup yang tidak fair.
- Menganggap loss training yang turun menjamin generalization.
- Mengira semua minimum yang ditemukan adalah global minimum.

## Debugging

Loss tidak turun:

- cek gradient `None` atau nol
- cek label dan loss compatibility
- overfit satu batch kecil
- periksa learning rate
- pastikan `model.train()`

Loss `NaN`:

- cek data invalid
- cek operasi numerik
- turunkan learning rate
- inspect gradient norm
- coba gradient clipping sebagai diagnosis

## Ringkasan

- Gradient descent bergerak berlawanan arah gradient.
- Learning rate mengatur ukuran langkah.
- Mini-batch gradient descent adalah praktik umum untuk neural network.

## Checklist Pemahaman

- [ ] Bisa menjelaskan rumus update.
- [ ] Bisa melakukan satu update manual.
- [ ] Bisa membedakan batch, stochastic, dan mini-batch.
- [ ] Bisa menjelaskan momentum.
- [ ] Bisa menulis training loop yang benar.
- [ ] Bisa mendiagnosis loss tidak turun.

## Hubungan Konsep

- Prasyarat: [[Derivative]], [[Gradient]], [[Chain Rule]]
- Parent: [[Optimization MOC]]
- Terkait: [[Convex Optimization]], [[Backpropagation]]
- Lanjutan: [[SGD]], [[Adam]], [[AdamW]], [[Learning Rate Scheduler]]

## Latihan

Untuk $L(w)=w^2$, $w_0=4$, dan $\eta=0.1$, hitung $w_1$.

2. Hitung $w_2$.
3. Jelaskan pengaruh learning rate $1.1$ pada fungsi tersebut.
4. Buat eksperimen overfit satu batch sebagai sanity check.
