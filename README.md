# 🧬 Novozymes Enzyme Stability Prediction  

> **CS 184A – Fall 2022 Final Project**  
> Predicting and ranking enzyme thermal stability using machine learning models.  

---

## 📌 Project Description  
Enzymes are widely used in **agriculture, health, nutrition, and cleaning**, but many are only marginally stable. Traditional methods for testing stability are costly and slow.  

This project explores **deep learning approaches (LSTM & Transformer Encoder)** to predict the **thermal stability (Tm)** of enzymes given their amino acid sequences and environmental pH. The focus is on **ranking stability** rather than absolute values.  

---

## 📊 Dataset  
- **Source:** [Kaggle: Novozymes Enzyme Stability Prediction](https://www.kaggle.com/competitions/novozymes-enzyme-stability-prediction)  
- **Train:** 31,398 samples (wild-type & engineered enzymes)  
- **Test:** 2,414 samples (single-point mutations, standardized pH = 8)  
- **Features:**  
  - Amino acid sequence (20 possible residues)  
  - pH (0–14)  
- **Target:** Enzyme thermostability (**Tm**)  

---

## ⚙️ Approach  

### 🔹 Preprocessing  
- Fixed errors in dataset (swapped `Tm`/`pH`).  
- Removed NaNs & excessively long sequences.  
- Encodings:  
  - **One-hot** for LSTM  
  - **Numeric embeddings** for Transformer  
- Grouped enzymes by **wild-type** for intra-group ranking.  

### 🔹 Models  
- **LSTM (Baseline):** Sequentially processes amino acids; concatenates pH; predicts Tm.  
- **Transformer Encoder:** Uses self-attention for long-range dependencies; positional encoding; mean pooling + pH concatenation.  

### 🔹 Loss Function  
- **MSE Loss:** Minimizes prediction error.  
- **Intra-group L1 Loss:** Enforces ranking correctness within enzyme groups.  
- **Final Loss = MSE + L1**  

---

## 🧪 Experiments  

- **Optimizer:** Adam (`lr=0.001`)  
- **Batch Size:** 16  
- **Epochs:** 15 (LSTM) / 8 (Transformer)  
- **Evaluation Metric:** Spearman’s Rank Correlation  

### 📈 Results  
| Model                        | Spearman (Test) |  
|-------------------------------|-----------------|  
| LSTM (1-layer, hidden=128)    | -0.038 |  
| LSTM (2-layer, hidden=128)    | -0.038 |  
| Transformer Encoder (1-layer) | -0.045 |  

⚠️ **Outcome:**  
- Validation performance looked reasonable, but test scores were poor due to **train/test distribution mismatch**.  
- Future improvements: add **3D structural features (AlphaFold, PDB, B-factors)**, use **transfer learning (ThermoNet)**, and integrate domain-specific libraries like **Rosetta**.  

---

## 👩‍💻 Contributors  
- **Calvin Yeung** – preprocessing, train/val split, training loop, LSTM/Transformer implementation  
- **Eesha Tur Razia Babar** – research, data exploration, visualization, LSTM experiments, report writing  
- **Po-Chu Hsu** – preprocessing, feature engineering, report writing  

---

## 📚 References  
- [DeepDDG](https://doi.org/10.1021/acs.jcim.8b00697)  
- [ThermoNet](https://github.com/gersteinlab/ThermoNet)  
- [Rosetta Commons](https://www.rosettacommons.org/software)  

---

🚀 *This repo demonstrates how machine learning can be applied to complex biological problems, while also highlighting the challenges of distribution shifts and feature engineering in bioinformatics.*  

