---
type: concept
status: not-started
domain: machine-learning
topic: ensemble-learning
level: intermediate
created: 2026-07-29
updated: 2026-07-29
---

# Random Forest and Gradient Boosting

## Tujuan

Memahami dua keluarga ensemble tree: bagging untuk mengurangi variance dan boosting untuk mengurangi error secara bertahap.

## Prasyarat

- [[Decision Tree]]
- [[Overfitting and Underfitting]]

## Ensemble Learning

Ensemble menggabungkan banyak weak atau unstable model agar prediction lebih kuat.

## Random Forest

Random Forest melatih banyak tree pada bootstrap sample dan subset fitur acak.

```text
Dataset
├── bootstrap 1 → Tree 1
├── bootstrap 2 → Tree 2
└── bootstrap B → Tree B
                      ↓
               vote / average
```

Randomness mengurangi correlation antar-tree. Averaging menurunkan variance.

## Gradient Boosting

Boosting membuat model secara sequential. Setiap learner baru berusaha memperbaiki residual atau negative gradient model sebelumnya:

$$
F_m(\mathbf{x})
=
F_{m-1}(\mathbf{x})
+
\eta h_m(\mathbf{x})
$$

- $h_m$: tree baru
- $\eta$: learning rate

## Perbandingan

| Aspek | Random Forest | Gradient Boosting |
|---|---|---|
| Training | dapat paralel | sequential |
| Fokus | mengurangi variance | memperbaiki error bertahap |
| Tuning | relatif mudah | lebih sensitif |
| Overfitting | relatif tahan | perlu regularization hati-hati |

## Implementasi

```python
from sklearn.ensemble import (
    HistGradientBoostingClassifier,
    RandomForestClassifier,
)

forest = RandomForestClassifier(
    n_estimators=300,
    min_samples_leaf=5,
    class_weight="balanced",
    n_jobs=-1,
    random_state=42,
)

boosting = HistGradientBoostingClassifier(
    learning_rate=0.05,
    max_iter=300,
    max_leaf_nodes=31,
    random_state=42,
)
```

## Hyperparameter Penting

Random Forest:

- jumlah tree
- `max_features`
- depth/minimum leaf
- class weighting

Boosting:

- learning rate
- jumlah iteration/tree
- tree complexity
- subsampling dan regularization

## Kompleksitas

Biaya bertambah terhadap jumlah tree. Forest memerlukan memory lebih besar tetapi training mudah diparalelkan; boosting bergantung sequential updates.

## Best Practice

- Gunakan tree ensemble sebagai baseline kuat data tabular.
- Validasi probability calibration.
- Gunakan early stopping untuk boosting.
- Analisis permutation importance atau SHAP dengan hati-hati.

## Kesalahan Umum

- Menganggap lebih banyak tree selalu memperbaiki generalization.
- Membandingkan model dengan preprocessing/data split berbeda.
- Menganggap feature importance sebagai causal importance.
- Tuning pada test set.

## Ringkasan

Random Forest merata-ratakan tree acak untuk stabilitas. Gradient Boosting menambah tree secara bertahap untuk memperbaiki error.

## Hubungan Konsep

- [[Decision Tree]]
- [[Cross-Validation and Model Selection]]
- [[Machine Learning Evaluation Metrics]]

## Checklist Pemahaman

- [ ] Bisa membedakan bagging dan boosting
- [ ] Paham bootstrap dan random feature subset
- [ ] Paham peran learning rate pada boosting
- [ ] Tahu risiko interpretasi importance

## Latihan

1. Bandingkan single tree dan Random Forest.
2. Plot validation score terhadap jumlah boosting iteration.
3. Bandingkan waktu training dan inference kedua model.

