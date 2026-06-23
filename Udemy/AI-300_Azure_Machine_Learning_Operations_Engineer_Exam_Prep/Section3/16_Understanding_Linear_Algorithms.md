# 16 – Understanding Linear Algorithms

---

## 📌 Key Concepts

- **Linear relationship:** a relationship between Y and X expressible as a polynomial of degree 1 (no X², X³)
- **Linear Regression:** predicts **continuous** output values (price, temperature, stock price)
- **Logistic Regression:** predicts **discrete / binary** output values (0 or 1, class membership)
- **Shrinkage Regression Methods:** Ridge (L2) and Lasso (L1) — linear regression regularised to reduce overfitting
- **Hyperparameter (λ):** tuning parameter in Ridge/Lasso that penalises large coefficients
- **Ridge Regression (L2):** penalty = λ × Σ(βᵢ²) — shrinks coefficients, never to exactly zero
- **Lasso Regression (L1):** penalty = λ × Σ|βᵢ| — can shrink coefficients all the way to zero (feature selection)
- **Sigmoid function:** the non-linear activation at the core of logistic regression
- **Log-odds / Logit:** the linear transformation inside logistic regression: log(p / 1−p) = β₀ + β₁x

---

## 📖 Definitions

| Term | Definition |
|------|------------|
| Linear Combination | Expression of the form θ₁ + θ₂x; degree-1 polynomial |
| Intercept (β₀ / θ₁) | Value of Y when all X variables are zero |
| Coefficient (β / θ₂) | Slope: how much Y changes per unit change in X |
| Continuous Values | Uncountable range (e.g. house price: $1M → $10M) → use Linear Regression |
| Discrete Values | Countable categories (e.g. 0 or 1) → use Logistic Regression |
| Regularisation | Adding a penalty term to MSE to prevent overfitting |
| Lambda (λ) | Hyperparameter controlling the strength of the regularisation penalty |
| Ridge (L2) | Penalises sum of squared coefficients; coefficients approach but never reach zero |
| Lasso (L1) | Penalises sum of absolute coefficients; can zero out irrelevant features |
| Sigmoid Function | S-shaped function mapping any real input to (0, 1); basis of logistic regression |
| MLflow | Open-source library deeply integrated with Azure ML for hyperparameter logging |

---

## 💡 Examples

- **Linear Regression — house price prediction:**
  `Y = β₀ + β₁(size) + β₂(location) + β₃(bedrooms)`
- **Logistic Regression — pumpkin colour:**
  Output: orange (1) or not orange (0) — discrete binary outcome
- **Logistic Regression — fraud detection:**
  Transaction is fraudulent (1) or genuine (0)
- **Logistic Regression — medical diagnosis:**
  Patient has condition A, B, or C (multi-class)
- **Lasso risk:** if λ is set too high, useful feature coefficients can be zeroed out → underfitting

---

## 🔢 Step-by-Step Processes

### Choosing the Right Algorithm
1. Is the output **continuous** (price, temperature)? → **Linear Regression**
2. Is the output **binary or categorical** (yes/no, class A/B/C)? → **Logistic Regression**
3. Is the linear model overfitting (high variance)? → Apply **Ridge or Lasso** regularisation
4. Fine-tune the **lambda (λ) hyperparameter** using MLflow hyperparameter logging

### Ridge Regression Workflow
1. Train simple linear regression → observe high coefficients / overfitting
2. Add Ridge penalty: `MSE_ridge = MSE + λ × Σ(βᵢ²)`
3. Tune λ; higher λ = stronger penalty, smaller coefficients
4. Evaluate test MSE at different λ values; select optimal λ

### Lasso Regression Workflow
1. Same as Ridge, but penalty = `λ × Σ|βᵢ|`
2. Coefficients can reach exactly 0 → automatic feature selection
3. Caution: too-high λ removes important features → underfitting

---

## 💻 Code Blocks

```python
from sklearn.linear_model import LinearRegression, Ridge, Lasso, LogisticRegression

# Linear Regression
lr = LinearRegression()
lr.fit(X_train, y_train)

# Ridge Regression (L2)
ridge = Ridge(alpha=1.0)   # alpha = lambda
ridge.fit(X_train, y_train)

# Lasso Regression (L1)
lasso = Lasso(alpha=0.1)   # careful: high alpha zeros out coefficients
lasso.fit(X_train, y_train)

# Logistic Regression (classification)
log_reg = LogisticRegression(max_iter=1000, C=0.3)
log_reg.fit(X_train, y_train)
```

```python
# Sigmoid function (illustration)
import numpy as np
def sigmoid(x):
    return 1 / (1 + np.exp(-x))
```

---

## 📝 Summary

> "Linear algorithms mean linear combinations of features — something like the equation of a straight line… y = β₀ + β₁x₁ + … + βₙxₙ."

This lecture covers the full family of linear supervised ML algorithms. **Linear regression** models continuous targets as weighted sums of features. **Logistic regression** wraps a linear combination in a sigmoid to produce class probabilities for discrete targets. Both can overfit when coefficients grow too large; **Ridge** and **Lasso** regularisation solve this by penalising large weights. The key tuning lever is the **lambda hyperparameter** — log it with MLflow to find the sweet spot between underfitting and overfitting.

---

## ✅ Actionable Takeaways

- Always ask: is my target continuous or discrete? That determines Linear vs. Logistic regression
- If your linear model overfits, apply Ridge or Lasso before switching algorithms
- Use MLflow to log experiments across different λ values and compare test MSE
- With Lasso, start with a small λ to avoid accidentally zeroing out important features
- Encode string/categorical features into numerical indices before feeding them to any linear model
- Remember: Logistic Regression is a **classification** model, despite the word "regression" in its name

---

## 🏷️ Tags

`#azure-ml` `#machine-learning` `#linear-regression` `#logistic-regression` `#ridge` `#lasso` `#regularisation` `#hyperparameters` `#supervised-learning` `#classification` `#regression` `#mlflow` `#sigmoid`

---

## 🔗 Backlink Suggestions

- [[14 – Introduction to Statistical and Machine Learning]] — bias-variance tradeoff motivates regularisation
- [[15 – Introduction to ML Libraries]] — sklearn implements all algorithms covered here
- [[17 – Lab Training a Cost Function Prediction Model with Designer]] — hands-on linear regression lab
- [[21 – Lab Training a Diabetes Predictor Model]] — hands-on logistic regression lab
- [[22 – Classification Evaluations Confusion Matrix and ROC Curve]] — evaluating logistic regression output
- [[23 – Introduction to AutoML]] — AutoML automates algorithm selection across this family
