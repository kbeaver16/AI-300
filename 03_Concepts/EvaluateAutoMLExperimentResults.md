# 🧠 **Understanding Automated ML Results (Azure Machine Learning)**

[learn.microsoft.com](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-understand-automated-ml?view=azureml-api-2)

## 1. **What Automated ML Produces**

Automated ML (AutoML) trains **many models** during an experiment. Each model produces:

- Evaluation **metrics**
- Diagnostic **charts**
- A recommended **best model**
- Optional **Responsible AI dashboard** (explanations, fairness, error analysis)

You can inspect results in:

- Azure ML Studio → _Jobs_
- Jupyter Notebook → _JobDetails widget_

---

# 📊 **Classification Metrics**

AutoML computes metrics using **scikit‑learn** implementations. For multiclass problems, metrics are averaged using:

- **Macro** — unweighted average across classes
- **Micro** — global TP/FP/FN counts
- **Weighted** — weighted by class frequency

### Key Metrics

|Metric|What It Measures|Goal|
|---|---|---|
|**AUC**|Area under ROC curve|→ 1|
|**Accuracy**|% of correct predictions|→ 1|
|**Average Precision**|Weighted precision across recall thresholds|→ 1|
|**Balanced Accuracy**|Mean recall across classes|→ 1|
|**F1 Score**|Harmonic mean of precision & recall|→ 1|
|**Log Loss**|Negative log-likelihood|→ 0|
|**Precision**|Avoiding false positives|→ 1|
|**Recall**|Avoiding false negatives|→ 1|
|**Matthews Correlation**|Balanced accuracy measure|→ 1|

### Example

If your dataset has **class imbalance**, macro‑averaged F1 is often more informative than accuracy because accuracy can be inflated by majority classes.

---

# 📈 **Classification Charts**

### **Confusion Matrix**

Shows how often each class is predicted correctly or incorrectly.

- **Good model** → values concentrated on diagonal
- **Bad model** → off‑diagonal errors dominate

Example:  
If class “Dog” is often predicted as “Cat,” the Dog row will have high counts in the Cat column.

---

### **ROC Curve**

Plots **TPR vs FPR** across thresholds.

- Curve near **top-left** → strong model
- Curve near **diagonal** → random model

AUC = probability a random positive is ranked above a random negative.

---

### **Precision–Recall Curve**

Useful when **positive class is rare**.

- High precision at high recall → strong model
- Curve near axes → weak model

---

### **Cumulative Gains Curve**

Shows % of positives captured when selecting top‑ranked predictions.

- Perfect model → steep rise to 100%
- Random model → diagonal line

---

### **Lift Curve**

Shows how many times better the model performs vs random.

- Lift > 1 → better than random
- Higher lift early in the curve → strong ranking ability

---

### **Calibration Curve**

Compares predicted probability vs actual frequency.

- Perfect model → diagonal line
- Overconfident model → backward “S”
- Underconfident model → forward “S”

---

# 📉 **Regression & Forecasting Metrics**

AutoML normalizes regression metrics to allow comparison across datasets with different ranges.

### Key Metrics

|Metric|Meaning|Goal|
|---|---|---|
|**Explained Variance**|How much variance is captured|→ 1|
|**MAE**|Avg absolute error|→ 0|
|**MAPE**|Avg % error|→ 0|
|**Median AE**|Median absolute error|→ 0|
|**R² Score**|Reduction in MSE vs variance|→ 1|
|**RMSE**|Root mean squared error|→ 0|
|**RMSLE**|RMSE on log scale|→ 0|
|**Spearman Correlation**|Rank correlation|→ 1|

### Metric Normalization

[ \text{normalized_error} = \frac{\text{error}}{y_{\max} - y_{\min}} ]

Example:  
If RMSE = 50 but the target range is 1000, normalized RMSE = 0.05 → indicates strong performance.

---

# 📉 **Regression & Forecasting Charts**

### **Residuals Histogram**

Shows distribution of prediction errors.

- Good model → peak at 0, symmetric
- Bad model → wide spread, skewed

---

### **Predicted vs True**

Plots mean predicted value per bin of true values.

- Good model → line close to **y = x**
- Large deviations → bias or poor generalization

---

### **Forecast Horizon Chart**

For forecasting experiments:

- X‑axis = time
- Vertical line = forecast horizon
- Shows predictions vs actuals across CV folds
- Purple band = confidence interval

Example:  
If predictions diverge after the horizon, the model may not capture seasonality well.

---

# 🖼️ **Image Model Metrics (Preview)**

For image classification:

- **Accuracy** for binary/multiclass
- **IoU** for multilabel
- Loss logged per epoch
- Predictions include confidence scores

Example:  
If multilabel threshold = 0.5, a class predicted with 0.48 confidence is treated as _not present_.

---

# 📚 **Vocabulary Section**

|Term|Definition|
|---|---|
|**AutoML**|Automated process that trains, tunes, and evaluates many ML models.|
|**AUC**|Area under ROC curve; measures ranking quality.|
|**Macro/Micro/Weighted Averaging**|Methods for aggregating multiclass metrics.|
|**Confusion Matrix**|Table showing predicted vs actual class counts.|
|**ROC Curve**|Plot of TPR vs FPR across thresholds.|
|**Precision**|TP / (TP + FP).|
|**Recall**|TP / (TP + FN).|
|**Lift**|Model performance relative to random.|
|**Calibration**|How well predicted probabilities match actual outcomes.|
|**Residual**|Prediction error: ( y_{\text{pred}} - y_{\text{true}} ).|
|**RMSE**|Square root of mean squared error.|
|**Forecast Horizon**|Point in time where predictions begin.|
|**IoU**|Intersection over Union; used in multilabel image tasks.|

---

# 🧪 **Examples to Solidify Understanding**

### **Example 1 — Classification**

A fraud detection model has:

- Accuracy = 0.98
- Recall (fraud class) = 0.42

Interpretation:  
The model rarely flags fraud → high accuracy but poor recall. You’d examine the **PR curve** and **confusion matrix** to diagnose.

---

### **Example 2 — Regression**

A house price model has:

- RMSE = 40,000
- Target range = 0–1,000,000

Normalized RMSE = 0.04 → strong performance despite large raw error.

---

### **Example 3 — Forecasting**

A demand forecasting model shows:

- Good alignment before horizon
- Divergence after horizon

Interpretation:  
Model fits history but struggles with future trend → consider adding seasonal features.