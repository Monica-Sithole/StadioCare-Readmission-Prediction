# Model 2: Random Forest

## 1. Introduction

Random Forest was selected as the second classification model for the project. The model aims to predict whether a patient is likely to be readmitted to hospital within 30 days of discharge, using patient and hospital encounter characteristics.

The model addresses the research question:

“Can patient and hospital encounter characteristics be used to identify patients at increased risk of readmission within 30 days of discharge?”

Random Forest was selected to complement Logistic Regression by capturing nonlinear relationships and interactions between features. Its performance is evaluated and compared with Logistic Regression in Part C.

## 2. Data Preprocessing

The model uses the processed training and testing datasets generated during the preprocessing and feature engineering stages.

The Diabetes 130-US Hospitals dataset contains 101,766 hospital encounters from 130 US hospitals between 1999 and 2008.

The following preprocessing and feature engineering techniques were applied:

* Missing numerical values were imputed using the median.
* Missing categorical values were imputed using the most frequent category.
* Categorical features were converted into numerical representations using one-hot encoding.
* Numerical features were standardised.
* The data was split into training and testing sets using a patient-level group split to reduce the risk of the same patient's encounters appearing in both sets.

The feature transformation process was fitted on the training data and applied to the test data to prevent information leakage.

The target variable was converted into a binary classification outcome:

* 1: Readmitted within 30 days (`<30`).
* 0: Not readmitted within 30 days (`NO` or `>30`).

## 3. Model Technique

Random Forest is a supervised ensemble learning algorithm that combines multiple decision trees to make predictions. Each tree is trained using randomly selected observations and a subset of features. The individual tree predictions are combined to produce the final classification and predicted probabilities.

For this project, Random Forest was implemented using the `RandomForestClassifier` estimator from Scikit-learn.

The model can capture nonlinear patterns and interactions between patient and hospital encounter characteristics without assuming a linear relationship between the features and the target.

## 4. Hyperparameter Configuration

The following hyperparameters were used:

| Hyperparameter                                 | Value    |
| ---------------------------------------------- | -------- |
| Number of trees (`n_estimators`)               | 200      |
| Maximum tree depth (`max_depth`)               | 20       |
| Minimum samples to split (`min_samples_split`) | 10       |
| Minimum samples per leaf (`min_samples_leaf`)  | 5        |
| Maximum features (`max_features`)              | sqrt     |
| Class weight (`class_weight`)                  | balanced |
| Random state (`random_state`)                  | 42       |
| Parallel processing (`n_jobs`)                 | -1       |

The model was configured with 200 decision trees. The maximum tree depth was restricted to 20, while the minimum samples required for splitting and leaf nodes were set to 10 and 5, respectively. These settings were used to control tree complexity.

The `sqrt` setting considers a subset of features at each split. Balanced class weights were applied to account for the difference in the number of positive and negative target observations.

The random state was set to 42 to support reproducibility, while `n_jobs=-1` enabled parallel processing using available CPU cores.

These were the initial hyperparameter settings used for the experiment and were not established through an exhaustive hyperparameter optimisation procedure.

## 5. Model Training

The Random Forest model was trained using the processed training dataset generated during feature engineering.

The same training and testing datasets were used for Logistic Regression and Random Forest to support a consistent performance comparison.

The model generated binary predictions and estimated probabilities of readmission within 30 days. The default classification threshold of 0.5 was used for the class-based evaluation metrics.

## 6. Model Outputs

The trained model was saved as:

`models/random_forest_model.joblib`

The model evaluation metrics were saved as:

`outputs/random_forest_metrics.csv`

The test predictions and predicted probabilities were saved as:

`outputs/random_forest_predictions.csv`

The feature importance results were saved as:

`outputs/random_forest_feature_importance.csv`

## 7. Model Evaluation

The performance of Random Forest was evaluated using accuracy, precision, recall, F1-score, ROC-AUC and PR-AUC.

The confusion matrix was used to examine true positives, true negatives, false positives and false negatives. ROC and precision-recall curves were generated to assess model discrimination and performance across classification thresholds.

The feature importance values were also calculated to identify which encoded features contributed to the model's tree-based predictions. These values indicate predictive contribution within the fitted model and do not establish causal relationships.

The detailed performance results, statistical analysis and comparison with Logistic Regression are presented in `Model2Performance.MD` and `Comparison.MD`.

## 8. Limitations

The model identifies patterns associated with observed readmission outcomes but does not establish whether readmissions were avoidable or caused by particular patient or hospital characteristics.

The dataset represents historical US hospital encounters from 1999 to 2008. Its results may not generalise directly to current South African healthcare settings.

The model should be considered a decision-support tool for identifying patients who may benefit from additional assessment and follow-up, rather than a replacement for clinical judgement.
