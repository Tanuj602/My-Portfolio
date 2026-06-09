# 👁️ Diabetic Retinopathy & Macular Edema CNN Classifier

> 3-class retinal image classification for Diabetic Retinopathy (DR) and Diabetic Macular Edema (DME) detection using a custom 7-layer CNN — addressing the growing shortage of ophthalmologists through AI-assisted screening.

**Research Intern | Luddy School of Informatics, IUPUI | May–Aug 2023**

---

## 📋 Project Overview

Built a deep learning classifier on the **MESSIDOR-2** dataset (1,200 retinal fundus images) to detect three conditions:

| Class | Label | Count |
|---|---|---|
| No Diabetic Retinopathy | 0 | 546 |
| DR only (no DME) | 1 | 428 |
| DR + Diabetic Macular Edema | 2 | 226 |

**Best result: 60% test accuracy — outperformed all 5 traditional ML baselines**

---

## 🏗️ CNN Architecture

```
Input (retinal fundus image)
    ↓
Conv1: 64 filters + BatchNorm + MaxPool2D
    ↓
Conv2: 128 filters + BatchNorm + MaxPool2D
    ↓
Conv3: 256 filters + BatchNorm + MaxPool2D
    ↓
Conv4: 512 filters + BatchNorm + MaxPool2D
    ↓
Conv5: 1,024 filters
    ↓
Conv6: 2,048 filters
    ↓
Conv7: 4,096 filters
    ↓
Flatten → Dense → Dropout → Softmax(3)
```

---

## 📊 Model Comparison

| Model | Test Accuracy | Notes |
|---|---|---|
| **CNN (7-layer)** | **60.0%** | ✅ Best |
| Random Forest | 57.5% | Loses spatial info |
| SVM | 55.0% | — |
| KNN (k=40) | 55.0% | — |
| XGBoost | 53.75% | — |
| Logistic Regression | 52.5% | — |

**Average Precision: 0.5676 | Average Recall: 0.6330**

---

## 🔬 Technical Details

- **Dataset:** MESSIDOR-2 (publicly available retinal fundus images)
- **Split:** 1,000 train / 200 test
- **Data augmentation:** Random horizontal flips + rotations — addresses class imbalance for DME class (only 226 images)
- **Dropout layers:** Prevent overfitting on limited medical imaging dataset
- **Framework:** Keras (TensorFlow backend)
- **Why CNN over RF/SVM:** Spatial features (vessel patterns, lesion locations, exudate shapes) are precisely what differentiates disease stages — flat feature vectors lose all spatial information

---

## 📁 Repository Structure

```
diabetic-retinopathy-cnn/
├── model.py             # CNN architecture definition
├── train.py             # Training pipeline with augmentation
├── evaluate.py          # Test accuracy, confusion matrix, P/R
├── baseline_models.py   # RF, SVM, KNN, XGBoost, LogReg comparison
├── data_loader.py       # MESSIDOR-2 loading and preprocessing
├── requirements.txt
└── README.md
```

---

## 🛠️ Tech Stack

`Python` `TensorFlow` `Keras` `Scikit-Learn` `OpenCV` `NumPy` `Matplotlib` `Seaborn`

---

*Part of undergraduate research at Indiana University's Luddy School of Informatics, IUPUI.*
