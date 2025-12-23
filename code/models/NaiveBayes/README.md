# TSMC Stock Prediction: Gaussaian Naive Bayes Model

This sub-module is constructed using  **Gaussian Naive Bayes**  model. It serves as a core deep learning base learner within the Stacking Ensemble architecture, specifically designed to capture basic probabilities trend in stock price movements.

## Technical Architecture

-   **Naive Bayes Classification**: Implements a Naive Bayes classifier for supervised learning tasks by assuming conditional independence between features given class labels. This probabilistic model enables efficient inference on high-dimensional data while managing variable correlation implicitly through the independence assumption.
    
-   **Cross-Validation**: Utilizes k-fold cross-validation to systematically evaluate the classifier's generalization performance, ensuring robust estimation of predictive accuracy and minimizing overfitting risk.
    

## Feature Engineering

The model operates on a strictly selected subset of  **K  Best Features**  decided using sklearn function SelecKBest() that produces the most informations. We do so to reduce noise and focus on momentum signals:

-   **Cross Vlidated K**:  Cross validation suggest that K = 28 is the best.
-   **Score function**:  mutual info classif is used in this model ( decided by cross validation )

## Execution Workflow

### 1. Data Preparation

-   **Input**: Loads preprocessed  `train.csv`  and  `test.csv`.

### 2. Training Pipeline

-   **Train Validation  Set Split**: The training data (2010-2024) is split (90/10) to monitor validation loss.
-  **Select Features**: Select K Best is preformed on both the sets.
-  **Train Model**:  Fit the model using tracing set and check performance using validation set.

## Output Format

The model outputs a CSV file containing the following columns, designed to facilitate performance evaluation and strategy backtesting:

-   **Date**: The trading date of the prediction.
-   **Predicted_Class**: The initial binary classification based on a standard 0.5 threshold (`1`  if Prob_Rise > 0.5, else  `0`).

## Performance Evaluation (2025 Test Set)

[](https://github.com/eevanthu/CS4602ML-FinalProject/tree/main/code/models/KNN#performance-evaluation-2025-test-set)

-   **Accuracy (Overall)**: ~54% (Baseline)
-   **Macro f1 Score**:  **63.0%**
