# Cardiac-Disease-Prediction
Heart disease prediction comparing Logistic Regression, Decision Tree, Random Forest and KNN with cross-validated tuning in Python

Heart Disease Prediction: Logistic Regression, Decision Tree, Random Forest & KNN

Cardiac Diagnostics Case Study predicts the presence of heart disease from patient health attributes. Four classification models are tuned with cross-validated grid search and compared on a held-out test set.

Open in Google Colab https://colab.research.google.com/drive/13du2cnay5bj3tnMTiG6JrExlPMx0YNvC

Overview
Load heart.csv (303 patients, 13 features plus a binary target). There are no missing values.
One-hot encode the categorical features cp, restecg and thal.
Split into 80% train / 20% test (random_state=42): 242 training and 61 test samples.
Tune each model with GridSearchCV (5-fold stratified CV, scored on recall).
Evaluate on the test set with precision, recall, F1 and accuracy for both classes.
Features
Type	Columns
Numerical (standardized for Logistic Regression and KNN)	age, trestbps, chol, thalach, oldpeak
One-hot encoded	cp, restecg, thal
Other (binary or ordinal, used as is)	sex, fbs, exang, slope, ca
Models
Model	Best hyperparameters found
Logistic Regression	C=0.001, class_weight=None
Decision Tree	criterion='entropy', max_depth=3, min_samples_leaf=6, min_samples_split=2
Random Forest	n_estimators=30, criterion='entropy', max_depth=2, bootstrap=True
KNN	n_neighbors=11, p=1 (Manhattan), weights='uniform'
Results (test set, 61 samples)

Class 1 = heart disease, class 0 = no heart disease.

Model	Accuracy	Precision (1)	Recall (1)	F1 (1)	Recall (0)
Logistic Regression	0.70	0.65	0.97	0.78	0.41
Random Forest	0.89	0.88	0.91	0.89	0.86
KNN	0.89	0.88	0.91	0.89	0.86
Decision Tree	0.80	0.83	0.78	0.81	0.83
Observations
Logistic Regression has the highest recall for heart disease (0.97), but this comes at a cost. The tuned model uses very strong regularization (C=0.001), and it correctly identifies only 41% of healthy patients. It flags most patients as positive, which also gives it the lowest accuracy (0.70).
Random Forest and KNN give the best balance. Both reach 0.89 accuracy with about 0.91 recall on the disease class, while also classifying healthy patients well (recall 0.86).
Recall was the tuning metric because missing a patient with heart disease is the costlier error in a screening setting. Optimizing for recall alone tends to favor models that over-predict the positive class, which is what happened with Logistic Regression.
Limitations: the test set has only 61 samples, so each misclassified patient moves the metrics noticeably. Results come from a single train/test split and would be more reliable with repeated cross-validation. This is a learning exercise, not a diagnostic tool.
