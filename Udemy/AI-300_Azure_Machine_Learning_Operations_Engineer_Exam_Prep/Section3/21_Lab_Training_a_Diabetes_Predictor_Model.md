# 21 – Lab: Training a Diabetes Predictor Model

---

## 📌 Key Concepts

- **Logistic Regression (Classification):** binary classifier predicting diabetes risk (True/False)
- **ML Table:** Azure ML's managed data format; the diabetes dataset is registered as an ML Table asset
- **Pandas DataFrame:** the working data structure for sklearn-based model training
- **Feature Engineering / Normalisation:** scaling all numerical features to a [0, 1] range using MinMaxScaler
- **MinMaxScaler:** sklearn preprocessing tool that rescales feature values proportionally
- **train_test_split:** sklearn utility to partition data into training (70 %) and testing (30 %) sets
- **random_state:** seed value ensuring reproducible train/test splits across runs
- **model.fit(X_train, y_train):** trains the logistic regression model on the scaled features
- **model.predict(X_test):** generates predictions on unseen test data
- **classification_report:** sklearn function generating precision, recall, F1-score, and support per class
- **Weighted Average Metrics:** overall model metric computed as a class-size-weighted mean

---

## 📖 Definitions

| Term | Definition |
|------|------------|
| ML Table | Azure ML's structured tabular data asset format, loadable via the `mltable` library |
| Azure ML URI (`azureml://`) | Unique resource identifier for registered data assets within the Azure ML workspace |
| MinMaxScaler | Scales feature values to [0, 1]: `x_scaled = (x − min) / (max − min)` |
| train_test_split | Function splitting a DataFrame into training and testing subsets |
| random_state | Integer seed for the random number generator, ensuring consistent splits across runs |
| model.fit() | Trains the model by learning coefficients from X_train and y_train |
| model.predict() | Applies the trained model to X_test to produce class predictions |
| classification_report | Generates per-class and averaged precision, recall, F1-score, and support |
| Precision | Of all predicted positives, what fraction are truly positive |
| Recall | Of all actual positives, what fraction did the model correctly predict |
| F1-Score | Harmonic mean of precision and recall; balanced single metric |
| Outcome Column | Target variable: True (has diabetes) or False (does not have diabetes) |

---

## 💡 Examples

- **Dataset:** 768 rows × 9 columns (pregnancies, glucose, blood pressure, skin thickness, insulin, BMI, diabetes pedigree function, age, outcome)
- **Feature scale problem:** glucose values up to 200; pregnancy values 1–10 → huge magnitude difference → introduces bias → fixed with MinMaxScaler
- **After scaling:** all feature values sit between 0 and 1 (e.g. glucose 148 → 0.74; pregnancies 6 → 0.35)
- **Training results:**
  - Weighted Average Precision: **0.76**
  - Weighted Average Recall: **0.77**
  - Weighted Average F1-Score: **0.76**
- **Observation:** recall for True (diabetes) class lower than False — more training data or a cleaner dataset would improve detection of positive cases

---

## 🔢 Step-by-Step Processes

### Full Lab Workflow

1. Open **Notebooks** in Azure ML workspace → clone course GitHub repo → navigate to `machine-learning-models/logistic-regression-model/model.ipynb`
2. Attach **managed compute instance** + select **Azure ML SDK v2 Python kernel**
3. **Create Azure ML client:**
   ```python
   from azure.ai.ml import MLClient
   from azure.identity import DefaultAzureCredential
   ml_client = MLClient(DefaultAzureCredential(), subscription_id, rg, workspace)
   ```
4. **Load ML Table data asset:**
   ```python
   import mltable
   tbl = mltable.load("azureml://datastores/.../diabetes-mltable/versions/1")
   df = tbl.to_pandas_dataframe()
   ```
5. **Feature Engineering — MinMaxScaler:**
   ```python
   from sklearn.preprocessing import MinMaxScaler
   scaler = MinMaxScaler()
   numeric_cols = ['pregnancies','glucose','blood_pressure','skin_thickness','insulin','bmi','diabetes_pedigree_function','age']
   df[numeric_cols] = scaler.fit_transform(df[numeric_cols])
   ```
