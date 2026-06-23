# DAY 08 — MODULE 6
Run pipelines in Azure Machine Learning

## MODULE LINK
- https://learn.microsoft.com/en-us/training/modules/run-pipelines-azure-machine-learning/

## TASKS
- [x] Complete module ✅ 2026-06-05
- [x] Learn pipeline jobs ✅ 2026-06-05
- [x] Study scheduling ✅ 2026-06-05

## KEY CONCEPTS
- Pipeline components
- Dependency chaining

---
## Lab overview
**Goal:**  
Create a **two‑step Azure ML pipeline** (data prep → model training), submit it as a **pipeline job**, and inspect the run, outputs, and lineage.

**You will:**
- Define **command components** for data preparation and training.
- Wire them together in a **DSL pipeline**.
- Submit the pipeline job from a notebook.
- Monitor the run in Azure ML studio and compare runs with different parameters.

---

### 0. Prerequisites
Before you start, make sure you have:
- **Azure ML workspace** created and accessible.
- **Compute cluster** (CPU) in that workspace, e.g. `cpu-cluster`.
- **Compute instance** for notebooks (or use VS Code with Azure ML connection).
- **Dataset** registered in the workspace (e.g. a tabular dataset named `diabetes-data`).
- **Python environment** with:
    - `azure-ai-ml`
    - `azure-identity`
    - `pandas`
    - `scikit-learn`
> Tip: If you use a compute instance, most of this is already available or easy to install via `pip`.

---

## 🔧 Step 0 — Full Environment Setup for the Pipeline Lab
Everything below is required **once** before you can run the pipeline job.

### 01 - Create a Resource Group
This isolates all your AI‑300 lab resources so you can clean them up easily later.

Azure Portal → Search: Resource groups → Create
- Choose a name like **rg-ai300-labs**
- Region: pick the same region you’ll use for Azure ML (e.g., _East US_, _West US 2_)
- Click **Review + Create** → **Create**
    

### 02 - Create an Azure Machine Learning Workspace
This is the control plane for all ML assets: compute, data, jobs, models, pipelines.

Azure Portal → Search: Azure Machine Learning → Create
- Subscription: your subscription
- Resource group: **rg-ai300-labs**
- Workspace name: **mlw-ai300**
- Region: same as resource group
- Leave default storage, key vault, registry, and app insights
- Click **Review + Create** → **Create**
    
### 03 - Open Azure ML Studio
This is where you’ll run jobs, create components, and monitor pipelines.
https://ml.azure.com → Select workspace: mlw-ai300
- Bookmark the workspace for quick access
- Confirm you can see: **Data**, **Compute**, **Jobs**, **Models**, **Environments**, **Components**

### 04 - Create a Compute Cluster (for pipeline steps)
Required for pipelines
Pipeline components run on compute clusters, not compute instances.
Azure ML Studio → Compute → Compute clusters → + New
- Name: **cpu-cluster**
- VM size: **Standard_DS11_v2** (safe + inexpensive)
- Min nodes: **0**
- Max nodes: **4**
- Idle shutdown: leave default
- Click **Create**

### 05 - Create a Compute Instance (for notebooks)
This is your development machine for writing and submitting pipeline jobs.
Azure ML Studio → Compute → Compute instances → + New
- Name: **ci-ai300**
- VM size: **Standard_DS11_v2** or similar
- Enable SSH: optional
- Click **Create**
- Wait until status = **Running**

### 06 - Upload or Register Your Dataset
Needed for prep step
Pipelines need a data asset. You can upload a CSV or use a built-in dataset.
Azure ML Studio → Data → + Create
- Type: **Tabular**
- Source: Upload a CSV (e.g., diabetes.csv) OR point to Blob storage
- Name: **diabetes-data**
- Schema: Auto-detect
- Confirm preview loads correctly
- Click **Create**

### 07 - Create a Workspace Datastore (optional but recommended)
A datastore gives you a clean path for pipeline inputs/outputs.
Azure ML Studio → Data → Datastores → + New datastore
- Type: **Azure Blob Storage**
- Name: **workspaceblobstore** (default)
- Link to the storage account created with the workspace
- Test connection → Save
    
