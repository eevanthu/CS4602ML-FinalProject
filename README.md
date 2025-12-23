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

🚀 Key Technical Features
1. Multi-dimensional Feature Engineering
We integrate 100+ technical indicators, macro-market data, and interdependency signals:

Technical: MA, RSI, MACD, K/D, Bollinger Bands.

Global Macro: S&P 500, NASDAQ, SOX (Philly Semiconductor), DJI.

Cross-Market: TSMC ADR price movements and VIX volatility.

2. Advanced Preprocessing Pipeline
Temporal Alignment: Corrects for U.S./Taiwan market time differences (Shift-1) to ensure Zero Data Leakage.

Cluster Augmentation: Using K-Means to identify market regimes, providing the KNN model with logical environment anchors.

Smart Imputation: Categorical handling of missing values (Binary vs. Continuous) to maintain time-series integrity.

3. Stacking Ensemble Mechanism
Base Learners: A diverse "Zoo" of models trained to capture different market anomalies.

Rolling Forecast: Models are trained annually (2020–2024) to generate out-of-sample predictions.

Meta-Learner: The final Stacking layer learns how to dynamically weight each model's probability output based on historical accuracy.

🛠️ Getting Started
Installation
Each module is designed with self-contained dependencies. It is recommended to use a virtual environment:

# 1. Clone the repository
git clone [https://github.com/your-repo/CS4602ML-FinalProject.git](https://github.com/your-repo/CS4602ML-FinalProject.git)

# 2. Setup environment for a specific module (e.g., KNN)
cd code/models/KNN
pip install -r requirements.txt

Execution Order
Preprocessing: Run code/preprocess/preprocess.ipynb to build the feature matrix.

Base Training: Execute notebooks in code/models/ to populate the results/ folder.

Ensemble: Run code/stack/stacking.ipynb to fuse base model predictions.

Evaluation: Use code/backtest/backtest6.py to generate the final investment report.

📈 Performance Summary
Results are updated per milestone in the CheckPoint folder.

Disclaimer: This project is for educational purposes only. Financial markets involve high risk; predictions are based on historical patterns and do not guarantee future performance.
