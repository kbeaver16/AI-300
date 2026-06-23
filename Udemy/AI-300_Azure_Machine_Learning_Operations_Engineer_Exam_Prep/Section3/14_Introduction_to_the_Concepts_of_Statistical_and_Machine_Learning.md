# 14 – Introduction to the Concepts of Statistical and Machine Learning

---

## 📌 Key Concepts

- The goal of ML is to **find the relationship** between input (X) variables and an output (Y) variable
- **Target / Label variable (Y):** the value to be predicted (dependent variable)
- **Features / Independent variables (X):** the inputs that influence Y
- **Regression line:** the learned function that describes the X→Y relationship
- **Model accuracy** is measured by comparing predictions to real (test) data
- **Mean Squared Error (MSE):** the primary metric for quantifying prediction error
- **Bias–Variance Tradeoff:** the central tension in building well-generalised models
- **Overfitting (High Variance):** model fits training data too closely; poor test performance
- **Underfitting (High Bias):** model is too simple; poor performance on both train and test
- **K-Fold Cross Validation:** the standard technique for estimating true model performance
- **Leave-One-Out Cross Validation (LOOCV):** exhaustive but computationally expensive alternative

---

## 📖 Definitions

| Term | Definition |
|------|------------|
| Target Variable (Y) | The dependent variable the model is trained to predict |
| Features (X) | Independent variables supplied as inputs to the model |
| Regression Line | The fitted line/curve describing the relationship between X and Y |
| Mean Squared Error (MSE) | Average of squared differences between predicted and true values |
| Bias | Error from oversimplifying the model (ignoring relevant features) |
| Variance | Error from overfitting; model too sensitive to training-data noise |
| Overfitting | Model performs well on training data but poorly on unseen test data |
| Underfitting | Model is too simple to capture the true signal in the data |
| K-Fold Cross Validation | Dataset split into K folds; model trained K times, each time holding out one fold for testing |
| LOOCV | Special case of cross validation where n − 1 rows are used for training and 1 for testing |
| Feature Engineering | Process of preparing, scaling, and creating features to improve model quality |

---

## 💡 Examples

- **Sales prediction:** Y = Sales; X₁ = TV ads, X₂ = Radio ads, X₃ = Newspaper ads → regression line shows how ad spend drives sales
- **Bias example:** Only modelling Sales against TV ads while ignoring Radio and Newspaper → high-bias (underfit) model
- **Variance example:** A curvilinear regression line that perfectly follows every data point in training → high-variance (overfit) model
- **K-Fold (K=5, 100 rows):** 5 splits of 20 rows each; 5 training/testing cycles; final performance = average of 5 MSEs

---

## 🔢 Step-by-Step Processes

### Model Training Lifecycle
1. **Data Collection** – gather raw data
2. **Data Exploration** – understand distributions, relationships, outliers
3. **Data Cleansing** – handle nulls, duplicates, inconsistencies
4. **Feature Engineering** – scale variables, create aggregates, encode categoricals
5. **Train/Test Split** – typically 70 % training / 30 % testing (or 80/20)
6. **Model Training** – fit the model on the training set
7. **Model Evaluation** – compute MSE, accuracy, etc. on the test set
8. **Deployment** – host model behind an API endpoint for real-time inference

### K-Fold Cross Validation (K = 5)
1. Split 100 rows into 5 equal folds of 20 rows
2. **Cycle 1:** train on folds 2–5, test on fold 1 → Error₁
3. **Cycle 2:** train on folds 1, 3–5, test on fold 2 → Error₂
4. Repeat for cycles 3, 4, 5
5. **Final MSE = mean(Error₁ … Error₅)**

---

## 💻 Code Blocks

*No code was demonstrated in this conceptual lecture. See labs (videos 17, 21) for implementation.*

---

## 📝 Summary

> "All we want to derive from this lecture and video is to just sort of develop an intuition behind what is happening and why is it happening."

This lecture establishes the foundational intuition for statistical and machine learning. A model learns the relationship between feature variables (X) and a target variable (Y). Model quality is measured by MSE — the smaller the gap between predicted and actual values, the better. The **bias–variance tradeoff** is the core challenge: overly complex models overfit (high variance), while overly simple models underfit (high bias). The sweet spot — **optimal model complexity** — minimises both training and testing error. K-Fold cross validation is the practical, industry-standard method for reliably estimating this performance.

---

## ✅ Actionable Takeaways

- Always split data into training and testing sets before modelling (70/30 or 80/20)
- Use **K-Fold cross validation** (not a single test split) for reliable performance estimates
- Monitor both training error **and** test error — a large gap signals overfitting
- Prefer **low-bias, low-variance** models; avoid extremes of complexity
- Perform feature engineering (scaling, encoding) before training
- Use MSE as your primary regression error metric; aim to minimise it

---

## 🏷️ Tags

`#azure-ml` `#machine-learning` `#statistics` `#bias-variance-tradeoff` `#overfitting` `#underfitting` `#cross-validation` `#mse` `#regression` `#feature-engineering` `#supervised-learning` `#foundations`

---

## 🔗 Backlink Suggestions

- [[15 – Introduction to ML Libraries]] — tools used to implement these concepts
- [[16 – Understanding Linear Algorithms]] — first algorithm covered builds directly on this theory
- [[17 – Lab Training a Cost Function Prediction Model with Designer]] — hands-on application of regression
- [[21 – Lab Training a Diabetes Predictor Model]] — applies train/test split and feature scaling covered here
- [[22 – Classification Evaluations Confusion Matrix and ROC Curve]] — expands model evaluation metrics
- [[23 – Introduction to AutoML]] — automates the model selection process described in this lecture
