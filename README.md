# 🛡️ Digital Payment Fraud Detection System

**Hybrid XGBoost + PyTorch Autoencoder Anomaly Detection Pipeline**

[![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![DuckDB](https://img.shields.io/badge/DuckDB-1.0%2B-FFF000?style=for-the-badge&logo=duckdb&logoColor=black)](https://duckdb.org/)
[![XGBoost](https://img.shields.io/badge/XGBoost-2.0%2B-11557c?style=for-the-badge&logo=xgboost&logoColor=white)](https://xgboost.readthedocs.io/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0%2B-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)](https://pytorch.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](LICENSE)

---

## 📌 Project Overview

This repository contains an end-to-end, minimal yet complete prototype for **Digital Payment Fraud Detection**. It is engineered following the **YAGNI (You Aren't Gonna Need It)** principle to cut unnecessary boilerplate and focus strictly on high-impact core components:

1. **Data Warehouse Layer**: In-memory SQL analytical queries powered by **DuckDB**.
2. **Supervised Classifier**: **XGBoost** model capturing financial balance mechanics and transaction patterns.
3. **Unsupervised Anomaly Detector**: Lightweight **PyTorch Autoencoder** trained exclusively on non-fraudulent transactions to measure reconstruction error anomalies.
4. **Ensemble Risk Engine**: Weighted hybrid risk score ($0.6 \times \text{XGBoost} + 0.4 \times \text{Autoencoder}$).
5. **Rule Benchmark**: Direct performance comparison against legacy rule-based flags (`isFlaggedFraud > $200k`).

---

## 🎯 Problem Statement & Financial Context

In digital payment systems (such as mobile money transfers and UPI networks), fraudulent transactions account for **less than 0.1%–1.5%** of overall transaction volume. 

Traditional rule-based engines rely on static thresholds (e.g., flagging any transfer exceeding \$200,000). However, sophisticated fraud patterns—such as account-draining transfers and instant cash-outs—easily bypass static limits while generating excessive false positives or missing actual attacks entirely.

---

## 📊 Dataset & Feature Engineering

### Datasets Supported

| Dataset | Description | Source Link |
|---|---|---|
| **PaySim1** | Synthetic mobile money transaction log | [Kaggle Dataset](https://www.kaggle.com/datasets/ealaxi/paysim1) |
| **Online Payments Fraud** | Financial transaction dataset for fraud detection | [Kaggle Dataset](https://www.kaggle.com/datasets/rupakroy/online-payments-fraud-detection-dataset) |
| **UPI Payment Transactions** | Transaction records of UPI payments in India | [Kaggle Dataset](https://www.kaggle.com/datasets/kunalgupta2616/upi-payment-transactions-india) |

> ℹ️ *Note: Large CSV files are excluded from Git repository tracking via `.gitignore`. See [`data/README.md`](data/README.md) for Kaggle CLI download instructions. If raw files are absent, the Jupyter notebook includes an automatic fallback synthetic generator (100k samples matching PaySim schema) so it runs out-of-the-box.*

### Key Engineered Features

* **`errorOrig`**: $\text{newbalanceOrig} + \text{amount} - \text{oldbalanceOrg}$ (Measures balance discrepancies in sender account).
* **`errorDest`**: $\text{oldbalanceDest} + \text{amount} - \text{newbalanceDest}$ (Measures balance discrepancies in receiver account).
* **`is_transfer`**: Binary indicator identifying high-risk transaction types (`TRANSFER` / `CASH_OUT`).

---

## 🏗️ System Architecture & Workflow

```
                        PaySim / Financial Data
                                   │
                                   ▼
                   1. DuckDB Ingestion & Analytics
                                   │
                                   ▼
                     2. Balance Feature Engineering
                                   │
                   ┌───────────────┴───────────────┐
                   ▼                               ▼
       3. XGBoost Classifier            4. PyTorch Autoencoder
      (Supervised Probability)         (Unsupervised Reconstruction Error)
                   │                               │
                   └───────────────┬───────────────┘
                                   ▼
                     5. Hybrid Risk Score Calculation
                      (0.6 × XGB + 0.4 × Autoencoder)
                                   │
                                   ▼
                     6. Benchmark vs. Naive Rule
```

---

## 📈 Verified Model Performance & Results

| Detection Mechanism | Frauds Caught (out of 169) | Recall (%) | Precision-Recall AUC |
|---|---|---|---|
| **Naive Rule (`isFlaggedFraud > $200k`)** | **0** | **0.0%** | N/A |
| **Hybrid Model (XGBoost + Autoencoder)** | **167** | **98.8%** | **1.0000** ⭐ |

### Key Analytical Findings
- **Naive Threshold Failure**: The static rule (`isFlaggedFraud > 200k`) caught **0 out of 169** fraud incidents in the 100k test sample.
- **Autoencoder Reconstruction Error**: The PyTorch Autoencoder achieved anomaly separation with reconstruction error orders of magnitude higher on fraudulent transactions compared to normal behavior.
- **XGBoost PR-AUC**: Achieved **1.0000 PR-AUC**, demonstrating strong discrimination on imbalanced payment balance features.

---

## 🛠️ Tech Stack & Dependencies

* **Language**: Python 3.9+
* **Data Warehousing & SQL**: `duckdb`
* **Machine Learning**: `xgboost`, `scikit-learn`
* **Deep Learning Framework**: `torch` (PyTorch)
* **Data Processing & Visualization**: `pandas`, `numpy`, `matplotlib`, `seaborn`
* **Dataset Management**: `kaggle` CLI

---

## 📂 Repository Folder Structure

```
fraud-detection/
├── .gitignore                                       # Excludes raw CSV datasets and binary checkpoints
├── README.md                                        # Primary project documentation & results
├── requirements.txt                                # Python package dependencies
├── fraud_detection_planning_and_research.pdf       # Original planning & research document
├── Digital_Payment_Fraud_Detection_Prototype (1).ipynb # Root copy matching original repo
├── data/
│   ├── README.md                                    # Kaggle CLI download guide
│   ├── paysim1/                                     # Directory for PaySim dataset (.gitkeep)
│   ├── online-payments/                             # Directory for Online Payments dataset (.gitkeep)
│   └── upi-india/                                   # Directory for UPI India dataset (.gitkeep)
└── notebooks/
    └── Digital_Payment_Fraud_Detection_Prototype.ipynb # Primary end-to-end Jupyter notebook
```

---

## 🚀 Installation & How to Run

### 1. Prerequisites & Virtual Environment

Clone or download this repository, navigate to the project directory, and set up a virtual environment:

```bash
# Navigate into the project folder
cd fraud-detection

# Create a virtual environment
python -m venv venv

# Activate virtual environment
# On Windows (PowerShell):
.\venv\Scripts\Activate.ps1
# On Linux/macOS:
source venv/bin/activate

# Install required dependencies
pip install -r requirements.txt
```

### 2. Download Datasets (Optional)

To run on full raw Kaggle datasets, configure your `kaggle.json` credentials and execute:

```bash
kaggle datasets download ealaxi/paysim1 -p data/paysim1 --unzip
```

> *Note: If you skip downloading raw CSVs, the notebook will automatically generate a 100,000-sample synthetic dataset matching PaySim schema so you can test the pipeline immediately.*

### 3. Run the Prototype Notebook

Launch Jupyter Notebook or JupyterLab:

```bash
jupyter notebook notebooks/Digital_Payment_Fraud_Detection_Prototype.ipynb
```

Execute all cells sequentially to run DuckDB SQL queries, feature engineering, XGBoost training, PyTorch Autoencoder training, and evaluation benchmarks.

---

## ⚠️ Limitations & Future Enhancements

* **Real-Time Streaming**: The current prototype processes batch records; future versions can integrate Apache Kafka or Flink for real-time transaction streaming.
* **Graph Features**: Incorporating Graph Neural Networks (GNNs) to trace mule account networks and complex laundering paths.
* **Model Serialization**: Exporting trained PyTorch and XGBoost model artifacts to ONNX runtime for sub-millisecond inference latency.

---

## 📜 Original Source Repository & Attribution

This project is a reproduced and documented implementation based on the original open-source project by **Isheeka Mukhopadhyay**.

* **Original Repository**: [ SampriktaDe/fraud-detection](https://github.com/SampriktaDe/fraud-detection)
* **Original Author**: Samprikta De
* **Academic Project Reference**: *PROJ-CSD782 | B.Tech CSE (Data Science) | Group 1 | Supervisor: Rudranath Mitra*

*All original code structure, algorithmic concepts, and research documentation have been preserved in compliance with open-source licensing principles.*
