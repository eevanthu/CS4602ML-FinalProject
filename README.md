# CS4602ML-FinalProject: TSMC (2330) Stock Movement Prediction
# 📈 Stacking Ensemble Architecture for Financial Forecasting

This project implements a robust machine learning pipeline to predict the price movement (Up/Down) of TSMC (2330). By leveraging a multi-model **Stacking Ensemble** approach, we combine the strengths of distance-based algorithms, tree-based models, and deep learning architectures to capture complex market dynamics.

---

## 🚪 Quick Navigation

| Resource | Description | Link |
| :--- | :--- | :--- |
| **🌐 Website Gallery** | Curated financial data sources & research sites | [🔗 View Sites](./website.md) |
| **📁 Data Repository** | Raw and processed datasets for 2330 forecasting | [🔗 Explore Data](./data/) |
| **📍 CheckPoints** | Project milestones and phase-wise reports | [🔗 CheckPoints](./checkpoint/) |
| **📊 Result Hub** | Exported prediction probabilities (Meta-features) | [🔗 View Results](./data/results/) |

---

## 🗂️ Project Structure

The repository is organized into four core functional domains:

```text
.
├── code/
│   ├── backtest/            # Trading strategy & performance evaluation
│   │   ├── backtest6.py
│   │   ├── README.md
│   │   └── requirements.txt
│   ├── models/              # Individual base learners
│   │   ├── KNN/             # K-Nearest Neighbors (K-Sweep & Cluster-Augmented)
│   │   ├── LSTM/            # Long Short-Term Memory (Deep Learning)
│   │   ├── RandomForest/    # Ensemble Tree-based model
│   │   ├── XGBoost/         # Gradient Boosting Machine
│   │   ├── NN/              # Multi-layer Perceptron / Neural Network
│   │   ├── NaiveBayes/      # Probabilistic Classifier
│   │   └── (Each model contains its own README.md & requirements.txt)
│   ├── preprocess/          # Data cleaning & feature engineering pipeline
│   │   ├── data_download.ipynb
│   │   ├── preprocess.ipynb
│   │   ├── README.md
│   │   └── requirements.txt
│   └── stack/               # Meta-learner for final ensemble prediction
│       ├── stacking.ipynb
│       ├── README.md
│       └── requirements.txt
├── data/
│   ├── processed/           # Cleaned features (train.csv, test.csv)
│   └── raw/                 # Source data (2330, ADR, DJI, SOX, etc.)
├── results/                 # Exported prediction probabilities (Meta-features)
|    ├── KNN/                 # KNN_prediction_all.csv, etc.
|    └── LSTM/                # LSTM_prediction_2025.csv, etc.
└── requirements.txt
```
## 📂 Data Pipeline & Workflow

The project follows a modular "data-first" architecture designed for high extensibility:

1.  **Data Acquisition (`data_download.ipynb`)**: Retrieves raw signals including TSMC (2330) prices via the `twstock` API, along with U.S. market indices (DJI, NASDAQ, SOX, SPX) and TSMC ADR data.
2.  **Core Preprocessing (`preprocess.ipynb`)**: Cleans raw data and generates the standardized `train.csv` (2010–2024) and `test.csv` (2025) files, which are stored in `data/processed/`.
3.  **Model-Specific Engineering**: Base learners consume these core files and apply specialized processing. For instance, the **KNN model** expands the feature pool into a **117-dimensional** space using trend discretization and lag features to capture market momentum.
4.  **Result Persistence (`results/`)**: Every base model exports its predicted "upward probabilities" into specific subdirectories (e.g., `results/KNN/KNN_prediction_all.csv`).
5.  **Stacking Execution (`stacking.ipynb`)**: The script retrieves these persistent meta-features to train the meta-learner for the final 2025 market direction forecast.

## 🛠️ Execution Order

1.  **Prepare Data**: Run `code/preprocess/preprocess.ipynb` to generate the processed CSV files.
2.  **Generate Meta-features**: Execute individual base learner notebooks in `code/models/` to populate the `results/` folder.
3.  **Perform Stacking**: Run `code/stack/stacking.ipynb` to synthesize predictions.
4.  **Backtest**: Run `code/backtest/backtest6.py` to evaluate financial performance.

---

## 🛠️ Getting Started

### Installation
Each module is designed with self-contained dependencies. It is recommended to use a virtual environment:

```bash
# 1. Clone the repository
git clone https://github.com/eevanthu/CS4602ML-FinalProject.git
cd CS4602ML-FinalProject

# 2. Setup a virtual environment (Python 3.9 recommended)
conda create -n tsmc_project python=3.9 -y
conda activate tsmc_project

# 3. Install dependencies
# Option A: To reproduce the whole project at once (Recommended)
pip install -r requirements.txt

# Option B: To reproduce the KNN model specifically
pip install -r code/models/KNN/requirements.txt
```
## Run the model
To run the whole project:
```
# Launch Jupyter Notebook
jupyter notebook
```
To reproduce the KNN model specifically, follow these steps:
```
# Navigate to the KNN module directory
cd code/models/KNN

# Launch Jupyter Notebook
jupyter notebook
```


### 📈 Performance Summary
* Detailed results and evaluation metrics are updated per project milestone in the `CheckPoint` folder.
