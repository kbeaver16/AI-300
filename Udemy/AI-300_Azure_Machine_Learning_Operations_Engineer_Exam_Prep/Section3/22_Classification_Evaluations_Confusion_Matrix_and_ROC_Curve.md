# 22 – Classification Evaluations: Confusion Matrix and ROC Curve

---

## 📌 Key Concepts

- **Confusion Matrix:** a 2×2 table comparing predicted vs. actual labels for binary classifiers
- **True Positive (TP):** model correctly predicted Positive
- **True Negative (TN):** model correctly predicted Negative
- **False Positive (FP):** model predicted Positive but actual was Negative (Type I Error)
- **False Negative (FN):** model predicted Negative but actual was Positive (Type II Error)
- **Recall (Sensitivity / TPR):** TP / (TP + FN) — of all actual positives, how many were caught?
- **False Positive Rate (FPR):** FP / (FP + TN) — of all actual negatives, how many were wrongly flagged?
- **Precision:** TP / (TP + FP) — of all predicted positives, how many are truly positive?
- **Accuracy:** (TP + TN) / (TP + TN + FP + FN) — overall proportion of correct predictions
- **F1-Score:** harmonic mean of Precision and Recall; single balanced metric
- **ROC Curve:** plots TPR (Recall) on Y-axis vs. FPR on X-axis across all classification thresholds
- **Area Under the Curve (AUC):** summary of ROC performance; 1.0 = perfect, 0.5 = random classifier

---

## 📖 Definitions

| Term | Formula | Interpretation |
|------|---------|----------------|
| True Positive (TP) | — | Model said Positive; truth is Positive ✅ |
| True Negative (TN) | — | Model said Negative; truth is Negative ✅ |
| False Positive (FP) | — | Model said Positive; truth is Negative ❌ (Type I Error) |
| False Negative (FN) | — | Model said Negative; truth is Positive ❌ (Type II Error) |
| Recall (TPR) | TP / (TP + FN) | What fraction of actual positives were detected? |
| False Positive Rate | FP / (FP + TN) | What fraction of actual negatives were wrongly flagged? |
| Precision | TP / (TP + FP) | Of predicted positives, what fraction are correct? |
| Accuracy | (TP + TN) / Total | What fraction of all predictions were correct? |
| F1-Score | 2 × (P × R) / (P + R) | Harmonic mean; balanced when classes are imbalanced |
| AUC (Area Under Curve) | 0 → 1 | 1.0 = perfect classifier; 0.5 = random guessing |
| ROC Curve | TPR vs. FPR plot | Visual of classifier performance across all thresholds |

---

## 💡 Examples

- **Confusion Matrix (email spam classifier):**
  - TP: model said "spam" → actual spam ✅
  - TN: model said "not spam" → actual not spam ✅
  - FP: model said "spam" → actual not spam ❌
  - FN: model said "not spam" → actual spam ❌

- **Confusion Matrix (diabetes classifier):**
  - TP: model predicts patient has diabetes → patient truly has diabetes
  - FN: model predicts no diabetes → patient truly has diabetes (dangerous miss!)

- **ROC Curve shapes:**
  - 🔵 Perfect classifier: steeply rises to top-left corner → AUC = 1.0
  - 🟢 Good real-world model: a curve between diagonal and perfect → AUC ~0.75–0.95
  - 🔴 Random / bad classifier: sits along the diagonal → AUC = 0.5

- **F1-Score insight:** if Precision = 0.90 but Recall = 0.40, the F1-Score ~0.56 — revealing the true weakness of the model

---

## 🔢 Step-by-Step Processes

### Computing a Confusion Matrix and Metrics (sklearn)
1. Train and obtain predictions: `y_pred = model.predict(X_test)`
2. Generate confusion matrix:
   ```python
   from sklearn.metrics import confusion_matrix
   cm = confusion_matrix(y_test, y_pred)
   # [[TN, FP], [FN, TP]]
   ```
3. Calculate individual metrics:
   ```python
   from sklearn.metrics import precision_score, recall_score, f1_score, accuracy_score
   print("Precision:", precision_score(y_test, y_pred))
   print("Recall:",    recall_score(y_test, y_pred))
   print("F1-Score:",  f1_score(y_test, y_pred))
   print("Accuracy:",  accuracy_score(y_test, y_pred))
   ```
