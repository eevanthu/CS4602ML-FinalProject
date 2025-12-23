
1. Data Acquisition (`data_download.ipynb`)

**Goal**: Download historical trading data and calculate essential technical indicators.

* **Data Sources**:
* **Target Stock**: TSMC (2330.TW) via `yfinance` (Originally, we used the twstock API for data collection. However, due to issues with twstock, we switched to using yfinance instead.).
* **Global Market Indicators**:
* **US Tech Indices**: NASDAQ (`^IXIC`), PHLX Semiconductor (`^SOX`).
* **Market Benchmarks**: S&P 500 (`^GSPC`), Dow Jones (`^DJI`).
* **Exchange Rate**: USD/TWD (`TWD=X`).
* **ADR**: TSMC ADR (`TSM`).




* **Feature Engineering (Initial)**:
* Calculates technical indicators immediately after download:
* **Trend**: SMA (MA5, MA10), EMA, MACD, ADX.
* **Momentum**: RSI (Wilder's Smoothing), Stochastic Oscillator (KD).
* **Volatility**: Bollinger Bands (High/Low/Position), ATR, Standard Deviation.
* **Price Action**: Daily Return, Bias Ratio (BR), Daily Range (High-Low, Open-Close).

* **Output**: Raw CSV files saved in `data/` (e.g., `2330.csv`, `^SOX.csv`).

## 2. Data Preprocessing (`preprocess.ipynb`)

**Goal**: Merge multiple data sources, handle missing values, and normalize features for machine learning.

* **Data Merging Strategy**:
* **`merge_asof`**: Uses "backward search" to align global market data with TSMC's trading days.
* **Time-Zone Alignment**:
* **US Market Data**: Shifted by **1 day** (`shift(1)`) to prevent data leakage (since US markets close after Taiwan markets open).
* **Exchange Rate**: Shifted by 1 day to ensure only pre-market information is used.

* **Cleaning & Transformation**:
* **Missing Values**: Imputes missing data using forward-fill (`ffill`) to maintain time-series continuity.
* **Data Typing**: Enforces numeric types for all feature columns.


* **Data Splitting**:
* **Training Set**: 2010/01/01 – 2024/12/31
* **Testing Set**: 2025/01/01 – 2025/12/31 (Held-out for final evaluation).


* **Normalization**:
* Applies **RobustScaler** to all feature columns. This is crucial for financial data as it reduces the influence of extreme outliers (e.g., market crashes or bubbles) better than standard Min-Max scaling.



## Output Files

After running these two notebooks, the `data/` directory will contain:

* `train.csv`: The scaled training dataset ready for model input.
* `test.csv`: The scaled testing dataset.
* `df.csv`: The intermediate merged dataset (unscaled) for reference.
