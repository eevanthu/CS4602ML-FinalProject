# TSMC Stock Prediction: LSTM Time-Series Module

This sub-module is responsible for constructing the **Long Short-Term Memory (LSTM)** model. It serves as a core deep learning base learner within the Stacking Ensemble architecture, specifically designed to capture non-linear temporal dependencies and sequential patterns in stock price movements.

## Technical Architecture

* **Sliding Window Mechanism**: Unlike static models, this module utilizes a **32-day lookback window** (`sequence_length=32`). It transforms the dataset into 3D tensors `(Batch, Sequence, Features)` to preserve the chronological order of price data.
* **Deep Learning Architecture**:
* **LSTM Layer**: A robust LSTM layer with **128 hidden units** extracts long-term dependencies from the input sequences.
* **Batch Normalization (1D)**: Applied immediately after the LSTM output to standardize hidden states, mitigating the vanishing gradient problem and accelerating convergence.
* **Weighted Loss Function**: Implements `BCEWithLogitsLoss` with a dynamic `pos_weight` (calculated from class distribution) to fundamentally address the class imbalance problem between "Up" and "Down" movements.


* **Regularization**: Incorporates `Dropout (0.2)` and **L2 Regularization** (via Adam optimizer weight decay) to prevent overfitting on noisy financial data.

## Feature Engineering

The model operates on a strictly selected subset of **9 High-Importance Features** to reduce noise and focus on momentum signals:

* **Momentum & Volatility**: `RSI14` (Relative Strength Index), `BB_position` (Bollinger Bands), `ADX` (Trend Strength).
* **Price Dynamics**: `Movement` (Lagged Target), `Open-Close` (Daily spread), `BR5`, `BR10` (Bias Ratios).
* **Cross-Market Influence**: `ADR` (TSMC US ADR), `TWD` (Exchange Rate).

## Execution Workflow

### 1. Data Preparation & Sequencing

* **Input**: Loads preprocessed `train.csv` and `test.csv`.
* **Sequence Generation**: The `create_sequences` function converts 2D tabular data into 3D time-series sequences.
* *Note*: For the test set, the script automatically concatenates the last 32 days of the training data to the beginning of the test data. This ensures the first prediction of 2025 has sufficient historical context (No "Cold Start" problem).



### 2. Training Pipeline

* **Validation Split**: The training data (2010-2024) is split (90/10) to monitor validation loss.
* **Early Stopping Simulation**: The model is trained for **50 epochs**. Training history (Loss/Accuracy/F1) is recorded to identify the optimal stopping point (typically observed around epoch 5-10).
* **Full-Retraining**: After identifying optimal hyperparameters, the model is retrained on the *entire* training dataset before generating test predictions.

### 3. Inference & Filtering

* **Confidence Scoring**: The raw probability output is processed through a confidence filter.
* Formula: 
* **Threshold**: Only predictions with confidence **> 0.3** (Prob > 0.65 or < 0.35) are considered strong signals.



## Output Format
The model outputs a CSV file containing the following columns, designed to facilitate performance evaluation and strategy backtesting:

* **Date**: The trading date of the prediction.
* **Actual_Movement**: The ground truth label (Target), where `1` indicates a price increase and `0` indicates a decrease.
* **Prob_Rise**: The raw predicted probability of an "Upward" movement (Float range: `0.0` to `1.0`).
* **Predicted_Class**: The initial binary classification based on a standard 0.5 threshold (`1` if Prob_Rise > 0.5, else `0`).
* **Confidence**: A metric representing the strength of the prediction signal, calculated as $| \text{Prob\_Rise} - 0.5 | \times 2$.
* **Filtered_Prediction**: The final actionable signal after applying the confidence threshold:
* `1`: High-confidence **Up** signal.
* `0`: High-confidence **Down** signal.
* `-1`: **Neutral/Hold** (Confidence is below the threshold, indicating high uncertainty).

## Performance Evaluation (2025 Test Set)

* **Accuracy (Overall)**: ~56% (Baseline)
* **Accuracy (High Confidence)**: **60.0%** (After filtering with threshold 0.3)
* **Key Strength**: capturing strong trend reversals and sustained momentum phases where static models often fail.