### 08 - Prepare Your Development Folder
You’ll store component scripts, environment files, and pipeline definitions here.
Compute Instance → JupyterLab → New Folder
- Create folder: **azureml-pipeline-lab**
- Inside it, create subfolders:
    - **components/prep**
    - **components/train**
    - **env**
    - **pipeline**
- You will place: prep.py, train.py, conda-env.yml, pipeline.py

### 09 - Install Required Python Packages
Ensures your notebook environment can submit pipeline jobs.
Compute Instance → Terminal
- Run:
```bash
pip install azure-ai-ml azure-identity pandas scikit-learn joblib
```
- Restart the kernel after installation

### 10 - Verify Workspace Access via SDK
Confirms your identity and workspace connection before running the pipeline.
Notebook cell
- Run:
```python
from azure.ai.ml import MLClient
from azure.identity import DefaultAzureCredential

ml_client = MLClient(
  DefaultAzureCredential(),
  subscription_id="<your-subscription>",
  resource_group_name="rg-ai300-labs",
  workspace_name="mlw-ai300"
)
    
ws = ml_client.workspaces.get("mlw-ai300")
print("Workspace name:", ws.name)
print("Location:", ws.location)
print("Description:", ws.description)
```
- If it prints workspace details, you're ready to build the pipeline

---

## 👍 Summary
You now have:

- A **resource group**
- An **Azure ML workspace**
- A **compute cluster** for pipeline steps
- A **compute instance** for development
- A **registered dataset**
- A **datastore**
- A **project folder structure**
- A **working SDK connection**

This is the complete foundation required to run the **Run a Pipeline Job** lab you’re building.

### 1. Create the project structure
On your compute instance (or local machine), create a folder structure like:

```text
azureml-pipeline-lab/
  components/
    prep/
      prep.py
    train/
      train.py
  env/
    conda-env.yml
  pipeline/
    pipeline.py
```

You’ll fill these files in the next steps.

---

### 2. Define the data preparation component
#### 2.1. Write `prep.py`

This script will:

- Load the input dataset.
- Perform simple cleaning/feature engineering.
- Save a processed dataset to an output folder.

Create `components/prep/prep.py`:

```python
import argparse
import os
import pandas as pd

def main():
    parser = argparse.ArgumentParser()
    parser.add_argument("--input_data", type=str, help="Path to input data")
    parser.add_argument("--output_data", type=str, help="Path to output data")
    args = parser.parse_args()

    os.makedirs(args.output_data, exist_ok=True)

    # Load data
    print(f"Loading data from {args.input_data}")
    df = pd.read_csv(args.input_data)

    # Simple cleaning: drop rows with missing target, fill numeric NaNs
    target_col = "Y"  # change to your actual target column
    df = df.dropna(subset=[target_col])
    numeric_cols = df.select_dtypes(include=["float64", "int64"]).columns
    df[numeric_cols] = df[numeric_cols].fillna(df[numeric_cols].median())

    # Save processed data
    output_path = os.path.join(args.output_data, "prepared.csv")
    print(f"Saving prepared data to {output_path}")
    df.to_csv(output_path, index=False)

if __name__ == "__main__":
    main()
```

Adjust `target_col` to match your dataset.

#### 2.2. Define the prep component in Python

You’ll define the component in `pipeline/pipeline.py` later, but conceptually:

- Inputs: `input_data` (MLTable or CSV).
- Outputs: `output_data` (folder with `prepared.csv`).
- Command: `python prep.py --input_data ... --output_data ...`.

We’ll wire this up after we create the training component.

---

### 3. Define the training component

#### 3.1. Write `train.py`

This script will:

- Load the prepared data.
- Split into train/test.
- Train a simple model (e.g. logistic regression).
- Log metrics.
- Save the model artifact.

Create `components/train/train.py`:

