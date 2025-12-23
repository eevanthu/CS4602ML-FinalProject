# CS4602ML-FinalProject
# 📈 TSMC (2330) Stock Movement Prediction & Stacking Ensemble Project

This project implements a robust machine learning pipeline to predict the price movement (Up/Down) of TSMC (2330). It leverages a multi-model **Stacking Ensemble** approach, combining distance-based algorithms, tree-based models, and deep learning architectures.

## 🗂️ Project Structure

The repository is organized into four main directories: data management, model implementation, stacking ensemble logic, and result analysis.

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
└── results/                 # Exported prediction probabilities (Meta-features)
    ├── KNN/                 # KNN_prediction_all.csv, etc.
    └── LSTM/                # LSTM_prediction_2025.csv, etc.



## 傳送門 🚪

| Title    | Description |
|  ----  | ----  |
| [🔗 網站整理](./website.md)  | 好的網站 |
| [📁 Data](./data/)  | 模型資料之類檔案夾 |
| [✅ CheckPoint](./checkpoint/) | Check Point |
