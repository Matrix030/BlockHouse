
# Order Flow Imbalance (OFI) Feature Engineering

This repository implements four key components of Order Flow Imbalance (OFI) based on the research paper **"Cross-Impact of Order Flow Imbalance in Equity Markets"**. These features are critical for modeling short-term price impacts using limit order book data across different depth levels and assets.

## 📁 Repository Structure

```

├── best\_level\_OFI.ipynb         # Computes OFI using only the best bid and ask levels
├── multi\_level\_OFI.ipynb        # Extends OFI computation to multiple depth levels
├── integrated\_level\_OFI.ipynb   # Aggregates OFI across multiple depths into a single feature
├── cross\_asset\_OFI.ipynb        # Computes cross-asset OFI interactions using LASSO regression

````

---

## 📊 Objective

The goal is to extract interpretable and high-frequency features from the order book that reflect imbalances in market pressure (buy vs. sell) and can be predictive of short-term price movements.

---

## 📘 Notebook Descriptions

### 1. `best_level_OFI.ipynb`
- **Purpose**: Calculates the OFI at the best bid and ask levels.
- **Logic**: Compares changes in order book volumes at level 1 over time.
- **Output**: A single OFI value per timestamp capturing market pressure at the top of the book.

### 2. `multi_level_OFI.ipynb`
- **Purpose**: Extends the OFI calculation to levels beyond the best bid/ask (e.g., depth levels 2, 3,..., N).
- **Logic**: Iteratively applies the OFI formula at each level and aggregates them.
- **Output**: One OFI feature per depth level per timestamp.

### 3. `integrated_level_OFI.ipynb`
- **Purpose**: Computes a single OFI value by integrating the influence across all visible depth levels.
- **Logic**: Weighted or unweighted sum of multi-level OFI values.
- **Output**: A consolidated OFI metric that captures deeper market intent.

### 4. `cross_asset_OFI.ipynb`
- **Purpose**: Quantifies the cross-impact of OFI from one asset on another using Lasso regression.
- **Logic**: Regresses asset A’s return on OFI of asset B (and vice versa) using L1-regularization.
- **Output**: Coefficients representing cross-impact magnitudes and directions.

---

## ⚙️ Requirements

- Python 3.8+
- Jupyter Notebook
- pandas, numpy, sklearn, matplotlib, seaborn

Install dependencies:
```bash
pip install -r requirements.txt
````

---

## 📄 References

* Mastromatteo, I., Tóth, B., & Bouchaud, J.-P. (2017). *Cross-impact and no-dynamic-arbitrage*. Market Microstructure and Liquidity.

