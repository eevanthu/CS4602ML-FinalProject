# TSMC Stock Prediction: Cluster-Augmented KNN Module

This sub-module is responsible for constructing the **Cluster-Augmented K-Nearest Neighbors (KNN)** model, serving as a primary base learner within the overall Stacking Ensemble architecture.

## 🧠 Technical Architecture

* **PCA Dimensionality Reduction**: Projects the 117-dimensional feature space onto 40 orthogonal principal components to mitigate the "Curse of Dimensionality" and filter out redundant market noise.
* **K-Means Market Regime Identification**: Employs unsupervised learning to identify distinct market regimes. [cite_start]These cluster labels are used as auxiliary features to guide the KNN model in identifying historical "neighbors" within structurally similar market environments [cite: 322-323, 602].
* **Temporal Alignment**: Implements a `shift(1)` operation on U.S. indices and ADR data to strictly prevent data leakage, ensuring the model only uses information available at the time of trade.

## 🛠️ Feature Engineering
The module utilizes an `enhance_features` function to expand the raw dataset into a 117-dimensional space. This includes:
* **Technical Indicators**: Discretized trend signals such as RSI overbought/oversold triggers and Moving Average crossovers.
* **Global Interdependency**: Spillover effects from major U.S. indices (NASDAQ, S&P 500, DJI, SOX).
* **Temporal Lags & Candlestick Patterns**: Price inertia over a 1-to-10 day lookback period and mathematical quantization of candlestick body/shadow ratios.

## 🔄 Execution Workflow ⚠️ IMPORTANT

To generate the meta-features required for the Stacking Layer, a **Yearly Rolling Forecast** approach must be adopted:

1.  **Yearly Data Splitting**:
    * Generate specific training and testing files for the target year (e.g., `2022_train.csv` and `2022_test.csv`).
    * The test set must contain only data from that specific year, while the training set includes all historical data prior to that year.
2.  **Model Re-execution**:
    * Point the training pipeline to the specific yearly slice and re-run the KNN training and prediction sequence.
    * Export the result as a CSV file named `KNN_prediction_YYYY.csv`.
3.  **Iteration**:
    * Repeat the above process to produce individual prediction files for each year from **2020 to 2025**.
4.  **Automated Merging**:
    * Store all yearly files in the `data/results/KNN/` directory.
    * Execute the merging script to aggregate all files into the final `KNN_prediction_all.csv` used for stacking.

## 📋 Output Format
* **Date**: Trading date.
* **Predict**: The predicted "probability of upward movement" (a value between 0 and 1).
* **Purpose**: These probabilities serve as input features for the Meta-Learner to determine final decision weights.

## 📈 Performance Evaluation (2025 Test Set)
* **Accuracy**: 63.90%
* **Macro F1**: 0.6390
* **Recall (Down=0)**: 77.38%
