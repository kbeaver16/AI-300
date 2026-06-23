# 17 – Lab: Training a Cost Function Prediction Model with Designer

---

## 📌 Key Concepts

- **Azure ML Pipeline Designer:** a low-code, drag-and-drop interface for building ML training pipelines
- **Automobile Price Prediction:** a linear regression model predicting car price from feature columns (X)
- **Sample Datasets:** pre-loaded datasets in the Designer's component library
- **Select Columns in Dataset:** component to include or exclude specific columns from training
- **Clean Missing Data:** component to handle null/NaN values (e.g. drop entire rows with any nulls)
- **Split Data:** component to partition a dataset into training (70 %) and testing (30 %) sets
- **Train Model:** component that fits a chosen algorithm on the training data
- **Score Model:** component that generates predictions on the test dataset
- **Evaluate Model:** component that computes evaluation metrics (MSE, R², MAE)
- **Coefficient of Determination (R²):** measures how well the regression line explains variance in Y
- **Experiment / Job:** the experiment organises the run; the job tracks the individual execution

---

## 📖 Definitions

| Term | Definition |
|------|------------|
| Pipeline Designer | Azure ML visual, drag-and-drop authoring tool for ML pipelines |
| Component | A modular building block in the Designer (e.g. Split Data, Train Model) |
| Data Asset | A registered dataset in the Azure ML workspace |
| Normalized Losses | A column in the automobile dataset excluded due to excessive nulls |
| Label Column | The target Y column the model will predict — "price" in this lab |
| Coefficient of Determination (R²) | 0 to 1 scale; higher = regression line explains more variance |
| Mean Absolute Error (MAE) | Average of absolute differences between predictions and true values |
| Root Mean Squared Error (RMSE) | Square root of MSE; in the same units as Y |
| Relative Absolute Error | MAE normalised relative to a simple baseline predictor |
| Managed Compute Instance | Azure VM used to execute the pipeline job |

---

## 💡 Examples

- **Excluded column:** `normalized_losses` — too many null values; dropping it improves model quality
- **Label column:** `price` — the dependent variable to be predicted
- **Evaluation results from the lab:**
  - R² = **0.86** (good fit)
  - MAE = **1,773** (predictions within ~$1,773 of true price on a ~$14k–$16k range)
  - Relative Absolute Error = **0.38**
  - Relative Squared Error = **0.13**
  - RMSE = **2,461**

---

## 🔢 Step-by-Step Processes

### Building the Automobile Price Prediction Pipeline

1. Navigate to **Azure ML Studio → Authoring → Designer**
2. Select **Classic Prebuilt** → **Create a new pipeline**
3. In the left panel, open **Sample Data** → search for **Automobile Price Data** → drag to canvas
4. Add **Select Columns in Dataset** → connect from dataset → configure:
   - Include: All Columns
   - Exclude: `normalized_losses`
5. Add **Clean Missing Data** → connect → configure:
   - Cleaning Mode: **Remove entire row** (drop any row with a null in any column)
6. Add **Split Data** → connect → configure:
   - Fraction for first output: **0.7** (70 % training / 30 % testing)
7. Add **Linear Regression** component (algorithm)
8. Add **Train Model** → connect:
   - Input 1 ← Linear Regression
   - Input 2 ← Split Data (output 1 = training set)
   - Label Column: `price`
9. Add **Score Model** → connect:
   - Input 1 ← Train Model (trained model)
   - Input 2 ← Split Data (output 2 = testing set)
10. Add **Evaluate Model** → connect Input 1 ← Score Model
11. Rename pipeline to **automobile_pipeline** → Save
12. Click ⋮ → **Configure and Submit** → create experiment **automobile price prediction** → select compute instance → Submit
13. Monitor job in **Jobs** section under Assets; wait for green ticks on all components
14. Review metrics in the **Evaluate Model** component → Outputs + Logs → Metrics tab

---

## 💻 Code Blocks

*This lab is performed entirely in the Azure ML Pipeline Designer (no-code/low-code interface). No Python scripts are required for training. However, the model artifacts include a generated `score.py` scoring script that can be reviewed in the Train Model → Outputs → trained_model_outputs folder.*

```python
# score.py (auto-generated — for reference only)
# Located in: Train Model → Outputs → trained_model_outputs/score.py
# Used when the model is deployed to a real-time or batch endpoint
```

---

## 📝 Summary

> "You'll also get comfortable with having an experience on the low code, drag and drop features that Azure Machine Learning pipeline designer has to build my Azure machine learning pipeline."

This lab walks through building a complete supervised regression pipeline in the Azure ML Designer — from raw data ingestion to model evaluation — without writing a single line of Python. Key steps include excluding noisy columns, cleaning nulls, splitting data 70/30, training a linear regression model, scoring predictions on the test set, and reviewing evaluation metrics. The resulting R² of 0.86 confirms a strong model fit for automobile price prediction.

---

## ✅ Actionable Takeaways

- Always inspect the dataset first — identify and exclude high-null columns like `normalized_losses`
- Use **Remove Entire Row** as the null-cleaning strategy when null rows are a small fraction of data
- Set the **Train/Test split to 0.7** (70 % train) as the standard starting ratio
- Always verify the **label column** name (case-sensitive!) in the Train Model component
- Connect **Split Data output 1** (training) to Train Model and **output 2** (test) to Score Model
- Check R² first for regression: values above 0.8 indicate a strong model
- After training, save the pipeline and note the experiment name for use in the inference pipeline (Lab 18)

---

## 🏷️ Tags

`#azure-ml` `#pipeline-designer` `#linear-regression` `#lab` `#automobile-price` `#low-code` `#regression` `#evaluation` `#r-squared` `#mse` `#supervised-learning` `#data-cleaning`

---

## 🔗 Backlink Suggestions

- [[14 – Introduction to Statistical and Machine Learning]] — theory behind regression and MSE
- [[16 – Understanding Linear Algorithms]] — explains the linear regression algorithm used here
- [[18 – Lab Creating a Realtime Inference Pipeline]] — next step: convert this trained pipeline to a real-time endpoint
- [[19 – Deploying the Model to Azure Container Instance]] — deployment and API testing
- [[20 – Anatomy of Azure ML Jobs Experiments and Runs]] — explains the experiment/job structure seen in this lab
