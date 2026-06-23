# DAY 10 — MODULE 7
Deploy a model with GitHub Actions

## MODULE LINK
- https://learn.microsoft.com/en-us/training/modules/deploy-model-github-actions/

## TASKS
- [x] Complete module ✅ 2026-06-08
- [x] Learn deployment workflows ✅ 2026-06-08
- [x] Study blue/green deployment ✅ 2026-06-08

## KEY CONCEPTS
- Endpoint deployment
- CI/CD automation

```json
//The output includes credentials that you must protect. Be sure that you do not include these credentials in your code or check the credentials into your source control. For more information, see https://aka.ms/azadsp-cli
{
  "clientId": "6e9c83fc-f60f-4add-8ee3-4d06787ff4ad",
  "clientSecret": "3me8Q~-kVAhD4cdCuEEWj6AmI5-vlouqBTDUWc6W",
  "subscriptionId": "4664de08-cbd6-47ef-80f4-77d82fb58f33",
  "tenantId": "22381759-e502-459b-b6fc-d571c80c370f",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```

```bash
keil@Azure:~/mslearn-mlops/infra$ az ad sp create-for-rbac
The output includes credentials that you must protect. Be sure that you do not include these credentials in your code or check the credentials into your source control. For more information, see https://aka.ms/azadsp-cli
{
  "appId": "fe0348e5-44a1-4c56-ae42-e0c56a1e87f8",
  "displayName": "azure-cli-2026-06-08-21-53-12",
  "password": ".RS8Q~5NqzbF5Fot-CsHIbgqqYXUIbRj_bzCXbdz",
  "tenant": "22381759-e502-459b-b6fc-d571c80c370f"
}
```
## EXERCISE NOTES

## NOTES
# 📘 Module Notes

## [Deploy a model with GitHub Actions – Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/deploy-model-github-actions/)

---

# 🧠 1. Module Overview

### Purpose

This module teaches how to:

- Deploy a machine learning model
- Automate that deployment using GitHub Actions
- Test the deployed model

All of this is done using:

