# 23 – Introduction to AutoML

---

## 📌 Key Concepts

- **Automated Machine Learning (AutoML):** a system that automatically selects algorithms, engineers features, tunes hyperparameters, and evaluates models to find the best-performing candidate
- **Target Metric:** the optimisation objective supplied by the user (e.g. AUC, accuracy, MSE, precision, recall)
- **Constraints:** limits on training time (e.g. max 15 minutes) and/or target metric threshold (e.g. stop when accuracy ≥ 90 %)
- **Leaderboard:** the ranked list of model + hyperparameter combinations tried by AutoML, sorted by the target metric
- **Top Model Selection:** the best model from the leaderboard is promoted for registration and deployment
- **Feature Engineering (AutoML):** data cleaning, scaling, and normalisation are handled automatically — no manual intervention needed
- **Hyper-parameter Tuning (AutoML):** AutoML iterates over combinations of algorithm × hyperparameter values automatically
- **Model Registry:** where the winning AutoML model is registered before deployment to an endpoint
- **Compute Time Saving:** early-stopping based on the target metric prevents wasting compute on marginal improvements

---

## 📖 Definitions

| Term | Definition |
|------|------------|
| AutoML | Automated pipeline that trains, evaluates, and ranks multiple ML models without manual code |
| Target Metric | The metric AutoML tries to maximise or minimise (AUC, accuracy, MSE, precision, recall) |
| Constraint | A stopping rule: max training time or target metric threshold |
| Leaderboard | Sorted table of all algorithm × hyperparameter combinations tried during AutoML; ranked by target metric |
| Feature Engineering (AutoML) | Automatic data cleaning, encoding, and scaling performed before each model trial |
| Hyperparameter Tuning (AutoML) | Systematic search over hyperparameter combinations for each algorithm |
| Model Registry | Azure ML versioned model store; AutoML registers the winning model here after the run |
| AUC | Area Under the ROC Curve; primary target metric for classification tasks |
| MSE | Mean Squared Error; primary target metric for regression tasks |

---

## 💡 Examples

- **Classification task:** AutoML optimises for AUC or accuracy
- **Regression task:** AutoML optimises for lower MSE or higher R²
- **Time constraint example:** `max_experiment_timeout_minutes = 15` → AutoML stops after 15 minutes regardless of whether a better model might still be found
- **Metric threshold example:** stop training as soon as accuracy ≥ 90 % even if better models are possible — saves compute cost for production approval

---

## 🔢 Step-by-Step Processes

### How AutoML Works (High Level)
1. **User provides:**
   - Labelled dataset (registered as a data asset in Azure ML)
   - Target column (Y variable)
   - Task type (classification / regression / forecasting / NLP / vision)
   - Target metric to optimise (AUC, accuracy, MSE, etc.)
   - Constraints (max training time, target metric threshold)
2. **AutoML pipeline:**
   - Reads user inputs and infers model type
   - Applies automatic feature engineering (cleaning, scaling, encoding)
   - Iterates through combinations of algorithms × hyperparameters
   - Evaluates each combination on a validation set
   - Builds a **leaderboard** of all trials, ranked by target metric
3. **User action:**
   - Reviews the leaderboard
   - Selects the top model
   - Registers the model in the **Model Registry**
   - Deploys to a real-time managed endpoint

### Key User Responsibilities
- Upload and register the dataset as an **ML Table** data asset
- Choose the correct **task type** (classification vs. regression)
- Select the most meaningful **target metric** for the business problem
- Set appropriate **time/cost constraints** to control compute spend

---

## 💻 Code Blocks

```python
# Submitting an AutoML job via Azure ML SDK v2

from azure.ai.ml import MLClient, automl, Input
from azure.ai.ml.constants import AssetTypes
from azure.identity import DefaultAzureCredential

ml_client = MLClient(DefaultAzureCredential(), subscription_id, resource_group, workspace_name)

# Define the training data input
training_data = Input(type=AssetTypes.MLTABLE, path="azureml:diabetes-mltable:1")

# Configure the AutoML classification job
classification_job = automl.classification(
    compute="my-compute-instance",
    experiment_name="automl-experiment",
    training_data=training_data,
    target_column_name="outcome",
    primary_metric="AUCWeighted",
    n_cross_validations=5,
    enable_model_explainability=True,
)

# Set constraints
classification_job.set_limits(
    timeout_minutes=15,
    trial_timeout_minutes=5,
    max_trials=20,
    enable_early_termination=True,
)

# Submit the job
returned_job = ml_client.jobs.create_or_update(classification_job)
print(f"AutoML job submitted: {returned_job.name}")
```

---

## 📝 Summary

> "Automated machine learning automatically builds and tunes ML models. It reduces manual effort in model selection, feature engineering, and hyperparameter tuning."

AutoML removes the manual trial-and-error of the traditional ML workflow. Instead of the practitioner hand-crafting each algorithm choice, hyperparameter grid, and feature scaling pipeline, AutoML explores this space automatically and surfaces the best model on a ranked leaderboard. The user's only responsibility is to provide clean (or let AutoML clean) labelled data, define the task, choose the target metric, and set cost/time guardrails. The winning model is then registered and deployed exactly like any manually trained model.

---

## ✅ Actionable Takeaways

- Always register your dataset as an **ML Table** data asset in Azure ML before starting an AutoML job
- Choose **AUC** as the primary metric for classification tasks; choose **MSE** or **RMSE** for regression
- Set a `timeout_minutes` constraint to prevent runaway compute costs — 15–30 minutes is practical for most course labs
- Review the **leaderboard** after the run: check multiple top models before committing to the winner
- Use the AutoML-generated **model explanation** feature to understand which features drove the winning model's predictions
- Register the top model from the leaderboard in the **Model Registry** before deploying

---

## 🏷️ Tags

`#azure-ml` `#automl` `#automated-machine-learning` `#hyperparameter-tuning` `#feature-engineering` `#leaderboard` `#classification` `#regression` `#model-registry` `#no-code` `#auc` `#mse`

---

## 🔗 Backlink Suggestions

- [[14 – Introduction to Statistical and Machine Learning]] — AutoML automates the model training lifecycle described here
- [[16 – Understanding Linear Algorithms]] — AutoML tries linear algorithms (and many others) automatically
- [[21 – Lab Training a Diabetes Predictor Model]] — AutoML can replace the manual sklearn workflow in that lab
- [[22 – Classification Evaluations Confusion Matrix and ROC Curve]] — AutoML uses AUC (from ROC) as its classification metric
- [[24 – Lab Automated Training with AutoML]] — hands-on implementation of the AutoML concepts from this video
- [[20 – Anatomy of Azure ML Jobs Experiments and Runs]] — AutoML jobs appear in the same experiment/job hierarchy