4. Generate the full classification report:
   ```python
   from sklearn.metrics import classification_report
   print(classification_report(y_test, y_pred))
   ```

### Plotting the ROC Curve
1. Get class probability scores (not binary predictions):
   ```python
   y_scores = model.predict_proba(X_test)[:, 1]
   ```
2. Compute TPR and FPR at all thresholds:
   ```python
   from sklearn.metrics import roc_curve, roc_auc_score
   fpr, tpr, thresholds = roc_curve(y_test, y_scores)
   auc = roc_auc_score(y_test, y_scores)
   ```
3. Plot the curve:
   ```python
   import matplotlib.pyplot as plt
   plt.plot(fpr, tpr, label=f"AUC = {auc:.2f}")
   plt.plot([0,1], [0,1], 'r--', label="Random (AUC=0.5)")
   plt.xlabel("False Positive Rate")
   plt.ylabel("True Positive Rate (Recall)")
   plt.title("ROC Curve")
   plt.legend()
   plt.show()
   ```

---

## 💻 Code Blocks

```python
from sklearn.metrics import (confusion_matrix, classification_report,
                              roc_curve, roc_auc_score)
import matplotlib.pyplot as plt
import seaborn as sns

# 1. Confusion Matrix
cm = confusion_matrix(y_test, y_pred)
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues',
            xticklabels=['Pred: No', 'Pred: Yes'],
            yticklabels=['True: No', 'True: Yes'])
plt.title('Confusion Matrix')
plt.show()

# 2. Full metrics report
print(classification_report(y_test, y_pred))

# 3. ROC Curve + AUC
y_scores = model.predict_proba(X_test)[:, 1]
fpr, tpr, _ = roc_curve(y_test, y_scores)
auc = roc_auc_score(y_test, y_scores)

plt.figure()
plt.plot(fpr, tpr, color='blue', label=f'Model (AUC = {auc:.2f})')
plt.plot([0,1], [0,1], 'r--', label='Random Classifier (AUC = 0.50)')
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('ROC Curve')
plt.legend()
plt.show()
```

---

## 📝 Summary

> "The confusion matrix is used to evaluate the performance of a classification model. It compares actual labels with the model's predicted labels… for much better interpretability and understanding what's going wrong and where."

The confusion matrix turns a model's predictions into a structured 2×2 breakdown of correct and incorrect calls, making failures visible and explainable to non-technical stakeholders. The derived metrics — **Recall, Precision, F1-Score, Accuracy** — each answer a distinct question about model quality. The **ROC Curve** visualises performance across all decision thresholds, and the **AUC** distils it into a single number: the closer to 1.0, the better. These tools are essential for any binary classification problem.

---

## ✅ Actionable Takeaways

- Always generate a confusion matrix alongside accuracy — accuracy alone is misleading on imbalanced datasets
- Focus on **Recall** when the cost of False Negatives is high (e.g. missing a disease diagnosis)
- Focus on **Precision** when the cost of False Positives is high (e.g. wrongly flagging fraud)
- Use **F1-Score** as your headline metric when you need to balance both Precision and Recall
- Plot the **ROC Curve** for every binary classifier; target AUC > 0.75 as a baseline
- Present the confusion matrix to stakeholders — it's the most intuitive way to explain model errors

---

## 🏷️ Tags

`#azure-ml` `#confusion-matrix` `#roc-curve` `#auc` `#precision` `#recall` `#f1-score` `#accuracy` `#classification` `#evaluation` `#binary-classification` `#diabetes` `#model-interpretability`

---

## 🔗 Backlink Suggestions

- [[21 – Lab Training a Diabetes Predictor Model]] — the model evaluated using these metrics
- [[16 – Understanding Linear Algorithms]] — logistic regression is the classifier producing these outputs
- [[14 – Introduction to Statistical and Machine Learning]] — MSE and general model accuracy concepts
- [[23 – Introduction to AutoML]] — AutoML uses AUC as a primary optimisation target for classification
- [[24 – Lab Automated Training with AutoML]] — AutoML leaderboard ranks models by AUC/accuracy
