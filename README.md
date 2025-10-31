# DA5401 — Assignment 7

- **Name**: Soumya Ranjan Patel  
- **Roll Number**: DA25M029




## Repository Structure

```
DA5401-assignment-7-patelsoumyaranjan04/
├── DA5401_A7.ipynb
└──sat.trn
└──sat.tst
└── README.md 
```

## ⚙️ Setup Instructions

### 🔧 Install dependencies
```
pip install numpy pandas scikit-learn matplotlib xgboost joblib

```

## 📊 Dataset Description

- **Source:** [UCI Statlog (Landsat Satellite)](https://archive.ics.uci.edu/ml/datasets/Statlog+(Landsat+Satellite))
- **Features:** 36 spectral bands per pixel patch  
- **Classes:** 6 land-cover categories (labels {1,2,3,4,5,7})  
- **Removed:** Class 6 (“all types present”) has no data  
- **Shape:** 6,435 samples × 36 features after merging train/test files


---

## ⚙️ Models Implemented

| Category | Model | Library | Expected Behavior |
|:--|:--|:--|:--|
| Baseline | DummyClassifier (`strategy='prior'`) | `sklearn.dummy` | Random baseline |
| Linear | Logistic Regression | `sklearn.linear_model` | Linear benchmark |
| Probabilistic | Gaussian Naive Bayes | `sklearn.naive_bayes` | Simplistic assumption baseline |
| Non-parametric | K-Nearest Neighbors | `sklearn.neighbors` | Moderate/good |
| Tree | Decision Tree | `sklearn.tree` | Moderate |
| Kernel | SVC (`probability=True`) | `sklearn.svm` | Strong nonlinear |
| Ensemble ⭐ | RandomForest | `sklearn.ensemble` | Strong generalization |
| Gradient Boosting ⭐ | XGBoost | `xgboost` | Best performer |
| Inverted (Bad) | Poor_ReversedTree | Custom wrapper | Intentionally AUC < 0.5 |

---

## 🧮 Evaluation Metrics

| Metric | Description |
|:--|:--|
| **Accuracy** | Fraction of correct predictions |
| **Weighted F1** | Harmonic mean of precision and recall (weighted by class support) |
| **Macro-AUC** | Mean of per-class AUCs (equal weight to each class) |
| **Macro-AP** | Mean of per-class Average Precision scores |

---

## 📈 Results Summary

| Model | Accuracy | Weighted F1 | Macro-AUC | Macro-AP |
|:--|--:|--:|--:|--:|
| **XGBoost** | **0.919** | **0.917** | **0.989** | **0.946** |
| **RandomForest** | 0.912 | 0.909 | 0.987 | 0.939 |
| KNN | 0.912 | 0.910 | 0.980 | 0.922 |
| SVC | 0.893 | 0.891 | 0.980 | 0.900 |
| LogisticRegression | 0.832 | 0.806 | 0.953 | 0.810 |
| GaussianNB | 0.783 | 0.790 | 0.948 | 0.786 |
| DecisionTree | 0.845 | 0.847 | 0.894 | 0.722 |
| DummyPrior | 0.239 | 0.092 | 0.500 | 0.167 |
| Poor_ReversedTree | 0.853 | 0.852 | **0.058** | 0.123 |

---

## 📉 ROC Curve — Macro-Averaged (One-vs-Rest)

<img width="1006" height="623" alt="image" src="https://github.com/user-attachments/assets/93e86742-ce57-4215-b3ed-df086e0b4628" />


> **Interpretation:**  
> - Curves close to the top-left indicate strong class separability.  
> - **XGBoost (AUC ≈ 0.989)** and **RandomForest (0.987)** dominate.  
> - **KNN and SVC** follow closely (AUC ≈ 0.98).  
> - **DecisionTree** lags behind (AUC ≈ 0.89).  
> - **Poor_ReversedTree (AUC ≈ 0.06)** performs worse than random, confirming the inversion logic.  

---

## 📈 Precision–Recall Curve — Macro-Averaged (One-vs-Rest)

<img width="791" height="496" alt="image" src="https://github.com/user-attachments/assets/a4b9846d-d2f7-4b14-bed7-15cbde7cb927" />


> **Interpretation:**  
> - **XGBoost (AP ≈ 0.946)** and **RandomForest (0.939)** maintain high precision at broad recall.  
> - **KNN and SVC** remain stable across thresholds.  
> - **DummyPrior** and **Poor_ReversedTree** show near-random behavior with early precision collapse.  
> - Confirms PRC as the more sensitive metric for imbalanced or skewed class distributions.

---

## 🧠 Key Insights

### ROC Analysis
- **AUC < 0.5** (as in Poor_ReversedTree) means score inversion — higher scores assigned to negatives.  
- DummyPrior sits on the random diagonal (AUC = 0.5).  
- Ensemble models maintain strong discrimination across all thresholds.

### PRC Analysis
- Shows clearer distinction for class imbalance cases.  
- Top models keep precision above 0.9 even at high recall.  
- Poor models collapse as recall increases — confirming weak calibration or random predictions.

---

## 🏁 Final Recommendation

| Model | Verdict | Justification |
|:--|:--|:--|
| **XGBoost** | ✅ **Best Overall** | Highest AUC (0.989) & AP (0.946); excellent across all metrics |
| **RandomForest** | 🟩 **Runner-up** | Near-identical performance; simpler and more interpretable |
| **KNN / SVC** | ⚙️ **Good backups** | Strong but slightly weaker under recall-heavy thresholds |
| **Logistic / NB / Tree** | ⚪ **Mediocre** | Linear or shallow tree limits boundary complexity |
| **Dummy / ReversedTree** | ❌ **Discard** | Random or inverted scoring |

---



