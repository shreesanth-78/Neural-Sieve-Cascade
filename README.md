# Neural-Sieve-Cascade
Neural Sieve Cascade: Multi-sieve deep learning pipeline for malicious URL detection

The **Neural Sieve Cascade (NSC)** is a three-stage malicious URL detection framework designed to balance **speed, accuracy, and real-time feasibility**. Instead of sending every URL to heavy models, NSC filters URLs **progressively**.

| Sieve | Model Type | Purpose | URLs Handled (≈) |
|------|------------|---------|------------------|
| **Sieve-1** | Logistic Regression (TF-IDF) | Fast filtering of clear benign/malicious URLs | 75% |
| **Sieve-2** | CNN + LSTM + BiLSTM (Soft-Voting) | Handles structurally ambiguous / obfuscated URLs | 14% |
| **Sieve-3** | TinyBERT | Resolves the hardest adversarial/phishing cases | 11% |

![NSC workflow](images/nsc_workflow.png)

---

## 📦 Dataset

**Total URLs:** 651,191  
**Classes:** Benign, Defacement, Phishing, Malware

**Example:**

| url | label |
|-----|-------|
| `http://secure-login.bank.verify-pay.com` | phishing |
| `https://google.com` | benign |

Original source: Kaggle – *malicious-urls-dataset*.

---

## 🧠 Model Details

### Sieve-1: Logistic Regression
- TF-IDF on character n-grams  
- Ultra-fast inference; accepts predictions with **≥ 0.90 confidence**

### Sieve-2: Deep Learning Ensemble
- **CNN**: catches lexical tricks (e.g., `paypa1.com`)  
- **LSTM**: long-range token dependencies  
- **BiLSTM**: forward + backward context  
- Soft-voting; accepts with **≥ 0.90 confidence**

### Sieve-3: TinyBERT
- Handles adversarial/semantic manipulations  
- Tuned to prioritize recall for phishing & malware

---

## 📊 Results

![Accuracy comparison](images/accuracy_comparison.png)

| Model | Accuracy (%) |
|------|---------------|
| Logistic Regression | 99.0 |
| CNN | 93.45 |
| LSTM | 90.48 |
| BiLSTM | 89.93 |
| TinyBERT | 94.86 |
| **NSC (Final)** | **97.28** |

**Final Confusion Matrix**

![Confusion matrix](images/confusion_matrix.png)

**Per-class Metrics**

| Class | Precision (%) | Recall (%) | F1-Score (%) |
|-------|---------------|------------|--------------|
| Benign | 97 | 99 | 98 |
| Defacement | 99 | 99 | 99 |
| Malware | 99 | 93 | 96 |
| Phishing | 95 | 87 | 91 |

Additional plots:  
<p align="center">
  <img src="images/precision.png" width="300" alt="Precision">  
  <img src="images/recall.png" width="300" alt="Recall">  
  <img src="images/F1-score.png" width="300" alt="F1-score">
</p>

---

## 🚀 Quickstart

```bash
# 1) Create env (optional)
python -m venv .venv && .\.venv\Scripts\activate   # Windows PowerShell

# 2) Install
pip install -r requirements.txt

# 3) Run the notebook
jupyter notebook notebooks/NSC_Final.ipynb