```python
import argparse
import os
import joblib
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score

def main():
    parser = argparse.ArgumentParser()
    parser.add_argument("--training_data", type=str, help="Path to prepared data")
    parser.add_argument("--regularization", type=float, default=1.0, help="C parameter for LogisticRegression")
    parser.add_argument("--model_output", type=str, help="Path to output model")
    args = parser.parse_args()

    os.makedirs(args.model_output, exist_ok=True)

    # Load prepared data
    data_path = os.path.join(args.training_data, "prepared.csv")
    print(f"Loading prepared data from {data_path}")
    df = pd.read_csv(data_path)

    target_col = "Y"  # change to your actual target column
    X = df.drop(columns=[target_col])
    y = df[target_col]

    # Train/test split
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

    # Train model
    print(f"Training LogisticRegression with C={args.regularization}")
    model = LogisticRegression(C=args.regularization, max_iter=1000)
    model.fit(X_train, y_train)

    # Evaluate
    y_pred = model.predict(X_test)
    acc = accuracy_score(y_test, y_pred)
    print(f"Accuracy: {acc}")

    # Save model
    model_path = os.path.join(args.model_output, "model.pkl")
    print(f"Saving model to {model_path}")
    joblib.dump(model, model_path)

if __name__ == "__main__":
    main()
```

Again, adjust `target_col` to match your dataset.

---

### 4. Define the environment

Create `env/conda-env.yml`:

```yaml
name: pipeline-env
channels:
  - conda-forge
  - defaults
dependencies:
  - python=3.10
  - pip
  - pip:
      - azure-ai-ml
      - azure-identity
      - pandas
      - scikit-learn
      - joblib
```

This environment will be used by both components.

---

### 5. Build the pipeline in `pipeline.py`

Now you’ll define:

- The **Azure ML client**.
- Two **command components** (prep and train).
- A **pipeline function** that wires them together.
- A **pipeline job submission**.

Create `pipeline/pipeline.py`:

```python
import os
from azure.identity import DefaultAzureCredential
from azure.ai.ml import MLClient
from azure.ai.ml import command, Input, Output
from azure.ai.ml.dsl import pipeline
from azure.ai.ml.entities import Environment

# --------- CONFIGURE WORKSPACE ---------
subscription_id = "<YOUR_SUBSCRIPTION_ID>"
resource_group = "<YOUR_RESOURCE_GROUP>"
workspace_name = "<YOUR_WORKSPACE_NAME>"

credential = DefaultAzureCredential()
ml_client = MLClient(
    credential=credential,
    subscription_id=subscription_id,
    resource_group_name=resource_group,
    workspace_name=workspace_name,
)

# --------- DEFINE ENVIRONMENT ---------
env = Environment(
    name="pipeline-env",
    description="Environment for pipeline components",
    conda_file="../env/conda-env.yml",
    image="mcr.microsoft.com/azureml/openmpi4.1.0-ubuntu20.04"  # common base image
)

# Register environment (optional but recommended)
env = ml_client.environments.create_or_update(env)

# --------- DEFINE COMPONENTS ---------
# Paths relative to this file
prep_code_path = "../components/prep"
train_code_path = "../components/train"

prep_component = command(
    name="prep-data-component",
    display_name="Prepare data",
    description="Clean and prepare raw data",
    inputs={
        "input_data": Input(type="uri_file")
    },
    outputs={
        "output_data": Output(type="uri_folder")
    },
    code=prep_code_path,
    command="python prep.py --input_data ${{inputs.input_data}} --output_data ${{outputs.output_data}}",
    environment=env,
    compute="cpu-cluster",  # your compute cluster name
)

train_component = command(
    name="train-model-component",
    display_name="Train model",
    description="Train a logistic regression model",
    inputs={
        "training_data": Input(type="uri_folder"),
        "regularization": Input(type="number")
    },
    outputs={
        "model_output": Output(type="uri_folder")
    },
    code=train_code_path,
    command=(
        "python train.py "
        "--training_data ${{inputs.training_data}} "
        "--regularization ${{inputs.regularization}} "
        "--model_output ${{outputs.model_output}}"
    ),
    environment=env,
    compute="cpu-cluster",
)

# --------- DEFINE PIPELINE ---------
@pipeline(
    description="Data prep and training pipeline",
)
def training_pipeline(
    raw_data_input: Input,
    regularization: float = 1.0,
):
    prep_step = prep_component(
        input_data=raw_data_input
    )

    train_step = train_component(
        training_data=prep_step.outputs.output_data,
        regularization=regularization
    )

    return {
        "prepared_data": prep_step.outputs.output_data,
        "trained_model": train_step.outputs.model_output,
    }

# --------- CREATE AND SUBMIT PIPELINE JOB ---------
if __name__ == "__main__":
    # Use a registered dataset as input
    raw_data = Input(
        type="uri_file",
        path="azureml://datastores/workspaceblobstore/paths/<path-to-your-raw-data>.csv"
        # OR: path="azureml:<your-registered-dataset-name>"
    )

    pipeline_job = training_pipeline(
        raw_data_input=raw_data,
        regularization=1.0,
    )

    pipeline_job = ml_client.jobs.create_or_update(
        pipeline_job,
        experiment_name="pipeline-lab-experiment",
    )

    print(f"Submitted pipeline job: {pipeline_job.name}")
```

