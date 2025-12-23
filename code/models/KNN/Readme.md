# TSMC Stock Prediction: Cluster-Augmented KNN Module

本子專案負責建構 **Cluster-Augmented KNN (分群增強型 K-近鄰演算法)**，作為整體 Stacking Ensemble 架構中的基底學習器 (Base Learner)。

## 🧠 核心技術架構 (Technical Architecture)

1. **PCA 降維**: 將 117 維特徵投影至 40 個主成分，減少雜訊並克服維度災難。
2. **K-Means 市場分群**: 辨識「市場狀態 (Market Regimes)」，並將其作為特徵引導 KNN 尋找相似環境下的鄰居。
3. **時序對齊**: 針對美股指數與 ADR 執行 `shift(1)`，嚴格杜絕資料洩漏 (Data Leakage)。

## 🛠️ 特徵工程 (Feature Engineering)
程式實作了 `enhance_features` 函式，整合量價型態、RSI、MACD 及 1-10 日滯後項 (Lags)，共產出 117 維特徵。

## 🔄 執行工作流 (Execution Workflow) ⚠️ 重要

為了生成 Stacking 所需的 Meta-features，必須採取「年份滾動式預測」，請遵循以下步驟：

1. **產生年份切片 (Yearly Data Splitting)**:
   - 根據目標年份，先產生對應的訓練與測試檔（例如：`2022_train.csv` 與 `2022_test.csv`） 。
   - 確保測試集年份為該特定年份，訓練集則包含該年份以前的所有歷史數據。

2. **重新執行模型 (Model Execution)**:
   - 將訓練流水線指向該年份的切片，重新執行 KNN 模型訓練與預測程序。
   - 輸出該年份的預測結果檔，命名格式為 `KNN_prediction_YYYY.csv`。

3. **重複週期 (Iteration)**:
   - 重複上述步驟，逐一產出 **2020 年至 2025 年** 的各年度預測檔。

4. **自動化合併 (Result Merging)**:
   - 確保所有年份檔案皆存放在 `data/results/KNN/` 目錄下。
   - 執行程式碼中的合併腳本，系統會自動抓取所有檔案並整合成最終的 `KNN_prediction_all.csv` 。

## 📋 輸出格式說明
* **Date**: 交易日期。
* **Predict**: 模型輸出的上漲機率值 (Probability)，數值介於 0 與 1 之間。
* **用途**: 此機率值將作為 Meta-Learner (XGBoost) 的輸入特徵，用於最終決策權重分配 。

## 📈 測試評估 (2025 Test Set)
* **Accuracy**: 63.90%
* **Macro F1**: 0.6390
* **Recall (Down=0)**: 77.38%
