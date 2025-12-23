---
title: README

---

# TSMC Stock Prediction: Deep Neural Network (DNN) Model

This sub-module is constructed using a **Deep Neural Network (DNN)** implemented in PyTorch. It serves as a sophisticated base learner designed to capture complex, non-linear relationships in stock price movements that traditional statistical models might miss.

## Technical Architecture

* **Deep Neural Network (DNN)**: Implements a multi-layer Perceptron (MLP) architecture using `torch.nn`. The model consists of fully connected (Linear) layers, activation functions (ReLU), and normalization layers to extract high-level features from technical indicators.
* **Regularization & Stability**:
    * **Batch Normalization**: Applied to stabilize learning and accelerate convergence.
    * **Dropout**: Implemented (default rate ~0.35) to prevent overfitting by randomly zeroing out neurons during training.
* **Optimization**:
    * **Optimizer**: Uses the **Adam** optimizer with weight decay to handle sparse gradients and minimize the loss function efficiently.
    * **Loss Function**: Utilizes **CrossEntropyLoss**, treating the prediction as a classification problem (Up/Down).

## Feature Engineering

Unlike models relying purely on statistical feature selection, this model uses **Domain-Knowledge Based Selection** to preprocess the data, focusing on momentum and relative changes rather than absolute values:

* **Removing Absolute Prices**: The model explicitly drops absolute columns (e.g., `Open`, `High`, `Low`, `Close`, `Transaction`, `Capacity`) and absolute Moving Averages (`MA5`, `MA10`). This prevents the model from overfitting to specific price levels that may not recur.
* **External Market Indicators**: Calculates the percentage change (`rate`) for external indices (e.g., DJI, NASDAQ, SOX, SPX) to capture global market correlation.
* **Normalization**: All numerical features are scaled using `MinMaxScaler` (mapped to 0-1 range) to ensure consistent gradient updates across different indicators.

## Execution Workflow

### 1. Data Preparation
* **Input**: Loads `df.csv`
* **Preprocessing Pipeline**:
    1.  Compute `pct_change` for external market indices.
    2.  Drop irrelevant (Date) and "cheating" features (absolute prices).
    3.  Normalize features using a scaler fitted on the training set.

### 2. Training Pipeline
* **Train/Validation Split**: The training data is split (e.g., 80/20) to monitor validation loss and accuracy during training.
* **Model Training**: The model iterates through `NUM_EPOCHS`, updating weights via backpropagation.
* **Monitoring**: Tracks Loss, Accuracy, and F1-Score for both training and validation sets to identify the best model checkpoint.

### 3. Testing & Evaluation
* **Prediction**: The model evaluates the `df.csv` dataset (preprocessed identically to the training set) with data of 2025.
* **Metrics**: Outputs Accuracy, F1-Score, and a Confusion Matrix to visualize True Positives/Negatives.

## Output Format

The model outputs a CSV file `submission.csv` containing the predictions, formatted for strategy backtesting:

* **Date** (Implicitly aligned with input)
* **Prediction**: The binary classification result (`1` for Rise, `0` for Fall).

## Performance Evaluation (Example Results)

### Based on the T+1 prediction test set evaluation:

* **Accuracy**: ~55.6%
* **F1-Score**: ~0.45

### Based on the T+1 prediction test set evaluation:
* **Accuracy**: ~52.2%
* **F1-Score**: ~0.30
### Confusion Matrix
Highlights the model's ability to distinguish between market movements, though specific precision/recall trade-offs are observed.