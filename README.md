# 🧬 JAK2 Inhibitor Discovery Pipeline

**From Bioactivity Data Mining to Machine Learning–Ready Dataset**

---

## 📌 Project Overview

This project implements a **reproducible, end-to-end computational drug discovery pipeline** targeting **Janus Kinase 2 (JAK2)**. The workflow is designed to bridge **raw bioactivity data acquisition** and **machine learning (ML)-ready dataset construction**, enabling downstream predictive modeling for inhibitor discovery.

The entire pipeline is contained in:

```bash
full_script.ipynb
```

Unlike fragmented workflows, this notebook integrates:

* Data retrieval from ChEMBL
* Systematic preprocessing and curation
* Bioactivity transformation and labeling
* Exploratory data analysis (EDA)
* Preparation for **supervised machine learning (QSAR modeling)**

---

## 🎯 Objectives

1. Retrieve **experimentally validated JAK2 bioactivity data**
2. Standardize and clean molecular and activity data
3. Convert IC50 values into **quantitative (pIC50)** and **categorical labels**
4. Analyze dataset structure and quality
5. Produce a **high-quality dataset suitable for ML model training**

---

## 🧪 Biological Context

* **Target:** Janus Kinase 2 (JAK2)
* **Role:** Critical in cytokine signaling and hematopoiesis
* **Clinical relevance:** Dysregulation linked to:

  * Myeloproliferative disorders
  * Inflammatory diseases
* **Therapeutic strategy:** Small-molecule kinase inhibitors

---

## ⚙️ Pipeline Architecture

---

### 🔹 1. Data Retrieval (ChEMBL API)

Bioactivity data is programmatically extracted using the `chembl_webresource_client`.

**Query criteria:**

* Target: JAK2
* Assay type: Binding / functional assays
* Activity type: IC50

**Raw dataset contents:**

* `molecule_chembl_id`
* `canonical_smiles`
* `standard_value` (IC50)
* `standard_units`
* `assay_type`

**Output file:**

```bash
bioactivity_raw_data.csv
```

---

### 🔹 2. Data Cleaning & Standardization

To ensure reliability and reproducibility, the following filters are applied:

#### ✔️ Data Integrity Filtering

* Remove entries with:

  * Missing IC50 values
  * Missing or invalid SMILES
* Drop duplicate compounds

#### ✔️ Unit Normalization

* Convert IC50 values to a **consistent unit (nM)**
* Remove inconsistent or ambiguous measurements

#### ✔️ Log Transformation

IC50 values are transformed into **pIC50**:

[
pIC50 = -\log_{10}(IC50 ; [M])
]

This step:

* Stabilizes variance
* Improves ML model performance
* Aligns with QSAR best practices

---

### 🔹 3. Bioactivity Classification

Compounds are categorized into discrete classes based on IC50 thresholds:

| Class        | IC50 Range    |
| ------------ | ------------- |
| Active       | ≤ 1000 nM     |
| Inactive     | ≥ 10000 nM    |
| Intermediate | 1000–10000 nM |

This produces:

* **Regression target:** pIC50
* **Classification labels:** Active / Inactive

---

### 🔹 4. Exploratory Data Analysis (EDA)

EDA is performed to validate dataset quality and uncover trends:

#### 📊 Distribution Analysis

* IC50 / pIC50 distributions
* Skewness and spread

#### 📊 Class Balance

* Proportion of active vs inactive compounds

#### 📊 Structural Insights

* Diversity of SMILES representations
* Detection of outliers

#### 📊 Data Quality Checks

* Missing values
* Duplicate molecules
* Range validation

---

## 🤖 Machine Learning Preparation

A key goal of this project is to **prepare the dataset for ML-based drug discovery**.

### ✔️ Dataset Outputs

```bash
bioactivity_preprocessed_data.csv
```

Contains:

* Clean SMILES
* pIC50 values (regression target)
* Activity class labels (classification target)

---

### ✔️ ML-Ready Features (Next Step)

Although not fully implemented in this notebook, the dataset is structured for:

#### Feature Engineering

* Molecular descriptors (e.g., RDKit)
* Fingerprints:

  * Morgan fingerprints
  * MACCS keys

#### Modeling Approaches

**Regression Models (Predict pIC50):**

* Linear Regression
* Random Forest Regressor
* Gradient Boosting
* Neural Networks

**Classification Models (Active vs Inactive):**

* Logistic Regression
* Support Vector Machines (SVM)
* Random Forest Classifier
* XGBoost

---

### ✔️ Evaluation Metrics

Planned evaluation strategies:

**Regression:**

* R² score
* RMSE

**Classification:**

* Accuracy
* Precision / Recall
* ROC-AUC

---

## 📊 Key Results

* Retrieved **~25,000+ bioactivity records** from ChEMBL

* Generated a **clean, standardized dataset**

* Successfully:

  * Normalized IC50 values
  * Created pIC50 transformation
  * Defined activity classes

* Produced a dataset suitable for:

  * QSAR modeling
  * Virtual screening
  * Drug discovery pipelines

---

## 🧰 Technologies Used

* **Python (Google Colab)**
* Core libraries:

  * `pandas`
  * `numpy`
  * `matplotlib`, `seaborn`
* Data source:

  * ChEMBL API (`chembl_webresource_client`)

---

## 📁 Repository Structure

```bash
├── full_script.ipynb
├── WorkingScript.ipynb
├── bioactivityJAK2_raw_data.csv
├── bioactivityJAK2_preprocessed_data.csv
├── README.md
```

---

## 🚀 How to Run

### ▶️ Google Colab

1. Upload notebook
2. Install dependencies (if prompted)
3. Run all cells sequentially

---

### ▶️ Local Environment

```bash
pip install pandas numpy matplotlib seaborn chembl_webresource_client
```

Run using Jupyter Notebook or JupyterLab.

---

## ⚠️ Limitations

* Bioactivity data derived from **heterogeneous assays**
* IC50 variability across experimental conditions
* No descriptor calculation included yet
* No ML models implemented (dataset preparation stage only)

---

## 🔮 Future Work

This project is designed as a **foundation for advanced computational drug discovery**:

### 🔬 Feature Engineering

* RDKit descriptor calculation
* Molecular fingerprint generation

### 🤖 Machine Learning

* QSAR model training
* Hyperparameter optimization
* Cross-validation pipelines

### 🧪 Advanced Modeling

* Deep learning (Graph Neural Networks)
* Structure-based docking simulations

### 🌍 Real-World Application

* Lead compound prioritization
* Drug repurposing workflows

---

## 📖 Reproducibility

* Fully script-based workflow (no manual edits)
* All intermediate datasets saved
* Notebook can be rerun end-to-end

---

## 👨‍🔬 Author

Mohamed
PharmD Student — Computational Drug Design | Bioinformatics

---

## 📜 License

For academic and research use.

---

## ⭐ Acknowledgements

* ChEMBL database for bioactivity data
* Open-source Python ecosystem
* Google Colab for computational environment

---