6. **Train/Test Split (70/30):**
   ```python
   from sklearn.model_selection import train_test_split
   X = df.drop('outcome', axis=1)
   y = df['outcome']
   X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)
   ```
7. **Train Logistic Regression Model:**
   ```python
   from sklearn.linear_model import LogisticRegression
   model = LogisticRegression(max_iter=10, C=0.3)
   model.fit(X_train, y_train)
   ```
8. **Generate Predictions:**
   ```python
   y_pred = model.predict(X_test)
   ```
9. **Evaluate Model:**
   ```python
   from sklearn.metrics import classification_report
   print(classification_report(y_test, y_pred))
   ```

---

## 💻 Code Blocks

```python
# Complete lab code summary

import pandas as pd
import mltable
from azure.ai.ml import MLClient
from azure.identity import DefaultAzureCredential
from sklearn.preprocessing import MinMaxScaler
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import classification_report

# 1. Load data asset
tbl = mltable.load("azureml:diabetes-mltable:1")
df = tbl.to_pandas_dataframe()

# 2. Feature engineering
numeric_cols = ['pregnancies','glucose','blood_pressure','skin_thickness',
                'insulin','bmi','diabetes_pedigree_function','age']
scaler = MinMaxScaler()
df[numeric_cols] = scaler.fit_transform(df[numeric_cols])

# 3. Split data
X = df.drop('outcome', axis=1)
y = df['outcome']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)
# Training rows: 537  |  Testing rows: 231

# 4. Train model
model = LogisticRegression(max_iter=10, C=0.3)
model.fit(X_train, y_train)

# 5. Predict & evaluate
y_pred = model.predict(X_test)
print(classification_report(y_test, y_pred))
# Weighted avg → Precision: 0.76 | Recall: 0.77 | F1: 0.76
```

---

## 📝 Summary

> "The reason that we are converting it into a pandas data frame is because the library that we're going to be using, Scikit-Learn, essentially works much better integrated with pandas data frames."

This lab trains a **logistic regression binary classifier** on the diabetes dataset to predict patient diabetes risk. The pipeline covers data loading from an Azure ML Table asset, feature normalisation with MinMaxScaler, a 70/30 train-test split with a fixed random seed, model training, and quantitative evaluation via sklearn's `classification_report`. The achieved weighted F1-score of 0.76 represents a solid baseline, with room to improve recall on the positive (diabetes) class through more data or hyperparameter tuning.

---

## ✅ Actionable Takeaways

- Always **scale features** before training logistic regression — unscaled magnitudes bias the model toward high-value features
- Use **MinMaxScaler** (not StandardScaler) when features have known bounded ranges
- Set `random_state=42` (or any fixed integer) in `train_test_split` for reproducibility across notebook runs
- Read both the **per-class** and **weighted average** rows of `classification_report` — they tell different stories
- If recall on the positive class is low, consider collecting more positive-class examples or adjusting the decision threshold
- Register the trained model in the Azure ML Model Registry after evaluating it for deployment

---

## 🏷️ Tags

`#azure-ml` `#logistic-regression` `#classification` `#scikit-learn` `#diabetes` `#feature-engineering` `#minmaxscaler` `#pandas` `#mltable` `#lab` `#supervised-learning` `#precision` `#recall` `#f1-score`

---

## 🔗 Backlink Suggestions

- [[14 – Introduction to Statistical and Machine Learning]] — train/test split and MSE concepts
- [[15 – Introduction to ML Libraries]] — sklearn, pandas, mltable library stack used here
- [[16 – Understanding Linear Algorithms]] — logistic regression theory behind this lab
- [[22 – Classification Evaluations Confusion Matrix and ROC Curve]] — deeper evaluation of classification model outputs
- [[20 – Anatomy of Azure ML Jobs Experiments and Runs]] — how to track and log this notebook's training run
- [[24 – Lab Automated Training with AutoML]] — AutoML can automate the same diabetes classification task
