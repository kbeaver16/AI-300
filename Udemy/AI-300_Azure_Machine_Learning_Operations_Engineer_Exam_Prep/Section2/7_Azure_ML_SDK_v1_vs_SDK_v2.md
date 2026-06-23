# 📘 Azure ML SDK v1 vs SDK v2

_From: 7_Azure_ML_SDK_v1_vs_SDK_v2.mp3_

## SUMMARY

Azure Machine Learning has evolved from **SDK v1** to **SDK v2**, bringing major improvements in abstraction, MLflow integration, authentication, pipeline creation, and support for modern Azure ML features like **MLTable** and **Data Assets**. All course labs use **SDK v2**, which is now the recommended and actively supported version.

> “SDK version 2 is a lot more abstracted than version 1… and has better compatibility with MLflow.”

---

## KEY CONCEPTS

### **Azure ML SDK**

A Python SDK used to:
- Connect to Azure ML workspaces
- Run experiments & jobs
- Train, validate, and deploy models
- Register data assets & ML tables
- Interact with MLflow for experiment tracking

---

### **SDK v1**

Older version of the Azure ML SDK.  
Characteristics:
- Package namespace: `azureml.core`
- Manual authentication (API keys, service principals)
- Limited abstraction
- No MLTable support
- Pipelines were harder to build and maintain
- Being deprecated

---

### **SDK v2**

Modern, improved SDK used in all current Azure ML workflows.  
Characteristics:
- Package namespace: `azure.ai.ml`
- High‑level abstraction
- Native MLflow integration
- YAML‑based pipelines
- MLTable + Data Asset support
- Uses **DefaultAzureCredential** for authentication
- Cleaner, more modular API

---

## DEFINITIONS

- **MLClient**  
    The primary object used to connect to and interact with an Azure ML workspace.
- **DefaultAzureCredential**  
    A unified authentication mechanism that automatically uses the user’s Azure identity.
- **MLTable**  
    A structured, versioned dataset format supported only in SDK v2.
- **MLflow**  
    An experiment tracking and model registry framework integrated directly into SDK v2.
    
---

## EXAMPLES

### Example: SDK v2 MLClient Initialization

```python
from azure.ai.ml import MLClient
from azure.identity import DefaultAzureCredential

ml_client = MLClient(
    credential=DefaultAzureCredential(),
    subscription_id="<SUBSCRIPTION_ID>",
    resource_group_name="<RESOURCE_GROUP>",
    workspace_name="<WORKSPACE_NAME>"
)
```

### Example: SDK v1 Import Pattern

```python
from azureml.core import Workspace, Experiment
```

### Example: SDK v2 Pipeline Structure

- Python scripts define components
- YAML defines pipeline structure
- MLClient submits pipeline jobs

---

## STEP‑BY‑STEP PROCESSES

### How SDK v2 Improves the ML Workflow

---

## ACTIONABLE TAKEAWAYS

- Always use **SDK v2** — SDK v1 is deprecated and lacks modern features.
- SDK v2 simplifies authentication, pipelines, and data management.
- MLflow integration in v2 is essential for experiment tracking and model governance.
- MLTable support is exclusive to SDK v2 and critical for modern Azure ML workflows.
- All course labs and future Azure ML development rely on SDK v2.

---

## TAGS

#AzureML #SDKv2 #MLflow #MLClient #MLOps #MLTable #DataAssets #AI300 #AzureMachineLearning

---

## BACKLINK SUGGESTIONS

- [[Azure ML – SDK v2 Usage]]
- [[Azure ML – MLflow Integration]]
- [[Azure ML – Pipelines (YAML + Components)]]
- [[Azure ML – Data Assets & MLTable]]
- [[Azure ML – Workspace Authentication]]
