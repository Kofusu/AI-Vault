---
type: concept
status: not-started
domain: machine-learning
topic: supervised-learning
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# Decision Tree

## Tujuan

Memahami model berbasis aturan yang membagi feature space secara rekursif.

## Intuisi

Decision Tree membuat rangkaian pertanyaan:

```text
feature_j <= threshold?
├── ya  → node kiri
└── tidak → node kanan
```

Leaf menghasilkan class, probability, atau nilai regression.

## Split Classification

Gini impurity:

$$
G=1-\sum_kp_k^2
$$

Entropy:

$$
H=-\sum_kp_k\log_2p_k
$$

Split dipilih untuk memaksimalkan penurunan impurity.

## Split Regression

Umumnya meminimalkan weighted squared error atau absolute error setelah split.

## Training

1. Evaluasi kandidat feature dan threshold.
2. Pilih split terbaik.
3. Bagi data ke child nodes.
4. Ulangi sampai stopping criterion.

Algoritma greedy tidak menjamin tree global terbaik.

## Implementasi

```python
from sklearn.tree import DecisionTreeClassifier, plot_tree
import matplotlib.pyplot as plt

model = DecisionTreeClassifier(
    criterion="gini",
    max_depth=4,
    min_samples_leaf=10,
    random_state=42,
)
model.fit(X_train, y_train)

plt.figure(figsize=(14, 8))
plot_tree(model, feature_names=feature_names, filled=True)
plt.show()
```

## Overfitting

Tree tanpa batas dapat menghafal training data. Kendalikan dengan:

- `max_depth`
- `min_samples_split`
- `min_samples_leaf`
- `max_leaf_nodes`
- cost-complexity pruning

## Feature Importance

Impurity-based importance dapat bias terhadap fitur continuous atau high-cardinality. Untuk interpretasi, pertimbangkan permutation importance dan validasi domain.

## Kompleksitas

Prediction rata-rata sekitar $O(\text{depth})$. Training bergantung implementasi dan pencarian split, sering mendekati $O(nd\log n)$.

## Kelebihan

- mudah divisualisasikan
- menangani nonlinearity dan interaction
- tidak membutuhkan scaling
- cocok untuk data tabular

## Kekurangan

- mudah overfit
- tidak stabil terhadap perubahan kecil data
- split axis-aligned dapat tidak efisien

## Best Practice

- Batasi kompleksitas tree.
- Evaluasi dengan cross-validation.
- Jangan menganggap feature importance sebagai causal effect.
- Gunakan ensemble untuk performa yang lebih stabil.

## Kesalahan Umum

- Membiarkan default tree tumbuh sangat dalam.
- Menilai model hanya dari training accuracy.
- Menginterpretasikan setiap rule sebagai kebenaran universal.
- Mengabaikan class imbalance.

## Ringkasan

Decision Tree belajar aturan split yang interpretable, tetapi satu tree memiliki variance tinggi dan perlu regularization.

## Hubungan Konsep

- [[Random Forest and Gradient Boosting]]
- [[Overfitting and Underfitting]]
- [[Machine Learning Evaluation Metrics]]

## Checklist Pemahaman

- [ ] Bisa menjelaskan impurity
- [ ] Paham split bersifat greedy
- [ ] Bisa mengendalikan overfitting
- [ ] Paham keterbatasan feature importance

## Latihan

1. Hitung Gini untuk node dengan distribusi class 75:25.
2. Bandingkan tree depth 2 dan tanpa batas.
3. Visualisasikan rule tree kecil.