- **Azure Machine Learning (Azure ML)**
- **Azure ML CLI v2**
- **GitHub Actions for CI/CD** [[Deploy a m...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/training/modules/deploy-model-github-actions/), [[learn.microsoft.com]](https://learn.microsoft.com/en-us/training/modules/deploy-model-github-actions/)

---

### Key Learning Objectives

- Deploy models to **managed endpoints**
- Trigger deployments via **GitHub workflows**
- Validate the deployed model through testing [[Deploy a m...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/training/modules/deploy-model-github-actions/)

---

### Module Structure (7 Units)

1. Introduction
2. Business Problem
3. Solution Architecture
4. Model Deployment
5. Exercise
6. Assessment
7. Summary [[learn.microsoft.com]](https://learn.microsoft.com/en-us/training/modules/deploy-model-github-actions/)

---

# 🏗️ 2. Core Concepts

## ✅ MLOps (Machine Learning Operations)

- A practice that **automates the ML lifecycle**
- Combines:
    - DevOps (CI/CD)
    - Data science workflows
- Supports:
    - Training
    - Deployment
    - Monitoring [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-setup-mlops-github-azure-ml?view=azureml-api-2)

---

## ✅ GitHub Actions (CI/CD)

- GitHub-native automation tool
- Uses **YAML workflows**
- Automates:
    - Build
    - Test
    - Deploy pipelines [[docs.github.com]](https://docs.github.com/en/actions/get-started/quickstart)

---

## ✅ Azure Machine Learning

- Cloud platform for ML lifecycle
- Handles:
    - Model training
    - Deployment
    - Monitoring
- Integrates directly with GitHub Actions [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-setup-mlops-github-azure-ml?view=azureml-api-2)

---

## ✅ Managed Online Endpoint

- A deployed API endpoint for real-time inference
- Azure manages:
    - Infrastructure
    - Scaling
    - Security
- Exposes model as a callable service (REST endpoint) [[docs.azure.cn]](https://docs.azure.cn/en-us/machine-learning/how-to-deploy-online-endpoints)

---

# 🧩 3. Architecture (Solution Flow)

High-level architecture implied by module:

```
GitHub Repo → GitHub Actions Workflow → Azure ML CLI → Endpoint Deployment → Testing
```

### Flow Breakdown

1. Code pushed to GitHub
2. GitHub Actions workflow triggers
3. Workflow uses Azure ML CLI
4. Model deployed to endpoint
5. Endpoint tested for predictions

This reflects a **CI/CD pipeline for ML (MLOps)**

---

# ⚙️ 4. Model Deployment Workflow

## Step-by-Step (from sources + module intent)

### Step 1: Register Model

- Store trained model in Azure ML
- Enables versioning and reuse

---

### Step 2: Define Endpoint

- Create endpoint resource
- Represents the public API

---

### Step 3: Create Deployment

- Attach model to endpoint
- Define:
    - Compute size
    - Scaling settings

---

### Step 4: Automate with GitHub Actions

- Create workflow file:

```
.github/workflows/deploy.yml
```

- Workflow steps:
    - Checkout code
    - Authenticate Azure
    - Run Azure CLI commands
    - Deploy model

---

### Step 5: Testing

- Call endpoint using:
    - REST API
    - CLI
    - Test scripts
- Validate predictions

---

## Example Deployment Command (from sources)

```bash
az ml online-endpoint create --name my-endpoint --file endpoint.yml
az ml online-deployment create --name blue \
  --endpoint-name my-endpoint \
  --file deployment.yml \
  --all-traffic
```

[[bing.com]](https://bing.com/search?q=azure+machine+learning+github+actions+deploy+model+steps+endpoint+managed+endpoint+learn+module+details)

---

# 🔁 5. Automation with GitHub Actions

## Why GitHub Actions?

- Triggers on:
    - Code push
    - Pull request
- Enables:
    - Repeatable deployments
    - Version-controlled ML pipelines

---

## Example Workflow

```yaml
name: Deploy Model

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Azure Login
        uses: azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}

      - name: Deploy model
        run: |
          az ml online-endpoint create --file endpoint.yml
```

---

## Security Practice

- Use:
    - GitHub Secrets
    - OpenID Connect (OIDC)
- Avoid storing credentials directly in repo [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-setup-mlops-github-azure-ml?view=azureml-api-2)

---

# 🧪 6. Testing the Deployed Model

Testing ensures:

- Model returns predictions correctly
- Endpoint is operational

### Testing methods:

- REST API calls
- CLI invocation
- Automated test scripts

Example:

```bash
curl -X POST https://<endpoint-url>/score \
     -H "Authorization: Bearer <token>" \
     -d '{"data": [1,2,3]}'
```

---

# 🧱 7. Example End-to-End Scenario

### Use Case: Predict Taxi Fare

(from MLOps reference article)

Pipeline:

1. Data → training script
2. Model trained
3. Saved in Azure ML
4. GitHub Action triggers deployment
5. Endpoint exposed
6. App sends prediction requests [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-setup-mlops-github-azure-ml?view=azureml-api-2)

---

# 🧾 8. Vocabulary

|Term|Definition|
|---|---|
|MLOps|Practice of automating ML lifecycle (training → deployment → monitoring)|
|CI/CD|Continuous Integration / Continuous Deployment automation pipeline|
|GitHub Actions|Workflow automation tool using YAML pipelines|
|Azure ML CLI v2|Command-line tool to manage ML resources|
|Managed Endpoint|Fully managed API endpoint for model inference|
|Deployment|Process of making a model available for use|
|Model Registry|Central location to store and version models|
|Scoring Script|Code that defines how model processes input|
|Inference|Generating predictions from a trained model|
|Workflow (YAML)|File defining CI/CD steps in GitHub Actions|
|Artifact|Saved output (e.g., trained model file)|
|Service Principal / OIDC|Authentication methods for Azure access|

---

# 🧪 9. Concrete Examples

## Example 1: Real Deployment Flow

- Developer pushes code → GitHub
- GitHub Action runs
- Model deployed automatically
- Endpoint becomes available

---

## Example 2: Blue-Green Deployment

- Deploy new version (“blue”)
- Route traffic gradually
- Roll back if needed

(This is referenced as best practice for ML deployment pipelines)

---

## Example 3: Automated Testing

- After deployment:
    - Run test script
    - Validate predictions
    - Fail pipeline if incorrect

---

# ✅ 10. Key Takeaways

- GitHub Actions enables **fully automated ML deployment**
- Azure ML simplifies deployment using **managed endpoints**
- Combined approach = **MLOps pipeline**
- Benefits:
    - Repeatability
    - Scalability
    - Reduced manual work

---

# ✅ If you want next

I can convert this into your **Obsidian / OneNote daily study template** (checklist + code snippets + exam tips for AI-300).
``