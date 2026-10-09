# Data Directory

> ⚠️ **Raw CSV datasets are NOT tracked by Git** (they exceed GitHub file size limits). Download them locally using the Kaggle CLI instructions below.

---

## 🛠️ Kaggle API Setup

1. Log into your Kaggle account and navigate to [Kaggle Account Settings](https://www.kaggle.com/settings).
2. Scroll to the **API** section and click **Create New Token**. This will download `kaggle.json`.
3. Place `kaggle.json` in your home directory:
   - **Windows**: `C:\Users\<Your-Username>\.kaggle\kaggle.json`
   - **Linux/macOS**: `~/.kaggle/kaggle.json`

---

## 📥 Dataset Download Commands

Run the following commands from the root directory of this repository:

```bash
# 1. PaySim1 Synthetic Mobile Money Dataset
kaggle datasets download ealaxi/paysim1 -p data/paysim1 --unzip

# 2. Online Payments Fraud Detection Dataset
kaggle datasets download rupakroy/online-payments-fraud-detection-dataset -p data/online-payments --unzip

# 3. UPI Payment Transactions India Dataset
kaggle datasets download kunalgupta2616/upi-payment-transactions-india -p data/upi-india --unzip
```

---

## 📊 Dataset Metadata & References

| Dataset Name | Source | Description |
|---|---|---|
| **PaySim1** | [Kaggle - PaySim1](https://www.kaggle.com/datasets/ealaxi/paysim1) | Synthetic mobile money transaction log simulated from financial records. |
| **Online Payments Fraud** | [Kaggle - Online Payments](https://www.kaggle.com/datasets/rupakroy/online-payments-fraud-detection-dataset) | Financial transaction dataset focused on payment fraud classification. |
| **UPI India** | [Kaggle - UPI India](https://www.kaggle.com/datasets/kunalgupta2616/upi-payment-transactions-india) | Transaction log of Unified Payments Interface (UPI) transactions in India. |
