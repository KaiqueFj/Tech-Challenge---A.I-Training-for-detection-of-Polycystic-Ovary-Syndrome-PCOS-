# 🩺 Polycystic Ovary Syndrome (PCOS) Detection

### Tech Challenge — Phase 1 | Pós Tech IADT

![Python](https://img.shields.io/badge/Python-3.8+-blue?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.x-f7931e?logo=scikitlearn&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-✓-green)
![SHAP](https://img.shields.io/badge/SHAP-Explainability-purple)

---

## 📌 Context

Polycystic Ovary Syndrome (PCOS) is one of the most common endocrine disorders in women of reproductive age, affecting between **6% and 12%** of this population. Early diagnosis is essential, but can be complex due to the multiple clinical, laboratory, and ultrasound criteria involved.

This project develops a **Machine Learning** system to assist healthcare professionals in identifying patients with PCOS, acting as a decision-support tool for diagnosis.

---

## 📂 Dataset

**Source**: [Polycystic Ovary Syndrome (PCOS) — Kaggle](https://www.kaggle.com/datasets/prasoonkottarathil/polycystic-ovary-syndrome-pcos)

| Property           | Value                                                   |
| ------------------ | ------------------------------------------------------- |
| File               | `PCOS_data_without_infertility.xlsx` (sheet `Full_new`) |
| Records            | 541 patients                                            |
| Original features  | 45 columns                                              |
| Target             | `PCOS (Y/N)` — binary (0 = no PCOS, 1 = PCOS)           |
| Class distribution | 67.3% negative / 32.7% positive                         |

> ⚠️ **Download the dataset from Kaggle and place the `.xlsx` file in the same folder as the notebook before running.**

---

## 🗂️ Project Structure

```
.
├── pcos_classification.ipynb   # Main notebook
├── README.md                   # This file
└── fig_*.png                   # Charts generated automatically on execution
```

---

## ⚙️ Installation

```bash
pip install -r requirements.txt
```

---

## 🔬 Project Pipeline

### 1. EDA (Exploratory Data Analysis)

- Target variable distribution
- Numerical feature histograms by diagnosis
- Binary symptom analysis (weight gain, hirsutism, etc.)
- Comparative boxplots between groups

### 2. Preprocessing

- Removal of irrelevant columns (`Sl. No`, `Patient File No.`)
- Conversion of object columns to numeric (`AMH`, `II beta-HCG`)
- Missing value imputation using **median**
- **Feature selection** by correlation with target (threshold: |r| ≥ 0.1)
- Normalization with `StandardScaler` (for Logistic Regression and KNN)
- Stratified 80/20 train/test split

### 3. Modeling

Five algorithms evaluated with **stratified cross-validation (5-fold)**:

| Model               | Characteristic                                       |
| ------------------- | ---------------------------------------------------- |
| Logistic Regression | Linear baseline, interpretable                       |
| KNN                 | Instance-based, local non-linear patterns            |
| Decision Tree       | Explicit rules, high interpretability                |
| Random Forest       | Ensemble, reduces overfitting                        |
| XGBoost             | Gradient boosting, state-of-the-art for tabular data |

### 4. Hyperparameter Optimization

A RandomizedSearchCV-based optimization was performed on the XGBoost model using 5-fold stratified cross-validation.

The optimization focused on maximizing Recall, a critical metric in medical screening scenarios where false negatives have a higher clinical impact.

The search space included:

- n_estimators
- max_depth
- learning_rate
- subsample
- colsample_bytree
- min_child_weight
- gamma
- scale_pos_weight

### 5. Explainability

- **Feature Importance** via Random Forest (Gini)
- **SHAP Values** via XGBoost (global and local)

---

## ⚙️ Hyperparameter Optimization

An additional optimization phase was conducted using RandomizedSearchCV and 5-fold stratified cross-validation.

The XGBoost model was selected for optimization due to its strong performance in tabular classification tasks.

Class imbalance was addressed through the use of the scale_pos_weight parameter.

The optimization objective was Recall, prioritizing the identification of patients with PCOS.

## 📊 Results

### Best Model

The Logistic Regression model achieved the best balance between Recall, F1-Score and interpretability.

| Metric    | Value  |
| --------- | ------ |
| Accuracy  | 91.74% |
| Precision | 88.57% |
| Recall    | 86.11% |
| F1-Score  | 87.32% |
| AUC-ROC   | 95.02% |

Given the clinical context of PCOS screening, Recall was considered one of the most important metrics because false negatives may delay diagnosis and treatment.

### Cross-Validation (5-fold, training set)

| Model               | Accuracy | F1-Score | Recall |
| ------------------- | -------- | -------- | ------ |
| Random Forest       | 0.8983   | 0.8319   | 0.7741 |
| XGBoost             | 0.8729   | 0.8080   | 0.8020 |
| Logistic Regression | 0.8682   | 0.7958   | 0.7874 |
| KNN                 | 0.8565   | 0.7460   | 0.6527 |
| Decision Tree       | 0.7895   | 0.6858   | 0.6943 |

---

## 🧠 Top Predictive Features (SHAP)

1. **Follicle No. (L) and (R)** — ovarian follicle count (core clinical criterion)
2. **Skin darkening** — acanthosis nigricans, sign of insulin resistance
3. **Hair growth** — hirsutism, sign of hyperandrogenism
4. **Weight gain** — associated with the metabolic profile of PCOS
5. **AMH (ng/mL)** — elevated anti-Müllerian hormone is a PCOS marker
6. **Cycle (R/I)** — menstrual irregularity is a diagnostic criterion

---

## ⚠️ Limitations & Considerations

- The model is a **decision-support tool** and does not replace medical diagnosis
- Rotterdam criteria remain the clinical gold standard
- Single-institution dataset (possible selection bias)
- Moderate sample size (541 patients)

---

## 👤 Author

Developed as Tech Challenge — Phase 1 of **Pós Tech IADT** (Artificial Intelligence for Developers and Decision Makers).