**Important adjustments:**

- Replace `subscription_id`, `resource_group`, `workspace_name` with your actual values.
- Set `compute="cpu-cluster"` to your compute cluster name.
- Set `raw_data` path to either:
    - A blob path in your workspace data store, or
    - A registered dataset (e.g. `path="azureml:diabetes-data"` if using MLTable).

---

### 6. Run the pipeline job from a notebook
Open a notebook on your compute instance:

1. **Upload the project folder** (or clone from Git).
2. In a cell, run:
```python
%cd azureml-pipeline-lab/pipeline

!python pipeline.py
```

You should see output similar to:

```text
Submitted pipeline job: training_pipeline-xxxxxxx
```

---

### 7. Monitor the pipeline run in Azure ML studio

In the Azure ML studio:

1. **Go to Jobs → Pipeline jobs**
    
    - Find the job with experiment name `pipeline-lab-experiment`.
    - Click the job.
2. **View the graph**
    
    - You’ll see two steps: `prep-data-component` and `train-model-component`.
    - Confirm that `prep` runs first, then `train`.
3. **Inspect logs**
    
    - Click each step → **Outputs + logs**.
    - Check `stdout` for messages like “Accuracy: …” and file paths.
4. **Check outputs**
    
    - For `prep`, confirm `prepared.csv` exists in the output folder.
    - For `train`, confirm `model.pkl` exists in the model output folder.
5. **Lineage**
    
    - From the job or dataset/model views, explore lineage to see how the pipeline produced them.

---

### 8. Rerun the pipeline with different parameters

To make this feel like a real lab, run the pipeline again with a different regularization value.

In the notebook:

```python
from azure.identity import DefaultAzureCredential
from azure.ai.ml import MLClient, Input
from pipeline import training_pipeline  # if you refactor pipeline.py accordingly

subscription_id = "<YOUR_SUBSCRIPTION_ID>"
resource_group = "<YOUR_RESOURCE_GROUP>"
workspace_name = "<YOUR_WORKSPACE_NAME>"

credential = DefaultAzureCredential()
ml_client = MLClient(
    credential=credential,
    subscription_id=subscription_id,
    resource_group_name=resource_group,
    workspace_name=workspace_name,
)

raw_data = Input(
    type="uri_file",
    path="azureml:<your-registered-dataset-name>"
)

pipeline_job = training_pipeline(
    raw_data_input=raw_data,
    regularization=0.5,  # different value
)

pipeline_job = ml_client.jobs.create_or_update(
    pipeline_job,
    experiment_name="pipeline-lab-experiment",
)

print(f"Submitted pipeline job: {pipeline_job.name}")
```

Then:

- Compare the two runs in **Jobs**.
- Look at metrics and logs to see how the change affected accuracy.

---

### 9. What you should be able to say after this lab

By the end, you should be comfortable saying:

- “I can define reusable **command components** for data prep and training.”
- “I can build a **pipeline** that orchestrates those components and submit it as a job.”
- “I know how to **monitor**, **debug**, and **compare** pipeline runs in Azure ML studio.”

If you want, next we can:

- Add a **model registration step** to the pipeline.
- Turn this into a **scheduled pipeline** or wire it into **GitHub Actions** for AI‑300‑style MLOps.
- 