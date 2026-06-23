# 📘 Azure Machine Learning Workspace — Introduction

_From: 2_Introduction_to_Azure_Machine_Learning_Capabilities_and_Integration.mp3_

## SUMMARY

Azure Machine Learning (Azure ML) is a **unified platform** for building, training, evaluating, versioning, and deploying both **classical ML models** and **generative AI models**. It integrates compute, storage, monitoring, and model governance into a single workspace, supported by several underlying Azure resources.

> “I want you to imagine Azure Machine Learning as a unified workspace…”  
> “You have thousands of models to choose from… vendor‑agnostic models…”

---

## KEY CONCEPTS & DEFINITIONS

### **Azure Machine Learning Workspace**

A centralized environment for:
- Classical ML (regression, classification, forecasting)
- Generative AI (LLMs from OpenAI, Gemini, Claude, Microsoft)
- Model training, evaluation, deployment, and versioning
- Managing compute, data, pipelines, and monitoring

---

### **Model Catalog**

A built‑in catalog containing:
- Microsoft models
- OpenAI models
- Gemini
- Claude
- Other vendor‑agnostic LLMs

Supports:
- Serverless API inference
- Managed compute deployments

---

### **Model Registry (via MLflow)**

A versioned repository for:
- Model artifacts
- Metrics (accuracy, precision, recall, etc.)
- Experiment runs
- Comparison across versions

> “You could… compute all the metrics… and log those metrics along with all the model files in a model registry…”

---

### **Underlying Azure Resources**

Azure ML automatically provisions:
- **Azure Key Vault**  
    Secure storage for secrets (API keys, credentials).
- **Azure Storage Account**  
    Stores:
    - Data assets
    - Pipeline state
    - Model artifacts
- **Log Analytics Workspace**  
    For logs, debugging, and KQL queries.
- **Application Insights**  
    For endpoint monitoring (latency, request counts, status codes).
- **Anomaly Detector Alert Rule**  
    For automated alerts (e.g., retraining triggers).

---

### **Azure Machine Learning Studio**

A browser‑based UI for:
- Notebook development
- Data asset management
- Compute management
- Model training & deployment
- Monitoring

> “It takes you to this really beautiful user interface… Azure Machine Learning Studio.”

---

### **Data Management**
- Register Azure Blob Storage or ADLS as a datastore
- Create **data assets** for training/testing
- Supports **data versioning**

---

### **Compute Options**

- **Compute Instances** (interactive dev)
- **Compute Clusters** (distributed training)
- **Kubernetes clusters** (CPU/GPU inference)
- **Serverless compute** (pay‑per‑use inference)

---

### **Deployment Options**

1. **Real‑Time Endpoints**
    - Low latency
    - Interactive applications
2. **Batch Endpoints**
    - High throughput
    - Parallel inference
    - Higher latency

---

### **Azure ML Designer (Low‑Code)**

Drag‑and‑drop interface for:
- Data prep
- Model training
- Evaluation
- Model registration
- Deployment

Ideal for POCs or non‑Python workflows.

---

### **Integration with Microsoft Foundry**

- Build GenAI applications
- Create AI agents
- Enterprise‑grade orchestration

---

## EXAMPLES

### **Example: Classical ML Workflow**

- Register training/testing data assets
- Create compute cluster
- Train scikit‑learn model in notebook
- Log metrics + artifacts with MLflow
- Register model
- Deploy to real‑time endpoint
- Monitor with Application Insights

---

### **Example: GenAI Workflow**

- Select LLM from model catalog
- Deploy to serverless endpoint
- Test inference
- Integrate via REST API

---

## STEP‑BY‑STEP PROCESSES

### **Process: Training & Deploying a Classical ML Model in Azure ML**

---

## ACTIONABLE TAKEAWAYS

- Azure ML unifies classical ML and GenAI workflows in one workspace.
- MLflow is essential for experiment tracking and model governance.
- Data assets and versioning ensure reproducibility.
- Real‑time vs batch endpoints require different performance trade‑offs.
- Designer enables low‑code experimentation without Python.
- Integration with Foundry expands GenAI application capabilities.

---

## TAGS

#AzureML #MachineLearning #GenAI #MLflow #ModelRegistry #AzureAI #DataEngineering #MLOps #LLM #Foundry

---

## BACKLINK SUGGESTIONS

- [[Azure ML – Data Assets]]
- [[Azure ML – Compute Options]]
- [[Azure ML – Model Registry (MLflow)]]
- [[Azure ML – Deployment Patterns]]
- [[Azure ML – Designer (Low‑Code)]]
- [[Generative AI in Azure ML]]
- [[Microsoft Foundry Overview]]