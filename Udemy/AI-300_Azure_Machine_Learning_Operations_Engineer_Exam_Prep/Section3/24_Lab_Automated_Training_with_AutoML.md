# 24 – Lab: Automated Training with AutoML

---

## 📌 Key Concepts

- **AutoML Job:** a no-code automated training run submitted from the Azure ML Studio UI
- **Automated ML Tab:** the dedicated Azure ML Studio section for configuring and submitting AutoML jobs
- **Task Type Selection:** AutoML supports classification, regression, time-series forecasting, NLP, and computer vision
- **Target Column:** the Y label column AutoML optimises predictions for — `outcome` (Boolean) in this lab
- **ML Table Data Asset:** the registered diabetes dataset used as both training and testing source
- **Experiment Scoping:** AutoML automatically creates an experiment named `automl-experiment` grouping all algorithm trials as jobs
- **Multiple Child Jobs:** each algorithm × hyperparameter combination AutoML tries appears as an individual job under the experiment
- **Best Performing Asset:** the single winning model from all trials; selected based on the target metric
- **Classification Task:** chosen because the outcome column is Boolean (True/False → binary classification)
- **No Code Required:** the entire AutoML configuration is performed through the Azure ML UI wizard

---

## 📖 Definitions

| Term | Definition |
|------|------------|
| Automated ML Tab | Azure ML Studio section for submitting and monitoring AutoML jobs without writing code |
| New Automated ML Job | Wizard-driven flow to configure and launch an AutoML training run |
| Training Method | For AutoML: "Train automatically" — no manual algorithm selection required |
| Task Type | The ML problem category: Classification, Regression, Time Series, NLP, or Computer Vision |
| Target Column | The label/Y column the AutoML job trains to predict (`outcome` in this lab) |
| Outcome Column | Boolean column in the diabetes dataset: True = has diabetes, False = does not have diabetes |
| Child Job | An individual AutoML trial; one algorithm + hyperparameter combination per child job |
| Best Performing Model | The AutoML trial with the highest value on the chosen optimisation metric |
| AutoML Experiment | The experiment container in Azure ML that groups all child jobs from one AutoML run |
| Managed Compute Instance | The Azure VM on which the AutoML job is executed |

---

## 💡 Examples

- **Dataset used:** diabetes ML Table (same one from Lab 21)
- **Task type selected:** Classification (because `outcome` is True/False — binary)
- **Target column:** `outcome`
- **Experiment name:** `automl-experiment`
- **Outcome:** AutoML produces a leaderboard of multiple algorithms (e.g. Logistic Regression, Random Forest, XGBoost, LightGBM, etc.) ranked by AUC or accuracy
- **Best model:** the top-ranked model from the leaderboard, ready to deploy as a real-time endpoint

---

## 🔢 Step-by-Step Processes

### Submitting an AutoML Job via Azure ML Studio UI

1. Go to **Azure ML Studio → Automated ML** tab (under Authoring)
2. Click **New Automated ML Job**
3. **Training Method:** select `Train automatically (AutoML)` → click Next
4. **Basic Settings:**
   - Create a new experiment: `automl-experiment`
   - Job display name: (accept default) → click Next
5. **Task Type and Data:**
   - Task type: **Classification**
   - Dataset: select `diabetes-mltable` (the ML Table registered in a previous lab)
   - Click Next
6. **Task Settings:**
   - Review the ML Table columns loaded by AutoML
   - Set **Target Column** to `outcome` (Boolean: True/False)
   - Click Next
7. **Compute:**
   - Compute type: **Compute Instance**
   - Select your managed compute instance
   - Click Next → Submit
8. **Monitor the run:**
   - Go to **Jobs** section → find `automl-experiment`
   - Observe multiple **child jobs** appearing — each one is a different algorithm trial
   - Wait for the AutoML run to complete
9. **Review the Leaderboard:**
   - Open the completed AutoML experiment
   - The leaderboard shows all model trials ranked by the target metric
10. **Select the Best Model:**
    - Click the top-ranked model
    - Click **Register Model** → saves to the Model Registry
11. **Deploy (optional, follow same steps as Labs 18–19):**
    - From the Model Registry, deploy to a **Real-Time Managed Endpoint**

