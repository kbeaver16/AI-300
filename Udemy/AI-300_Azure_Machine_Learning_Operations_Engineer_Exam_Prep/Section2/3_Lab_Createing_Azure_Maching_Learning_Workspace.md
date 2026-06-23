# 📘 Lab: Creating an Azure Machine Learning Workspace

_From: 3_Lab_Creating_Azure_Machine_Learning_Workspace.txt_

## SUMMARY

This lab walks through deploying an **Azure Machine Learning Workspace** in the Azure Portal, including all automatically provisioned underlying resources (Key Vault, Storage Account, Application Insights, Container Registry). It then explores the Azure ML Studio interface, its major components, and how the workspace connects to underlying resources.

> “With the workspace provisioning we’ll also see that all other different resources… will also be created within that same managed resource group.”

---

## KEY CONCEPTS

### **Azure Machine Learning Workspace**

A top‑level Azure resource that provides:

- Collaboration space for data scientists & ML engineers
- Notebook execution
- Model training & evaluation
- Model registry
- Deployment endpoints
- Compute management

---

### **Managed Resource Group Components**

When you create an Azure ML workspace, Azure automatically provisions:
- **Azure Storage Account**  
    Stores:
    - Experiment run state
    - Data assets
    - Pipeline state
    - Notebook outputs
- **Azure Key Vault**  
    Stores secrets for:
    - Storage
    - Application Insights
    - Container Registry
    - Azure AI Services
- **Application Insights**  
    Monitors:
    - Endpoint latency
    - Request counts
    - Status codes
- **Azure Container Registry (ACR)**  
    Stores:
    - Custom environments
    - Curated environments
    - Docker images for training & inference
- **Log Analytics Workspace**  
    Used for diagnostics and KQL queries.

---

### **Azure Machine Learning Studio**

The browser‑based UI for interacting with the workspace.

Key areas include:
- **Model Catalog**  
    ~12,000 vendor‑agnostic models (OpenAI, Gemini, Claude, Microsoft).  
    Deployable via:
    - Serverless API inference
    - Managed compute
- **Notebooks**  
    Jupyter‑style environment for:
    - Training
    - Experimentation
    - GitHub integration
- **Automated ML**  
    Auto‑selects:
    - Best algorithm
    - Best hyperparameters
    - Produces a ready‑to‑register model
- **Designer (Low‑Code)**  
    Drag‑and‑drop ML pipelines for:
    - Data prep
    - Training
    - Evaluation
    - Deployment
- **Prompt Flow**  
    Build GenAI microservices and LLM workflows.
- **Assets**
    - **Data**: datastores + data assets
    - **Jobs**: experiment runs
    - **Components**: reusable pipeline steps
    - **Pipelines**: multi‑step ML workflows
    - **Environments**: curated + custom
    - **Models**: model registry
- **Endpoints**
    - Real‑time endpoints
    - Batch endpoints
    - Serverless endpoints
    - Azure OpenAI endpoints
- **Compute**
    - Compute instances
    - Compute clusters
    - Kubernetes clusters
    - Serverless compute
- **Monitoring**  
    Track deployed model performance.
- **Connections Center**  
    Shows all underlying linked resources:
    - Default blob store
    - Key Vault
    - Application Insights
    - ACR
    - Optional: Azure OpenAI, Foundry, Azure Search, Databricks, OneLake, SharePoint

---

## DEFINITIONS

- **Datastore**  
    A reference to a storage location (Blob, ADLS, OneLake).
- **Data Asset**  
    A versioned dataset stored in a datastore.
- **Experiment Run**  
    A single execution of a training script or notebook cell.
- **Model Registry**  
    A versioned repository of trained models + metadata.
- **Managed Compute**  
    Azure‑provisioned compute for training or inference.
- **Serverless Inference**  
    Pay‑per‑use model deployment with no infrastructure management.

---

## STEP‑BY‑STEP PROCESS

### Creating an Azure Machine Learning Workspace (Portal)

---

## EXAMPLES

### Example: Workspace Resource Group Contents

A typical resource group after workspace creation includes:

- `mystorageaccount123`
- `myworkspace-kv`
- `myworkspace-insights`
- `myworkspace-acr`
- `myworkspace` (Azure ML Workspace)

### Example: Using the Model Catalog

- Deploy **Claude Opus** on serverless inference
- Deploy **Gemini 2** on managed compute for fine‑tuning
- Compare cost vs control trade‑offs

---

## ACTIONABLE TAKEAWAYS

- Azure ML workspaces automatically provision all required infrastructure — no manual setup needed.
- The **Connections Center** is your source of truth for linked resources.
- **workspaceblobstore** is the default datastore for experiments and data assets.
- Use **serverless inference** for cost‑efficient LLM usage; use **managed compute** for full control.
- The Studio UI is your command center for all ML workflows: data → training → registry → deployment.

---

## TAGS

#AzureML #MLOps #MachineLearning #AzureAI #ModelRegistry #PromptFlow #Designer #LLM #AI300 #AzurePortal

---

## BACKLINK SUGGESTIONS

- [[Azure ML – Workspace Architecture]]
- [[Azure ML – Data Assets & Datastores]]
- [[Azure ML – Compute Management]]
- [[Azure ML – Model Catalog]]
- [[Azure ML – Model Registry (MLflow)]]
- [[Azure ML – Endpoints (Real‑Time & Batch)]]
- [[Azure ML – Prompt Flow]]
- [[Azure ML – Designer Pipelines]]