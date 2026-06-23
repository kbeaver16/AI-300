# DAY 01 — MLOPS LEARNING PATH OVERVIEW

## LEARNING PATH
- https://learn.microsoft.com/en-us/training/paths/build-first-machine-operations-workflow/

## TASKS
- [x] Review full learning path 🛫 2026-06-01 📅 2026-06-01 ✅ 2026-06-01
- [x] Identify all modules 🛫 2026-06-01 📅 2026-06-01 ✅ 2026-06-01
- [x] Map to exam domain: MLOps infrastructure 🛫 2026-06-01 📅 2026-06-01 ✅ 2026-06-01

## KEY CONCEPTS
- Azure ML workspace
- Infrastructure-first design
- DevOps vs MLOps

## NOTES
# 🔧 **DevOps vs. MLOps — The Core Difference**

**DevOps** manages the lifecycle of _software_.  
**MLOps** manages the lifecycle of _machine learning models_.

That’s the whole thing in one sentence.

But the details matter, because ML introduces chaos that normal DevOps pipelines can’t handle.

---

# 🧩 **Why DevOps Isn’t Enough for ML**

Traditional DevOps assumes:

- Code changes are the main thing that evolves
- Builds are deterministic
- Tests are predictable
- Deployments are repeatable

Machine learning breaks all of that.

ML systems change because of:

- **Data drift**
- **Model drift**
- **Feature changes**
- **Retraining cycles**
- **Experimentation**
- **Hyperparameter tuning**
- **Non‑deterministic training**

So MLOps adds the missing pieces.

---

# ⚙️ **Side‑by‑Side Comparison**

## **1. What They Manage**

|Area|DevOps|MLOps|
|---|---|---|
|Primary asset|Code|Code + Data + Models|
|Change drivers|Developers|Data + Models + Pipelines|
|Deployment target|Apps/APIs|Models, endpoints, pipelines|

---

## **2. Pipelines**

|Pipeline Type|DevOps|MLOps|
|---|---|---|
|CI|Build/test code|Build/test code + validate data + validate model|
|CD|Deploy app|Deploy model + register model + deploy endpoint|
|Retraining|Not needed|Required (scheduled or triggered)|

---

## **3. Testing**

|Testing|DevOps|MLOps|
|---|---|---|
|Unit tests|Code|Code + data validation|
|Integration tests|Services|Feature pipelines + model scoring|
|Performance tests|Throughput|Accuracy, precision, recall, drift|

---

## **4. Monitoring**

|Monitoring|DevOps|MLOps|
|---|---|---|
|Runtime metrics|CPU, memory, logs|CPU, memory, logs + **model accuracy**, drift, data quality|
|Alerts|App failures|Model degradation|

---

## **5. Tooling (Azure)**

|DevOps|MLOps|
|---|---|
|GitHub Actions / Azure DevOps|Azure Machine Learning Pipelines|
|App Service / AKS|Managed Online Endpoints|
|App Insights|App Insights + Data Drift Monitors|
|ARM/Bicep/Terraform|ARM/Bicep/Terraform + AML Registries|

---

# 🧠 **The Mental Model**

Think of MLOps as:

> **DevOps + Data Engineering + Model Lifecycle Management**

It’s DevOps, but with:

- data pipelines
- feature stores
- experiment tracking
- model registries
- retraining workflows
- drift detection
- responsible AI checks

DevOps alone can’t handle those.

---

# 🎯 **AI‑300 Exam Tip**

If the question mentions:

- data drift
- model drift
- retraining
- experiment tracking
- model registry
- feature pipelines
- hyperparameter tuning
- reproducibility
- lineage

The correct answer is **MLOps**, not DevOps.

If the question mentions:

- CI/CD for app code
- infrastructure as code
- deployment automation
- release pipelines

The correct answer is **DevOps**.

---
# 🧠 **AI + ML Architecture Notes (Microsoft Learn)**
[AI Architecture Design - Azure Architecture Center | Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/)

## 1. **What AI Is**

AI is technology that enables machines to imitate intelligent human behavior. Machines can:

- Analyze data to create images or videos
- Analyze and synthesize speech
- Interact verbally in natural ways
- Make predictions
- Generate new data  
    [Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/)

AI is used when traditional logic or rule‑based systems cannot handle complexity effectively.  
[Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/)

---

## 2. **Why Architects Need AI/ML Knowledge**

Solution architects must understand:

- The AI/ML landscape
- How Azure AI services integrate into workloads
- How to apply Azure Well‑Architected Framework principles to AI workloads  
    [Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/)

