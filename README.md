# 🧬 JAK2 Inhibitor Discovery using QSAR & Machine Learning

**End-to-End Pipeline: Data Mining → Feature Engineering → Model Development → Validation**

---

## 📌 Project Overview

This project implements a **complete computational drug discovery pipeline** for identifying inhibitors of **Janus Kinase 2 (JAK2)** using **Quantitative Structure–Activity Relationship (QSAR) modeling and machine learning**.

Unlike basic preprocessing pipelines, this work includes:

* Bioactivity data retrieval from ChEMBL
* Rigorous data curation and preprocessing
* Molecular descriptor and fingerprint generation
* Statistical analysis and chemical space exploration
* **Supervised machine learning modeling (Random Forest + benchmarking)**
* **Model validation (Y-randomization)**
* **Model interpretability using SHAP**

The entire workflow is implemented in:

```bash
full_script.ipynb
```

---

## 🎯 Objectives

* Construct a **high-quality QSAR dataset** for JAK2 inhibitors
* Engineer molecular features using:

  * Lipinski descriptors
  * PubChem fingerprints (881 bits)
* Develop predictive models for **bioactivity (pIC50)**
* Validate model robustness using statistical techniques
* Identify key molecular features driving activity

---

## 🧪 Target Information

* **Protein:** JAK2 (Janus Kinase 2)
* **Organism:** Homo sapiens
* **Bioactivity metric:** IC50 → transformed to pIC50

---

## ⚙️ Pipeline Architecture

---

### 🔹 1. Data Retrieval (ChEMBL API)

* Queried ChEMBL for:

  * Target: JAK2
  * Activity type: IC50

* Raw dataset saved as:

```bash
bioactivity_raw_data.csv
```

---

### 🔹 2. Data Curation & Preprocessing

#### ✔️ Cleaning Steps

* Removed missing IC50 values
* Removed invalid or empty SMILES
* Filtered relevant bioactivity entries

#### ✔️ Initial Classification

* Active: ≤ 1000 nM
* Inactive: ≥ 10000 nM
* Intermediate: removed later

#### ✔️ Duplicate Handling

* Aggregated duplicate compounds using **median IC50 per SMILES**

---

### 🔹 3. Data Transformation

IC50 values converted to pIC50:

[
pIC50 = -\log_{10}(IC50 ; [M])
]

* Enables:

  * Better distribution
  * Improved ML performance

#### Final Labeling:

* Active: pIC50 ≥ 6
* Inactive: pIC50 < 6

---

### 🔹 4. Exploratory Data Analysis (EDA)

Performed:

* pIC50 distribution analysis
* Class balance visualization
* Chemical space exploration:

  * Molecular Weight vs LogP
* Boxplots for descriptor comparison

#### Statistical Testing

* **Mann–Whitney U test** applied to:

  * pIC50
  * MW
  * LogP
  * H-bond donors/acceptors

Results stored in:

```bash
mannwhitney_summary.csv
```

---

## 🧪 Feature Engineering

---

### 🔹 Lipinski Descriptors (RDKit)

Calculated:

* Molecular Weight (MW)
* LogP
* Hydrogen bond donors
* Hydrogen bond acceptors

---

### 🔹 Molecular Fingerprints

Using PaDEL:

* **PubChem fingerprints (881 features)**
* Additional substructure fingerprints

Output:

```bash
pubchem_fingerprints.csv
```

---

### 🔹 Final QSAR Dataset

Combined:

* Bioactivity data
* pIC50 values
* Fingerprints

Final dataset:

```bash
QSAR_dataset.csv
```

---

## 🤖 Machine Learning Modeling

---

### 🔹 Problem Type

* **Regression task:** Predict continuous pIC50 values

---

### 🔹 Feature Matrix

* Initial: 881 fingerprint features
* After variance filtering:

  * Removed low-information features
  * Reduced dimensionality

---

### 🔹 Model: Random Forest Regressor

* Algorithm: Ensemble learning
* Trees: 200 estimators
* Train/Test Split: 80/20

---

### 🔹 Model Performance

* **R² score (test set): ~0.76**

Interpretation:

* Indicates **strong predictive capability**
* Captures ~76% of variance in bioactivity

---

### 🔹 Visualization

* Predicted vs experimental pIC50 regression plot
* Confirms correlation and model fit

---

## 🔬 Model Validation

---

### 🔹 Y-Randomization Test

To ensure the model is not due to chance:

* Randomly shuffled target values
* Retrained model multiple times

**Results:**

* Randomized R² ≈ -0.21
* Actual R² ≈ 0.76

✔️ Conclusion:

* Model is **statistically valid**
* Not due to random correlations

---

## ⚖️ Model Benchmarking

---

### 🔹 LazyPredict Comparison

* Evaluated multiple regression models automatically
* Compared performance across algorithms

Outputs:

* Ranked models by R²
* Identified best-performing algorithms

---

## 🧠 Model Interpretability

---

### 🔹 SHAP Analysis

Used SHAP (SHapley Additive Explanations) to:

* Identify most important molecular features
* Understand contribution of fingerprints to predictions

Output:

* SHAP summary plot showing feature importance

---

## 📊 Key Results

* Built a **complete QSAR pipeline** from raw data to ML model
* Generated high-dimensional molecular feature space
* Achieved:

  * **R² ≈ 0.76 (Random Forest)**
* Validated model robustness via:

  * Y-randomization
* Identified important structural features using SHAP

---

## 🧰 Technologies Used

* Python (Google Colab)
* Libraries:

  * `pandas`, `numpy`
  * `matplotlib`, `seaborn`
  * `RDKit`
  * `padelpy`
  * `scikit-learn`
  * `LazyPredict`
  * `SHAP`
* Data source:

  * ChEMBL API

---

## 📁 Repository Structure

```bash
├── full_script.ipynb
├── WorkingScript.ipynb
├── bioactivity_raw_data.csv
├── bioactivityJAK2_preprocessed_data.csv
├── df_lipinski.csv
├── pubchem_fingerprints.csv
├── QSAR_dataset.csv
├── mannwhitney_summary.csv
├── EDA_results.zip
├── README.md
```

---

## 🚀 How to Run

### ▶️ Google Colab

1. Upload notebook
2. Install dependencies
3. Run all cells

---

### ▶️ Local Setup

```bash
pip install pandas numpy matplotlib seaborn scikit-learn rdkit padelpy shap lazypredict chembl_webresource_client
```

---

## ⚠️ Limitations

* Bioactivity data from heterogeneous experimental conditions
* No hyperparameter tuning performed
* Potential dataset imbalance
* External validation dataset not included

---

## 🔮 Future Work

* Hyperparameter optimization (GridSearch / Bayesian tuning)
* External validation dataset
* Deep learning models (GNNs)
* Molecular docking integration
* Multi-target prediction

---

## 👨‍🔬 Author

Mohamed
PharmD Student — Computational Drug Design & Bioinformatics

---

## 📜 License

MIT License

---

## ⭐ Acknowledgements

* ChEMBL database
* Open-source cheminformatics tools
* Google Colab

---