---

## 💻 Code Blocks

```python
# Equivalent AutoML job using Azure ML SDK v2 (for reference)
from azure.ai.ml import MLClient, automl, Input
from azure.ai.ml.constants import AssetTypes
from azure.identity import DefaultAzureCredential

ml_client = MLClient(
    DefaultAzureCredential(),
    subscription_id="<your-subscription-id>",
    resource_group_name="<your-resource-group>",
    workspace_name="<your-workspace-name>"
)

# Reference the registered ML Table
training_data = Input(
    type=AssetTypes.MLTABLE,
    path="azureml:diabetes-mltable:1"
)

# Configure AutoML classification job
classification_job = automl.classification(
    compute="<your-compute-instance-name>",
    experiment_name="automl-experiment",
    training_data=training_data,
    target_column_name="outcome",
    primary_metric="AUCWeighted",   # or "Accuracy"
)

# Set time/trial constraints
classification_job.set_limits(
    timeout_minutes=15,
    trial_timeout_minutes=5,
    max_trials=20,
    enable_early_termination=True,
)

# Submit the job
returned_job = ml_client.jobs.create_or_update(classification_job)
ml_client.jobs.stream(returned_job.name)  # Stream logs

# Retrieve the best model
best_run = ml_client.jobs.get(returned_job.name)
print(f"Best model run ID: {best_run.name}")
```

```python
# Retrieve and register the best AutoML model after the run
from azure.ai.ml.entities import Model
from azure.ai.ml.constants import AssetTypes

# Register the best model
model = ml_client.models.create_or_update(
    Model(
        path=f"azureml://jobs/{returned_job.name}/outputs/best_model",
        name="automl-diabetes-classifier",
        type=AssetTypes.MLFLOW_MODEL,
    )
)
print(f"Registered model: {model.name}, version: {model.version}")
```

---

## 📝 Summary

> "Submit an automated ML job to train a model without writing a single line of code."

This lab applies AutoML in practice to the same diabetes classification problem from Lab 21, but without writing any sklearn code. Through the Azure ML Studio UI wizard, the user configures the experiment name, task type (classification), dataset (diabetes ML Table), and target column (`outcome`). AutoML then launches multiple child jobs — each testing a different algorithm × hyperparameter combination — and presents a ranked leaderboard. The best performing model is selected, registered, and ready for deployment. This demonstrates how AutoML compresses the full manual ML workflow into a guided, no-code experience.

---

## ✅ Actionable Takeaways

- Ensure the diabetes ML Table data asset is already registered before starting the AutoML job (from Lab 21)
Select **Classification** as the task type — `outcome` is Boolean (True/False), not a continuous value
- Always set a time constraint (`timeout_minutes`) to prevent the AutoML job from running indefinitely
- After the run completes, **review the full leaderboard** — don't automatically accept the top model without checking AUC values and explainability
- **Register the best model** from the leaderboard in the Model Registry before deploying
- Compare AutoML's best model AUC against the manually trained logistic regression (Lab 21) F1 ~0.76 — AutoML may yield a higher score by trying more complex algorithms (e.g. XGBoost, LightGBM)
- Use the same endpoint deployment steps (Labs 18–19) to serve the AutoML winner as a real-time API

---

## 🏷️ Tags

`#azure-ml` `#automl` `#lab` `#classification` `#diabetes` `#no-code` `#leaderboard` `#model-registry` `#best-model` `#automated-training` `#mltable` `#binary-classification`

---

## 🔗 Backlink Suggestions

- [[23 – Introduction to AutoML]] — conceptual foundation for this lab
- [[21 – Lab Training a Diabetes Predictor Model]] — manual version of the same classification task
- [[22 – Classification Evaluations Confusion Matrix and ROC Curve]] — metrics used by AutoML to rank models on the leaderboard
- [[20 – Anatomy of Azure ML Jobs Experiments and Runs]] — AutoML child jobs appear in the same Jobs → Experiments view
- [[18 – Lab Creating a Realtime Inference Pipeline]] — deploy the AutoML winner using the same inference pipeline approach
- [[19 – Deploying the Model to Azure Container Instance]] — ACI deployment applies equally to AutoML-generated models
