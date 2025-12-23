# CS4602ML-FinalProject: TSMC (2330) Stock Movement Prediction
# 📈 Stacking Ensemble Architecture for Financial Forecasting

This project implements a robust machine learning pipeline to predict the price movement (Up/Down) of TSMC (2330). By leveraging a multi-model **Stacking Ensemble** approach, we combine the strengths of distance-based algorithms, tree-based models, and deep learning architectures to capture complex market dynamics.

---

## 🚪 Quick Navigation (傳送門)

| Resource | Description | Link |
| :--- | :--- | :--- |
| **🌐 Website Gallery** | Curated financial data sources & research sites | [🔗 View Sites](./website.md) |
| **📁 Data Repository** | Raw and processed datasets for 2330 forecasting | [🔗 Explore Data](./data/) |
| **📍 CheckPoints** | Project milestones and phase-wise reports | [🔗 CheckPoints](./checkpoint/) |
| **📊 Result Hub** | Exported prediction probabilities| [🔗 View Results](./data/results/) |

---

## 🗂️ Project Structure

The repository is organized into four core functional domains:

```text
.
├── code/
│   ├── backtest/            # Trading strategy & financial performance evaluation
│   │   ├── backtest6.py     # Core backtesting engine
│   │   └── README.md        # Performance metrics (Sharpe, MDD, Returns)
│   ├── models/              # Base Learners (The Model Zoo)
│   │   ├── KNN/             # K-Nearest Neighbors (Cluster-Augmented)
│   │   ├── LSTM/            # Long Short-Term Memory (Temporal Dependency)
│   │   ├── RandomForest/    # Ensemble Tree-based model
│   │   ├── XGBoost/         # Gradient Boosting Machine
│   │   ├── NN/              # Multi-layer Perceptron (MLP)
│   │   └── NaiveBayes/      # Statistical baseline
│   ├── preprocess/          # ETL & Feature Engineering pipeline
│   │   ├── data_download.ipynb
│   │   └── preprocess.ipynb # Multi-dimensional indicator generation
│   └── stack/               # Meta-learner for final ensemble aggregation
│       └── stacking.ipynb   # Meta-feature fusion & final prediction
├── data/
│   ├── processed/           # Standardized features (Post-ETL)
│   └── raw/                 # Source signals (2330, ADR, Global Indices)
└── results/                 # Probabilistic Meta-features for Stacking
