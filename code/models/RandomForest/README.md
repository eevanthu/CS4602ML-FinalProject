# TSMC Stock Price Prediction
This project uses a machine learning model (Random Forest) to predict if the TSMC (2330.TW) stock price will go "Up" or "Down."

## 1. Introduction
We use TSMC's past trading data and US market indicators (like NASDAQ and the SOX index) to predict the stock price for the next two days (T+2).

## 2. How to Use
Prepare the Data: Put the 2330.csv file in the same folder as the code.

Run the Notebook: Open DT_and_RF_for_TSMC_stock.ipynb and run all the cells.

Check the Output: The program will create two files:

TSMC_Prediction_Proba.csv: Contains the predictions and "confidence scores."

prediction_accuracy_over_time.png: A chart showing the model's performance.

## 3. Our Method

Feature Engineering: We do not use "absolute prices." Instead, we use "percentage changes" and "relative indicators." This helps the model stay accurate even when the stock price becomes very high.

External Indicators: we added data from the US SOX index and NASDAQ because they strongly affect TSMC's stock.

Model Tuning: We use the Random Forest model. We also used a tool called RandomizedSearchCV to find the best settings for the model.

Time Series Split: We make sure the model learns from the "past" to predict the "future." This is the correct way to test stock models.

## **4**. Performance
Accuracy: In our tests, the model's accuracy is about 62%.

Most Important Factor: The SOX index (US semiconductor market) is the most important factor for predicting TSMC's price.