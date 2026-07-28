---
type: concept
status: not-started
domain: machine-learning
topic: learning-paradigms
level: foundations
order: 5
created: 2026-07-28
---

# Self-Supervised Learning

## Tujuan

- Memahami supervisory signal yang dibuat dari data sendiri.
- Mengenal pretext task, contrastive, masked modeling, dan transfer.
- Membedakannya dari unsupervised dan semi-supervised.

## Intuisi

Data membuat “soal dan jawaban” sendiri:

```text
Image
 ↓ masking/augmentation
Training task tanpa human label
 ↓
Representation
 ↓ fine-tuning
Downstream task
```

## Konsep Dasar

Self-supervised learning tidak membutuhkan human annotation untuk pretraining, tetapi tetap mempunyai target yang dibentuk otomatis.

## Kenapa Dibutuhkan?

- unlabeled data sangat besar
- label mahal
- representation dapat ditransfer
- foundation model membutuhkan scalable objective

## Cara Kerja

### Contrastive Learning

View dari sample sama dibuat dekat; sample berbeda dibuat terpisah.

$$
\operatorname{sim}(z_i,z_i^+)
>
\operatorname{sim}(z_i,z_j^-)
$$

### Masked Modeling

Sebagian input disembunyikan, model memprediksi content atau representation yang hilang.

### Distillation tanpa Label

Teacher dan student menghasilkan target representation dengan augmentation berbeda.

## Augmentation sebagai Assumption

Jika dua augmentation dianggap semantic-equivalent, model didorong invariant terhadap perubahan itu.

Augmentation yang menghapus informasi task dapat merusak representation.

## Implementasi Konseptual

```python
view_a = augment(image)
view_b = augment(image)

embedding_a = encoder(view_a)
embedding_b = encoder(view_b)

loss = contrastive_loss(
    embedding_a,
    embedding_b,
)
```

## Pretraining dan Fine-Tuning

```text
Large unlabeled corpus
      ↓ pretrain
Encoder
      ↓ linear probe / fine-tune
Labeled downstream dataset
```

Linear probe menguji kualitas fixed representation. Fine-tuning mengubah encoder.

## Studi Kasus Computer Vision

Pretrain encoder pada image pabrik tanpa defect label, lalu fine-tune untuk:

- defect classification
- anomaly detection
- retrieval

Evaluation harus memakai downstream split yang tidak bocor dari pretraining jika claim generalization membutuhkannya.

## Best Practice

- Dokumentasikan pretraining corpus.
- Pilih augmentation sesuai domain.
- Bandingkan random initialization dan supervised pretraining.
- Pisahkan linear probe dan fine-tuning result.
- Audit compute cost.
- Cek duplicate antara pretrain dan benchmark.

## Kesalahan Umum

- Menyamakan self-supervised dengan tanpa objective.
- Augmentation mengubah semantic.
- Benchmark contamination.
- Claim representation umum dari satu downstream task.
- Mengabaikan compute budget.

## Debugging

- Monitor representation collapse.
- Periksa embedding variance.
- Visualisasikan nearest neighbor.
- Uji linear probe.
- Ablate augmentation.
- Periksa loss tanpa hanya mengandalkan magnitude.

## Ringkasan

- Self-supervision membuat target dari struktur data.
- Representation dipelajari sebelum downstream label.
- Augmentation dan objective menentukan invariance.
- Evaluation transfer harus fair.

## Checklist Pemahaman

- [ ] Bisa membedakan self, semi, dan unsupervised.
- [ ] Bisa menjelaskan contrastive learning.
- [ ] Bisa menjelaskan masked modeling.
- [ ] Bisa membedakan linear probe dan fine-tuning.
- [ ] Bisa mengenali representation collapse.

## Latihan

1. Rancang dua view augmentation untuk produk industri.
2. Identifikasi augmentation berbahaya.
3. Buat downstream evaluation plan.
4. Bandingkan random initialization.

