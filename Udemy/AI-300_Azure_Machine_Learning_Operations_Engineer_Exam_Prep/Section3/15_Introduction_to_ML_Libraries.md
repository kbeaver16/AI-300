# 15 – Introduction to ML Libraries

---

## 📌 Key Concepts

- **Jupyter Notebooks** are the primary development environment for all ML experiments in this course
- The ML ecosystem is divided into: data processing, model training, and visualisation layers
- **Scikit-Learn** handles all classical (linear & non-linear) supervised ML models
- **PyTorch** is the deep-learning framework used exclusively for neural network / deep learning models
- **Pandas & NumPy** handle tabular data manipulation and numerical operations
- **Matplotlib & Seaborn** handle visualisation and graph generation
- **Hugging Face Transformers / TensorFlow** are out-of-scope for this course (used for LLMs/transformers)
- **Spark** is available for large-scale distributed data processing alongside Pandas

---

## 📖 Definitions

| Library / Framework | Role |
|---------------------|------|
| Jupyter Notebooks | Interactive Python notebook environment for running ML experiments |
| Scikit-Learn (sklearn) | Classical ML library — regression, classification, clustering, evaluation |
| PyTorch | Deep learning framework for training neural networks |
| Pandas | DataFrame-based library for data manipulation and cleaning |
| NumPy | Numerical computing library; underpins most scientific Python |
| Matplotlib | Core plotting library for 2-D charts and graphs |
| Seaborn | Statistical visualisation library built on top of Matplotlib |
| Apache Spark | Distributed data processing engine for big-data workloads |
| Hugging Face Transformers | Library for training and using transformer-based LLMs (out of scope) |
| TensorFlow | Alternative deep-learning framework (out of scope for this course) |

---

## 💡 Examples

- **Regression model** → trained with `sklearn.linear_model.LinearRegression`
- **Classification model** → trained with `sklearn.linear_model.LogisticRegression`
- **Deep learning model** → trained with `torch.nn` (PyTorch)
- **Plotting model metrics** → `matplotlib.pyplot.plot(...)` or `seaborn.heatmap(...)`
- **Data loading & cleaning** → `pandas.read_csv(...)`, `df.dropna()`

---

## 🔢 Step-by-Step Processes

### Typical Lab Workflow (Course Pattern)
1. Open a **Jupyter Notebook** in Azure ML workspace
2. Load and process data using **Pandas** (and optionally **Spark** for large datasets)
3. Perform feature engineering with **NumPy** / Pandas
4. Train the model using **Scikit-Learn** (classical) or **PyTorch** (deep learning)
5. Evaluate and visualise results with **Matplotlib** and **Seaborn**
6. Log experiments and metrics with **MLflow** (integrated into Azure ML)

---

## 💻 Code Blocks

```python
# Common imports used throughout the course labs

# Data processing
import pandas as pd
import numpy as np

# Model training (classical ML)
from sklearn.linear_model import LinearRegression, LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import MinMaxScaler
from sklearn.metrics import classification_report

# Visualisation
import matplotlib.pyplot as plt
import seaborn as sns

# Deep learning (when applicable)
import torch
import torch.nn as nn
```

---

## 📝 Summary

> "Most labs in the course will utilize Spark and pandas for data processing, Scikit-Learn and PyTorch for model development, and matplotlib library and seaborn for visualization and a lot of Python scripting."

This lecture maps the complete ML library ecosystem used throughout the course. The core stack is: **Pandas/NumPy** for data wrangling, **Scikit-Learn** for classical supervised learning, **PyTorch** for deep learning, and **Matplotlib/Seaborn** for visualisation — all running inside **Jupyter Notebooks** on Azure ML. Transformer-based LLM frameworks (Hugging Face, TensorFlow) are explicitly noted as out-of-scope.

---

## ✅ Actionable Takeaways

- Ensure Jupyter Notebook is connected to your Azure ML managed compute instance before running any lab
- Use **Scikit-Learn** for regression and classification tasks (not PyTorch — that is reserved for deep learning)
- Use **Pandas DataFrames** as the standard data structure; most sklearn functions integrate with them directly
- Import **Matplotlib/Seaborn** for every lab to visualise training results and evaluation metrics
- Do **not** attempt to install or use Hugging Face Transformers or TensorFlow — they are out of scope
- Select the **Azure ML SDK v2 Python kernel** when attaching a notebook to a compute instance

---

## 🏷️ Tags

`#azure-ml` `#python` `#libraries` `#scikit-learn` `#pytorch` `#pandas` `#numpy` `#matplotlib` `#seaborn` `#jupyter` `#deep-learning` `#tooling` `#course-setup`

---

## 🔗 Backlink Suggestions

- [[14 – Introduction to Statistical and Machine Learning]] — theoretical foundations these libraries implement
- [[16 – Understanding Linear Algorithms]] — first use of Scikit-Learn concepts
- [[17 – Lab Training a Cost Function Prediction Model with Designer]] — lab using Azure ML Designer (low-code alternative)
- [[21 – Lab Training a Diabetes Predictor Model]] — full sklearn lab (LogisticRegression, MinMaxScaler, classification_report)
- [[24 – Lab Automated Training with AutoML]] — AutoML abstracts away direct library usage