The Azure Architecture Center provides:

- Example architectures
- Architectural baselines
- Design guides  
    [Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/)

---

## 3. **Core AI Concepts**

### **Algorithms**

Algorithms are step‑by‑step instructions machines follow to achieve a goal.  
They help explore, analyze, and find meaning in complex datasets.  
Examples include:

- Classifying animals
- Identifying languages
- Translating text  
    [Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/)

### **Machine Learning**

Machine learning uses algorithms to create predictive models.  
Key steps:

1. Parse data
2. Learn patterns
3. Generate a model
4. Validate against known data
5. Measure performance
6. Adjust and retrain  
    [Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/)

This process is called **training**.

---

## 4. **Azure AI Platform Overview**

Azure provides a broad ecosystem for AI/ML workloads, supporting everything from simple prototypes to enterprise‑scale architectures.  
[deepwiki.com](https://deepwiki.com/MicrosoftDocs/architecture-center/4-ai-and-machine-learning-architecture)

### **Key Platform Components**

- **Azure AI Foundry** – AI development environment
- **Azure Machine Learning** – ML workspace, pipelines, model registry
- **Azure OpenAI Service** – LLMs, chat, embeddings
- **Azure AI Search** – vector + hybrid search
- **Azure AI Services** – vision, speech, language APIs
- **Azure Storage / ADLS** – data storage
- **Azure Cosmos DB** – NoSQL
- **Azure Synapse Analytics** – analytics + data engineering
- **Azure App Service / AKS / Functions** – hosting and orchestration  
    [deepwiki.com](https://deepwiki.com/MicrosoftDocs/architecture-center/4-ai-and-machine-learning-architecture)

---

## 5. **Architectural Patterns for AI/ML**

Azure Architecture Center includes patterns for:

- Basic AI integrations
- MLOps workflows
- Data science pipelines
- Azure OpenAI chat applications
- Enterprise landing zones for AI  
    [deepwiki.com](https://deepwiki.com/MicrosoftDocs/architecture-center/4-ai-and-machine-learning-architecture)

These patterns emphasize:

- Security
- Networking
- Monitoring
- Governance
- Scalability

---

## 6. **Well‑Architected Framework for AI Workloads**

AI workloads should follow the five pillars:

- Reliability
- Security
- Cost optimization
- Operational excellence
- Performance efficiency  
    [Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/)

The guidance influences:

- Data pipelines
- Model training
- Deployment
- Monitoring
- Responsible AI considerations

---

# 📘 **Vocabulary Section**

### **AI (Artificial Intelligence)**

Technology enabling machines to mimic human intelligence.  
[Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/)

### **Algorithm**

A finite set of step‑by‑step instructions used to solve a problem or perform a task.  
[Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/)

### **Machine Learning (ML)**

A subset of AI where algorithms learn patterns from data to make predictions.  
[Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/)

### **Model Training**

The process of feeding data into an algorithm so it can learn patterns and produce a predictive model.  
[Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/)

### **Predictive Model**

A model that uses learned patterns to make predictions on new data.  
[Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/)

### **Azure Machine Learning (AML)**

Azure’s platform for building, training, deploying, and managing ML models.  
[deepwiki.com](https://deepwiki.com/MicrosoftDocs/architecture-center/4-ai-and-machine-learning-architecture)

### **Azure AI Foundry**

A unified environment for building AI applications.  
[deepwiki.com](https://deepwiki.com/MicrosoftDocs/architecture-center/4-ai-and-machine-learning-architecture)

### **Azure OpenAI Service**

Provides access to OpenAI models (GPT, embeddings, etc.) via Azure.  
[deepwiki.com](https://deepwiki.com/MicrosoftDocs/architecture-center/4-ai-and-machine-learning-architecture)

### **Azure AI Search**

Search engine with vector + hybrid search for LLM apps.  
[deepwiki.com](https://deepwiki.com/MicrosoftDocs/architecture-center/4-ai-and-machine-learning-architecture)

### **MLOps**

Operational practices for managing ML lifecycle: training, deployment, monitoring, governance.  
[deepwiki.com](https://deepwiki.com/MicrosoftDocs/architecture-center/4-ai-and-machine-learning-architecture)

### **Landing Zone**

A secure, governed Azure environment for deploying workloads, including AI.  
[deepwiki.com](https://deepwiki.com/MicrosoftDocs/architecture-center/4-ai-and-machine-learning-architecture)

---

## GOAL
Understand how infrastructure supports ML lifecycle
