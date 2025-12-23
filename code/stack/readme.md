# TSMC Stock Prediction: Stacking Ensemble Module

This sub-module implements the **Stacking Ensemble Learning** strategy, serving as the final decision-making layer of the project. It integrates the predictive signals from individual base learners (KNN, LSTM, XGBoost) to generate a robust and consistent market direction forecast.

## Stacking Strategy

Instead of relying on a single model, this module constructs a **Meta-Learner** that learns how to trust each base model under different conditions.

* **Base Learners (Input Features)**:
* **KNN**: Captures historical pattern similarities.
* **LSTM**: Captures temporal dependencies and trends.
* **XGBoost (Base)**: Captures complex non-linear feature interactions.


* **Meta-Learner**: **XGBoost Classifier**
* We deliberately chose a shallow XGBoost model (constrained `max_depth`) as the meta-learner to act as a **weighted voting mechanism** rather than a complex pattern recognizer. This design prevents the stacking layer from overfitting to the base models' noise.



## Execution Workflow

### 1. Meta-Dataset Construction

* **Input**: Loads `stacking.csv`, which aggregates the probability outputs from all base learners.
* **Data Split**:
* **Training Set**: **2021–2024** (Used to teach the Meta-Learner how to combine base models).
* **Testing Set**: **2025** (Strictly held-out for final evaluation).



### 2. Base Model Evaluation

* Before training the meta-learner, the script evaluates the standalone performance of each base model (Accuracy on Train vs. Test) to establish a baseline for comparison.

### 3. Meta-Model Training (Grid Search)

* **Algorithm**: XGBoost Classifier.
* **Hyperparameter Tuning**: Implements `GridSearchCV` with 3-fold Cross-Validation to find the optimal balance.
* **Key Parameters**:
* `n_estimators`: [50, 100, 200]
* `max_depth`: [1, 2, 3] (Kept low to prevent overfitting)
* `learning_rate`: [0.005, 0.01, 0.02]





### 4. Final Inference & Feature Importance

* The best meta-model is selected to predict the 2025 market direction.
* **Feature Importance Analysis**: The script outputs the importance weights assigned to each base model, revealing which model contributes most to the final decision.

## Output Format

The module exports a CSV file (e.g., `stacking_pred_prob.csv`) containing:

* **Date**: Trading date.
* **Probability**: The final ensemble probability (Normalized 0.0 - 1.0).
* **Actual**: The ground truth movement for verification.
* **Purpose**: This file is the "gold standard" prediction used for final backtesting and trading simulation.
