
### 1. Data Acquisition
**Notebook:** `data_download.ipynb`
* **Description:** Downloads historical trading data for TSMC using the `twstock` API. It also fetches global market indicators (e.g., DJI, ADR) using the `yfinance` API.
* **Output:** Generates the raw dataset `2330.csv`, `DJI.csv`, `ADR.csv`, `NASDAQ.csv`, `SOX.csv`, `SPX.csv`, `DJI.csv`

### 2. Data Preprocessing
**Notebook:** `preprocess.ipynb`
* **Description:** Handles missing values, performs feature engineering (calculating RSI, MACD, Bollinger Bands), and aligns time-series data.
* **Data Split:**
    * **Training Set:** 2021–2024
    * **Testing Set:** 2025
* **Output:** Saves the cleaned datasets for model training (e.g., `train.csv`, `test.csv`).

### 3. Base Model Training (Single Models)
Train the individual base learners to generate initial prediction probabilities. You can run these notebooks in any order.

* **KNN:** `KNN.ipynb` - Implements K-Nearest Neighbors with dynamic time warping or Euclidean distance.
* **LSTM:** `LSTM.ipynb` - Trains a Long Short-Term Memory network with a sliding window approach (Seq Length=32).
* **XGBoost:** `XGBoost.ipynb` - Trains a Gradient Boosting decision tree model.
* **Output:** Each notebook saves its probability predictions into a CSV file 

### 4. Stacking Ensemble
**Notebook:** `stacking.ipynb`
* **Description:** Aggregates the probability outputs from the three base models. It trains a Meta-Learner (XGBoost) to assign dynamic weights to each base model and generates the final prediction.
* **Output:**
    * Final performance metrics (Accuracy, F1-score).
    * Feature Importance plot.
    * `prediction_analysis_summary.png` and prediction result files.
