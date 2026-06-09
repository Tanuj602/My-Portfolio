# 🧠 Parkinson's Cognitive Assessment ML Pipeline

> Predicting Montreal Cognitive Assessment (MoCA) scores from wearable accelerometer data using machine learning — enabling remote, continuous cognitive monitoring for Parkinson's patients.

**Research Intern | Luddy School of Informatics, IU Indianapolis | May–Jul 2024**
**Supervisors: Prof. Hee Tae Jung · Dr. Sarath Janga**

---

## 📋 Project Overview

This project builds an end-to-end ML regression pipeline to predict **MoCA scores** (0–30 scale, clinical measure of cognitive function) from wearable sensor accelerometer data. The clinical goal: allow Parkinson's patients to be monitored continuously at home rather than requiring infrequent, expensive clinic visits.

**Key result: Best RMSE of 2.17** — clinically meaningful (MoCA impairment threshold is 26/30, so ±2 points is acceptable for screening)

---

## 🏗️ Pipeline Architecture

```
Raw Accelerometer Data (wearable sensor)
        ↓
TSFEL Feature Extraction (5-minute windows)
        ↓
Data Cleaning + Missing Value Handling
        ↓
PCA Dimensionality Reduction
        ↓
Ridge / ElasticNet Regularization
        ↓
Leave-One-Out Cross Validation
        ↓
MoCA Score Prediction (RMSE 2.17)
```

---

## 📊 Model Benchmark Results

| Model | RMSE | Notes |
|---|---|---|
| **Ridge** | **2.177** | ✅ Best performer |
| **ElasticNet** | **2.177** | ✅ Best performer |
| Linear Regression | 2.18 | Good baseline |
| Random Forest | 2.28 | Overfitting on small N |
| Gradient Boosting | 2.25 | — |
| AdaBoost | 2.30 | — |
| XGBoost | 2.29 | — |

**Why Ridge/ElasticNet won:** Clinical accelerometer data is high-dimensional with many correlated movement features. L2/combined regularization shrinks redundant coefficients toward zero — the correct inductive bias for this data structure. Tree-based models overfit on small clinical datasets.

---

## 🔬 Technical Details

- **Feature extraction:** TSFEL (Time Series Feature Extraction Library) on 5-minute accelerometer windows
- **Dimensionality reduction:** PCA — confirmed hypothesis that PCA reduces variability
- **Validation:** Leave-One-Out Cross-Validation (LOO-CV) — maximizes every patient sample
- **Regularization:** Ridge (L2) and ElasticNet (L1+L2) to prevent overfitting
- **Key bug fixed:** Identified that StandardScaler was converting MoCA target scores to z-scores rather than actual scores — traced systematically, corrected pipeline

---

## 📁 Repository Structure

```
parkinsons-moca-prediction/
├── pipeline.py          # Main ML pipeline
├── features.py          # TSFEL feature extraction
├── models.py            # All 7 model implementations
├── evaluate.py          # RMSE evaluation and LOO-CV
├── requirements.txt
└── README.md
```

---

## 🛠️ Tech Stack

`Python` `Scikit-Learn` `XGBoost` `TSFEL` `PCA` `Ridge` `ElasticNet` `Pandas` `NumPy` `Matplotlib`

---

## 📈 Clinical Context

- MoCA score range: 0–30
- Impairment threshold: ≤26/30
- RMSE 2.17 means predictions are off by ~2 points on average
- Clinically acceptable for **screening** — good enough to flag patients for follow-up

---

*Part of undergraduate research at Indiana University's Luddy School of Informatics, BioHealth Informatics Department.*
