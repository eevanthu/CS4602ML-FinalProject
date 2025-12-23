# TSMC Stock Prediction: Cluster-Augmented KNN Module

本子專案負責建構 **Cluster-Augmented KNN (分群增強型 K-近鄰演算法)**，作為整體 Stacking Ensemble 架構中的核心基底學習器 (Base Learner) 之一。

## 🧠 核心技術架構 (Technical Architecture)

本模組不採用傳統的原始數據輸入，而是透過空間維度增強提升預測精度：
1. **PCA 降維 (Dimensionality Reduction)**: 針對 117 維的高維特徵進行主成分分析，提取前 40 個正交主成分，有效克服「維度災難」並濾除雜訊。
2. **K-Means 市場分群**: 利用非監督式學習識別「市場狀態 (Market Regimes)」。將分群標籤作為額外特徵輸入，引導 KNN 模型在相似的歷史環境中尋找近鄰。
3. **時序對齊 (Temporal Alignment)**: 針對美股指數與 ADR 執行 `shift(1)`，確保預測時僅使用已知歷史資料，杜絕 Data Leakage。

## 🛠️ 特徵工程細節 (Feature Engineering)

程式碼實作了 `enhance_features` 函式，產生共 117 維的特徵池：
* **量價型態**: 包含 K 棒實體比例、上下影線長度及光頭光腳線辨識。
* **動能指標**: RSI (含超買賣判斷)、MACD (含黃金/死亡交叉偵測)。
* **美股聯動**: DJI, NASDAQ, SOX, SPX 之變動率與 ADR 溢價狀況。
* **時間序列項**: 包含 1 至 10 日的滯後項 (Lags) 以捕捉市場慣性。

## 📈 實驗結果 (Test Evaluation)

經過超參數掃描 ($k=11$, $n\_clusters=7$)，模型在 2025 獨立測試集表現如下：
* **Accuracy**: 63.90% 
* **Macro F1**: 0.6390
* **Recall (Down=0)**: 77.38% (模型對於下跌趨勢具備極高的辨識敏感度)

## 📋 輸出規範 (Output for Stacking)

本程式會產出各年份的預測結果檔 (例如 `KNN_prediction_2022.csv`)：
* **Date**: 交易日期。
* **Predict**: 模型輸出的上漲機率值 (Probability)，供 Meta-Learner 進行最終決策權重分配。
