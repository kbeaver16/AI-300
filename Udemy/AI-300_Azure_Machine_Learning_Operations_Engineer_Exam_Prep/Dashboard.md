---
tags:
  - AI-300
  - MLOps
  - Azure
  - certification
  - dashboard
aliases:
  - AI-300 Dashboard
  - MLOps Exam Prep
created: 2026-06-13
modified: 2026-06-13
exam-date: ""
target-score: 700
pass-score: 700
exam-cost: "$165 USD"
exam-duration: "100 minutes"
exam-level: Associate
replaces: DP-100
status: in-progress
cssclasses:
  - wide-page
---
# 🎯 AI-300 — Azure ML Operations Engineer · Exam Prep Dashboard

> [!info]+ 📋 Exam Quick Facts
> | Field | Detail |
> |---|---|
> | **Exam Code** | AI-300 |
> | **Full Title** | Machine Learning Operations (MLOps) Engineer Associate |
> | **Level** | Associate (Intermediate) |
> | **Pass Score** | 700 / 1000 |
> | **Duration** | ~100 minutes · 40–60 questions |
> | **Cost** | $165 USD (Beta: ~80% discount) |
> | **Products** | Azure Machine Learning · Microsoft Foundry |
> | **Tools** | Python · GitHub Actions · Bicep · Azure CLI · MLflow |
> | **Replaces** | DP-100 |
> | **Scheduled Date** | `INPUT[date:exam-date]` |

---

## 📊 Overall Progress Tracker

> [!tip] How This Dashboard Works
> Check off tasks inline here **or** in any note tagged `#AI-300` inside `AI-300/` — Dataview aggregates everything automatically. Use `#lab`, `#domain/1` through `#domain/5` tags to keep queries clean.

> [!abstract]+ 🔢 Global Task Summary
> ```dataview
> TABLE WITHOUT ID
>   length(filter(file.tasks, (t) => t.completed)) AS "✅ Done",
>   length(filter(file.tasks, (t) => !t.completed)) AS "⏳ Remaining",
>   round(
>     length(filter(file.tasks, (t) => t.completed)) /
>     length(file.tasks) * 100, 1
>   ) + "%" AS "Overall %"
> FROM "AI-300"
> WHERE file.name = this.file.name
> ```

```dataview
TABLE WITHOUT ID
  rows.file.link AS "File",
  length(filter(rows.file.tasks, (t) => t.completed)) AS "✅ Done",
  length(filter(rows.file.tasks, (t) => !t.completed)) AS "⏳ Remaining"
FROM "AI-300"
GROUP BY file.folder
SORT rows.file.name ASC
````

---

## 🗓️ Today's Focus

```tasks
not done
tags include #AI-300
due today
sort by priority
```

```tasks
not done
tags include #AI-300
due before today
sort by priority
```

> [!check]+ ✅ Completed Today
> 
> ```tasks
> done on today
> tags include #AI-300
> ```

---

## 🏗️ DOMAIN 1 — Design & Implement an MLOps Infrastructure

> [!note] Domain Weight: ~20% of exam · 5 core modules Key tools: **Bicep**, **Azure CLI**, **GitHub Actions**, **Azure ML Workspace**, **Compute Clusters**

### 📚 Module 1.1 — ML Workspace: Your AI Control Room

- [ ] Understand Azure ML Workspace architecture and resource dependencies #AI-300 #domain/1
- [ ] Provision a Workspace via Azure Portal (baseline orientation) #AI-300 #domain/1
- [ ] Identify linked resources: Storage Account, Key Vault, Application Insights, Container Registry #AI-300 #domain/1
- [ ] Configure workspace-level RBAC roles (Owner, Contributor, Reader, AML roles) #AI-300 #domain/1
- [ ] Explore Studio UI: Experiments, Models, Endpoints, Compute, Pipelines, Datastores #AI-300 #domain/1
- [ ] Understand Private Link and network isolation options for Workspace #AI-300 #domain/1
- [ ] Study managed vs. customer-managed keys for encryption at rest #AI-300 #domain/1

---

### 📚 Module 1.2 — Compute Targets: Choosing the Right Engine

- [ ] Differentiate Compute Instance vs. Compute Cluster vs. Serverless vs. Attached Compute #AI-300 #domain/1
- [ ] Configure Compute Cluster: min/max nodes, idle timeout, VM SKU selection #AI-300 #domain/1
- [ ] Understand when to use GPU vs. CPU clusters for training #AI-300 #domain/1
- [ ] Configure Serverless Spark for large-scale data processing #AI-300 #domain/1
- [ ] Attach external compute: Databricks, HDInsight, Kubernetes (AKS/Arc) #AI-300 #domain/1
- [ ] Understand Compute Instance as dev environment: terminals, notebooks, VS Code integration #AI-300 #domain/1
- [ ] Know cost optimization strategies: low-priority VMs, auto-scale, idle shutdown #AI-300 #domain/1

---

### 📚 Module 1.3 — Data, Environments & Components

- [ ] Register and manage Datastores (Azure Blob, ADLS Gen2, Azure SQL, Snowflake) #AI-300 #domain/1
- [ ] Create and version Data Assets (URIFile, URIFolder, MLTable) #AI-300 #domain/1
- [ ] Understand curated vs. custom Environments in AML #AI-300 #domain/1
- [ ] Build a custom Environment from Docker image or Conda specification #AI-300 #domain/1
- [ ] Version and reuse Environments across training runs #AI-300 #domain/1
- [ ] Define reusable pipeline Components using YAML and Python SDK v2 #AI-300 #domain/1
- [ ] Register Components to the Workspace component registry #AI-300 #domain/1
- [ ] Understand asset versioning strategy: semantic vs. auto-increment #AI-300 #domain/1

---

### 📚 Module 1.4 — Infrastructure as Code: Provisioning at Scale

- [ ] Write Bicep templates to provision AML Workspace and dependencies #AI-300 #domain/1
- [ ] Parameterize Bicep templates for dev/test/prod environment parity #AI-300 #domain/1
- [ ] Deploy Bicep templates via GitHub Actions CI/CD workflow #AI-300 #domain/1
- [ ] Use Azure CLI `az ml` extension for imperative resource management #AI-300 #domain/1
- [ ] Use Azure ML Python SDK v2 (`azure-ai-ml`) for programmatic provisioning #AI-300 #domain/1
- [ ] Manage YAML-defined resources: workspaces, compute, datastores, environments #AI-300 #domain/1
- [ ] Understand `az ml job create` and workspace configuration files #AI-300 #domain/1
- [ ] Implement resource tagging strategy for cost management and governance #AI-300 #domain/1

---

### 📚 Module 1.5 — Git & CI/CD for ML Projects

- [ ] Integrate AML Workspace with a GitHub repository #AI-300 #domain/1
- [ ] Design branch strategy for ML projects (feature → dev → main) #AI-300 #domain/1
- [ ] Write a GitHub Actions workflow that triggers on PR to run model training #AI-300 #domain/1
- [ ] Store AML credentials as GitHub Secrets; use federated identity (OIDC) #AI-300 #domain/1
- [ ] Implement code quality gates: linting, unit tests, pre-commit hooks #AI-300 #domain/1
- [ ] Understand Azure DevOps Pipelines as an alternative to GitHub Actions #AI-300 #domain/1
- [ ] Implement protected branches and required PR reviews for model promotion #AI-300 #domain/1

---

## 🔄 DOMAIN 2 — Implement ML Model Lifecycle & Operations

> [!note] Domain Weight: ~35% of exam · 8 core modules Key tools: **MLflow**, **AML Pipelines**, **AutoML**, **Responsible AI Dashboard**, **Online/Batch Endpoints**

### 📚 Module 2.1 — MLflow: Track Every Experiment

- [ ] Understand MLflow components: Tracking, Projects, Models, Registry #AI-300 #domain/2
- [ ] Configure MLflow autologging in an AML training script #AI-300 #domain/2
- [ ] Log custom metrics, parameters, artifacts, and tags with `mlflow.log_*` #AI-300 #domain/2
- [ ] Compare runs in AML Studio's Experiment UI #AI-300 #domain/2
- [ ] Register a model to MLflow Model Registry from a completed run #AI-300 #domain/2
- [ ] Use `mlflow.pyfunc` to wrap a custom model for standardized serving #AI-300 #domain/2
- [ ] Query runs programmatically using `MlflowClient` API #AI-300 #domain/2
- [ ] Understand MLflow model flavors: sklearn, pytorch, tensorflow, pyfunc #AI-300 #domain/2

---

### 📚 Module 2.2 — Training Pipelines: Automate Everything

- [ ] Design a multi-step AML Pipeline using Python SDK v2 #AI-300 #domain/2
- [ ] Define pipeline steps as reusable Components with inputs/outputs #AI-300 #domain/2
- [ ] Chain component data dependencies (OutputPath → InputPath) #AI-300 #domain/2
- [ ] Schedule pipelines with `JobSchedule` (cron or recurrence triggers) #AI-300 #domain/2
- [ ] Pass pipeline parameters at runtime for flexible re-runs #AI-300 #domain/2
- [ ] Implement conditional step execution and parallel job patterns #AI-300 #domain/2
- [ ] Use the Pipeline Run UI to debug step failures and view outputs #AI-300 #domain/2
- [ ] Understand `command`, `pipeline`, `sweep`, and `automl` job types in AML #AI-300 #domain/2

---

### 📚 Module 2.3 — AutoML & Hyperparameter Tuning

- [ ] Configure an AutoML job for classification, regression, and forecasting #AI-300 #domain/2
- [ ] Set primary metric, training timeout, concurrency, and featurization options #AI-300 #domain/2
- [ ] Interpret AutoML results: leaderboard, model explanations, confusion matrix #AI-300 #domain/2
- [ ] Configure a Sweep Job for hyperparameter tuning #AI-300 #domain/2
- [ ] Define search space using `choice`, `uniform`, `loguniform`, `randint` #AI-300 #domain/2
- [ ] Select sampling strategy: Random, Grid, Bayesian #AI-300 #domain/2
- [ ] Configure early termination policies: Bandit, Median Stopping, Truncation Selection #AI-300 #domain/2
- [ ] Retrieve best child run and register the winning model #AI-300 #domain/2
- [ ] Understand NAS (Neural Architecture Search) with AutoML for images #AI-300 #domain/2

---

### 📚 Module 2.4 — Model Registration & Versioning

- [ ] Register a model from a training run, local path, or existing URI #AI-300 #domain/2
- [ ] Add model properties, tags, and stage labels (Development/Staging/Production) #AI-300 #domain/2
- [ ] Implement a model versioning convention aligned with your pipeline strategy #AI-300 #domain/2
- [ ] Archive deprecated model versions; understand immutability of registered models #AI-300 #domain/2
- [ ] Use `Model` asset type in SDK v2 vs. legacy MLflow Registry patterns #AI-300 #domain/2
- [ ] Query and compare registered model metadata programmatically #AI-300 #domain/2

---

### 📚 Module 2.5 — Model Approval & Responsible AI Gates

- [ ] Implement a model approval workflow using GitHub Actions + environment protection rules #AI-300 #domain/2
- [ ] Configure required reviewers for promotion from staging to production #AI-300 #domain/2
- [ ] Integrate Responsible AI Dashboard into the approval gate process #AI-300 #domain/2
- [ ] Generate Responsible AI Dashboard components: Error Analysis, Fairness, Explainability #AI-300 #domain/2
- [ ] Understand fairness metrics: demographic parity, equalized odds, individual fairness #AI-300 #domain/2
- [ ] Use `raiwidgets` and `responsibleai` Python packages in AML pipelines #AI-300 #domain/2
- [ ] Document model cards and lineage for governance and auditability #AI-300 #domain/2
- [ ] Implement Azure AI Content Safety integration as a pre-deployment gate #AI-300 #domain/2

---

### 📚 Module 2.6 — Deploying Models: Endpoints in Production

- [ ] Understand **Managed Online Endpoint** vs. **Kubernetes Online Endpoint** #AI-300 #domain/2
- [ ] Create an Online Endpoint and configure traffic-split deployments (blue/green) #AI-300 #domain/2
- [ ] Configure autoscaling rules: CPU threshold, request queue depth, schedule-based #AI-300 #domain/2
- [ ] Deploy a **Batch Endpoint** for asynchronous, large-batch inference #AI-300 #domain/2
- [ ] Create a scoring script (`score.py`) with `init()` and `run()` functions #AI-300 #domain/2
- [ ] Configure liveness and readiness probes for Online Endpoints #AI-300 #domain/2
- [ ] Implement canary deployments: route 10% traffic to new deployment, validate, then promote #AI-300 #domain/2
- [ ] Understand MLflow model deployment without a custom scoring script #AI-300 #domain/2
- [ ] Secure endpoints with key-based auth vs. Azure AD token auth #AI-300 #domain/2
- [ ] Monitor endpoint latency, throughput, and error rate from Application Insights #AI-300 #domain/2

---

### 📚 Module 2.7 — Distributed Training: Scale to Big Data

- [ ] Understand data parallelism vs. model parallelism for distributed training #AI-300 #domain/2
- [ ] Configure distributed PyTorch training job with `torch.distributed` and `torchrun` #AI-300 #domain/2
- [ ] Use `distribution: mpi` or `distribution: pytorch` in AML command job YAML #AI-300 #domain/2
- [ ] Configure multi-node, multi-GPU training with `process_count_per_instance` #AI-300 #domain/2
- [ ] Use Horovod for TensorFlow distributed training in AML #AI-300 #domain/2
- [ ] Understand DeepSpeed integration for large model training #AI-300 #domain/2
- [ ] Monitor distributed training runs: per-node metrics, communication overhead #AI-300 #domain/2

---

### 📚 Module 2.8 — Drift, Monitoring & Retraining

- [ ] Understand data drift vs. concept drift vs. prediction drift #AI-300 #domain/2
- [ ] Configure a **Model Monitor** in AML for production endpoints #AI-300 #domain/2
- [ ] Define reference dataset and target dataset for drift comparison #AI-300 #domain/2
- [ ] Set drift magnitude thresholds and configure email/alert notifications #AI-300 #domain/2
- [ ] Interpret drift signals: feature attribution drift, data quality metrics #AI-300 #domain/2
- [ ] Implement automated retraining pipeline triggered by drift alert #AI-300 #domain/2
- [ ] Use Azure Monitor and Log Analytics to query endpoint telemetry #AI-300 #domain/2
- [ ] Understand collection mode: `production_data` vs. `reference_data` in monitors #AI-300 #domain/2

---

## 🤖 DOMAIN 3 — Design & Implement a GenAIOps Infrastructure

> [!note] Domain Weight: ~20% of exam · 5 core modules Key tools: **Microsoft Foundry**, **Azure AI Studio**, **Bicep**, **PromptFlow**, **Azure OpenAI**

### 📚 Module 3.1 — Foundry: Hubs, Projects & Platform Setup

- [ ] Understand Azure AI Foundry hierarchy: Hub → Project → Resources #AI-300 #domain/3
- [ ] Provision a Foundry Hub and linked Project using Bicep templates #AI-300 #domain/3
- [ ] Configure Hub-level shared resources: connections, compute, storage #AI-300 #domain/3
- [ ] Manage Project-level isolation: separate datasets, deployments, and keys #AI-300 #domain/3
- [ ] Configure RBAC for Foundry: AI Developer, AI Inference Deployment Operator roles #AI-300 #domain/3
- [ ] Understand Foundry's relationship with Azure OpenAI and Azure AI Services #AI-300 #domain/3
- [ ] Navigate Foundry portal: Playgrounds, Model Catalog, Deployments, Evaluations #AI-300 #domain/3

---

### 📚 Module 3.2 — Deploying Foundation Models

- [ ] Deploy models from the Foundry Model Catalog (OpenAI, Meta, Mistral, Cohere) #AI-300 #domain/3
- [ ] Understand deployment types: Azure OpenAI Service vs. Serverless API vs. Managed Compute #AI-300 #domain/3
- [ ] Configure Provisioned Throughput Units (PTUs) vs. token-based consumption #AI-300 #domain/3
- [ ] Set content filtering policies at the deployment level #AI-300 #domain/3
- [ ] Deploy open-source models (Llama, Phi) to managed compute endpoints #AI-300 #domain/3
- [ ] Compare model tiers: GPT-4o, GPT-4o mini, Phi-4, Llama-3 for cost/performance #AI-300 #domain/3
- [ ] Understand model quotas and capacity planning for production workloads #AI-300 #domain/3

---

### 📚 Module 3.3 — Model Versioning & Production Strategies

- [ ] Implement versioned model deployments in Foundry #AI-300 #domain/3
- [ ] Configure traffic splitting between model versions for A/B testing #AI-300 #domain/3
- [ ] Implement rollback strategy for GenAI deployments #AI-300 #domain/3
- [ ] Manage prompt versions as versioned assets in source control #AI-300 #domain/3
- [ ] Understand model lifecycle in Foundry: preview → GA → deprecated → retired #AI-300 #domain/3
- [ ] Use API versioning (`api-version` parameter) for Azure OpenAI compatibility #AI-300 #domain/3

---

### 📚 Module 3.4 — PromptOps: Design, Compare, Version & Ship

- [ ] Understand Prompt Flow as an orchestration framework for LLM apps #AI-300 #domain/3
- [ ] Build a basic Prompt Flow: LLM node → Python node → output #AI-300 #domain/3
- [ ] Configure connections (Azure OpenAI, cognitive search, custom) in Prompt Flow #AI-300 #domain/3
- [ ] Use variants to A/B test different prompt templates in a flow #AI-300 #domain/3
- [ ] Run batch evaluations over a dataset in Prompt Flow #AI-300 #domain/3
- [ ] Deploy a Prompt Flow to a Managed Online Endpoint #AI-300 #domain/3
- [ ] Version and track flows in Git; integrate with CI/CD #AI-300 #domain/3
- [ ] Understand system prompt engineering: persona, instructions, examples, constraints #AI-300 #domain/3

---

### 📚 Module 3.5 — Network Security & IaC for Foundry

- [ ] Configure Private Endpoints for Foundry Hub and Project #AI-300 #domain/3
- [ ] Implement Virtual Network (VNet) integration for Foundry managed compute #AI-300 #domain/3
- [ ] Use Managed VNet (Allow Internet Outbound vs. Allow Only Approved Outbound) #AI-300 #domain/3
- [ ] Configure outbound rules for necessary external dependencies #AI-300 #domain/3
- [ ] Implement customer-managed keys (CMK) for Foundry data encryption #AI-300 #domain/3
- [ ] Provision secure Foundry infrastructure with Bicep: Hub, Project, Private Endpoints #AI-300 #domain/3
- [ ] Understand service endpoints vs. private endpoints for PaaS resource access #AI-300 #domain/3

---

## 🔍 DOMAIN 4 — Implement GenAI Quality Assurance & Observability

> [!note] Domain Weight: ~15% of exam · 4 core modules Key tools: **Foundry Evaluations**, **Azure Monitor**, **Application Insights**, **Azure AI Content Safety**

### 📚 Module 4.1 — Evaluation: Datasets, Metrics & Quality Gates

- [ ] Understand GenAI quality dimensions: groundedness, relevance, coherence, fluency, similarity #AI-300 #domain/4
- [ ] Build a golden dataset (question + context + ground truth answer) for evaluation #AI-300 #domain/4
- [ ] Run a Foundry Evaluation using built-in AI-assisted metrics (GPT-4 as judge) #AI-300 #domain/4
- [ ] Define custom evaluators using Python functions or Prompt Flow flows #AI-300 #domain/4
- [ ] Set pass/fail thresholds for evaluation metrics as quality gates in CI/CD #AI-300 #domain/4
- [ ] Compare evaluation results across model versions and prompt variants #AI-300 #domain/4
- [ ] Understand BLEU, ROUGE, BERTScore for NLP evaluation context #AI-300 #domain/4

---

### 📚 Module 4.2 — Safety Evaluations & Custom Metrics

- [ ] Configure **Azure AI Content Safety** for hate, violence, sexual, self-harm detection #AI-300 #domain/4
- [ ] Run safety red-teaming evaluations in Foundry #AI-300 #domain/4
- [ ] Implement jailbreak detection and prompt shield evaluations #AI-300 #domain/4
- [ ] Build a custom metric evaluator as a Prompt Flow component #AI-300 #domain/4
- [ ] Integrate safety evaluation results into model approval gates #AI-300 #domain/4
- [ ] Understand indirect prompt injection attacks and detection strategies #AI-300 #domain/4
- [ ] Review and act on AI safety incident reporting workflows #AI-300 #domain/4

---

### 📚 Module 4.3 — Cost Tracking, Logging & Debugging

- [ ] Enable diagnostic logging on Azure OpenAI and Foundry deployments #AI-300 #domain/4
- [ ] Query token consumption logs in Log Analytics workspace #AI-300 #domain/4
- [ ] Set up Azure Monitor alerts for token quota thresholds #AI-300 #domain/4
- [ ] Implement cost allocation tagging at Project level for chargeback #AI-300 #domain/4
- [ ] Use Application Insights to trace end-to-end Prompt Flow execution #AI-300 #domain/4
- [ ] Debug LLM call failures: timeout, rate limit (429), content filter blocks #AI-300 #domain/4
- [ ] Understand Azure Cost Management + Billing for AI service optimization #AI-300 #domain/4

---

### 📚 Module 4.4 — Monitoring GenAI in Production

- [ ] Configure operational monitoring for Foundry endpoints: latency, TTFT, TPM, RPM #AI-300 #domain/4
- [ ] Set up Prompt Flow tracing for distributed observability #AI-300 #domain/4
- [ ] Implement conversation logging for offline quality analysis #AI-300 #domain/4
- [ ] Configure automated quality evaluations on sampled production traffic #AI-300 #domain/4
- [ ] Build Azure Monitor dashboards for GenAI operational KPIs #AI-300 #domain/4
- [ ] Implement alerting on quality degradation (e.g., groundedness score drops) #AI-300 #domain/4
- [ ] Understand drift monitoring for GenAI: distribution shift in user queries #AI-300 #domain/4

---

## ⚡ DOMAIN 5 — Optimize GenAI Systems & Model Performance

> [!note] Domain Weight: ~10% of exam · 3 core modules Key tools: **Azure AI Search**, **Fine-tuning APIs**, **Phi-4**, **RAG patterns**

### 📚 Module 5.1 — RAG Optimization: Better Retrieval, Better Answers

- [ ] Understand the RAG (Retrieval-Augmented Generation) architecture #AI-300 #domain/5
- [ ] Configure Azure AI Search as a vector store: embedding indexing, hybrid search #AI-300 #domain/5
- [ ] Implement semantic ranking and query rewriting for improved recall #AI-300 #domain/5
- [ ] Tune chunking strategy: chunk size, overlap, and splitting method #AI-300 #domain/5
- [ ] Implement query expansion and hypothetical document embedding (HyDE) #AI-300 #domain/5
- [ ] Measure RAG quality: retrieval precision/recall, end-to-end groundedness #AI-300 #domain/5
- [ ] Implement agentic RAG with multi-step tool calls and self-reflection #AI-300 #domain/5
- [ ] Understand Integrated Vectorization in Azure AI Search for automated indexing #AI-300 #domain/5

---

### 📚 Module 5.2 — Embeddings & Hybrid Search

- [ ] Understand dense embeddings (text-embedding-3-small/large) vs. sparse (BM25) #AI-300 #domain/5
- [ ] Configure a vector index in Azure AI Search with HNSW algorithm #AI-300 #domain/5
- [ ] Implement hybrid search: combine vector similarity + keyword (BM25) with RRF scoring #AI-300 #domain/5
- [ ] Evaluate embedding model quality on domain-specific datasets #AI-300 #domain/5
- [ ] Understand token limits and text truncation strategy for embedding generation #AI-300 #domain/5
- [ ] Use Azure AI Search Skillsets for automated enrichment pipelines #AI-300 #domain/5

---

### 📚 Module 5.3 — Fine-Tuning: Methods, Data & Production

- [ ] Understand when to fine-tune vs. RAG vs. prompt engineering #AI-300 #domain/5
- [ ] Prepare a fine-tuning dataset in JSONL format (system, user, assistant turns) #AI-300 #domain/5
- [ ] Submit a fine-tuning job via Azure OpenAI Fine-tuning API or Foundry #AI-300 #domain/5
- [ ] Monitor fine-tuning job metrics: training loss, validation loss, token accuracy #AI-300 #domain/5
- [ ] Deploy a fine-tuned model to a dedicated deployment in Azure OpenAI #AI-300 #domain/5
- [ ] Understand LoRA and QLoRA for parameter-efficient fine-tuning of open-source models #AI-300 #domain/5
- [ ] Evaluate fine-tuned model against base model on held-out evaluation set #AI-300 #domain/5
- [ ] Manage fine-tuned model lifecycle: versioning, rollback, cost amortization #AI-300 #domain/5

---

## 🧪 Hands-On Labs Tracker

> [!warning] Labs should be completed in a dedicated Azure subscription with budget alerts set to ≤ $50/month.

```dataview
TABLE WITHOUT ID
  file.link AS "Lab Note",
  status AS "Status",
  lab-type AS "Type",
  completion-date AS "Completed"
FROM "AI-300/Labs"
SORT file.name ASC
```

### 🔵 Infrastructure Labs

- [ ] **Lab 1.1** — Provision AML Workspace + dependencies via Bicep template #AI-300 #lab #domain/1
- [ ] **Lab 1.2** — Create compute cluster (CPU + GPU) via Azure CLI and Python SDK v2 #AI-300 #lab #domain/1
- [ ] **Lab 1.3** — Register ADLS Gen2 datastore and create MLTable data asset #AI-300 #lab #domain/1
- [ ] **Lab 1.4** — Build a custom Environment from Dockerfile and test locally #AI-300 #lab #domain/1
- [ ] **Lab 1.5** — Create a GitHub Actions workflow that deploys a Bicep template to AML #AI-300 #lab #domain/1

### 🟠 ML Lifecycle Labs

- [ ] **Lab 2.1** — Train a scikit-learn model with MLflow autologging in AML #AI-300 #lab #domain/2
- [ ] **Lab 2.2** — Build a 3-step AML Pipeline: prep → train → evaluate #AI-300 #lab #domain/2
- [ ] **Lab 2.3** — Run an AutoML classification job; interpret leaderboard and explanations #AI-300 #lab #domain/2
- [ ] **Lab 2.4** — Configure a Sweep Job with Bayesian sampling + Bandit early termination #AI-300 #lab #domain/2
- [ ] **Lab 2.5** — Register a winning model and add a model card with tags #AI-300 #lab #domain/2
- [ ] **Lab 2.6** — Generate a Responsible AI Dashboard (Error Analysis + Fairness + Explainability) #AI-300 #lab #domain/2
- [ ] **Lab 2.7** — Deploy model to Managed Online Endpoint with blue/green traffic split #AI-300 #lab #domain/2
- [ ] **Lab 2.8** — Deploy model to Batch Endpoint; invoke with a dataset URI #AI-300 #lab #domain/2
- [ ] **Lab 2.9** — Configure Model Monitor for a deployed endpoint; inject synthetic drift #AI-300 #lab #domain/2
- [ ] **Lab 2.10** — Implement a GitHub Actions approval gate for model promotion #AI-300 #lab #domain/2

### 🟣 GenAIOps Labs

- [ ] **Lab 3.1** — Provision Foundry Hub + Project with Bicep; configure private endpoints #AI-300 #lab #domain/3
- [ ] **Lab 3.2** — Deploy GPT-4o mini from Model Catalog; test in Foundry Playground #AI-300 #lab #domain/3
- [ ] **Lab 3.3** — Build a Prompt Flow RAG app: AI Search → LLM → output #AI-300 #lab #domain/3
- [ ] **Lab 3.4** — Use Prompt Flow variants to compare two prompt templates #AI-300 #lab #domain/3
- [ ] **Lab 3.5** — Deploy a Prompt Flow to Managed Online Endpoint; call via REST API #AI-300 #lab #domain/3
- [ ] **Lab 3.6** — Configure content filtering policy at deployment level; test blocked outputs #AI-300 #lab #domain/3

### 🟡 Evaluation & Monitoring Labs

- [ ] **Lab 4.1** — Run Foundry Evaluation on a golden dataset (groundedness, coherence, fluency) #AI-300 #lab #domain/4
- [ ] **Lab 4.2** — Enable diagnostic logging on Azure OpenAI; query token usage in Log Analytics #AI-300 #lab #domain/4
- [ ] **Lab 4.3** — Run safety red-teaming evaluation and review jailbreak detection results #AI-300 #lab #domain/4
- [ ] **Lab 4.4** — Configure a Monitor alert when token quota exceeds 80% #AI-300 #lab #domain/4

### 🟢 Optimization Labs

- [ ] **Lab 5.1** — Build a vector index in Azure AI Search; implement hybrid search with RRF #AI-300 #lab #domain/5
- [ ] **Lab 5.2** — Fine-tune GPT-4o mini on a custom dataset; compare against base model #AI-300 #lab #domain/5
- [ ] **Lab 5.3** — Implement chunking strategies; measure RAG precision/recall trade-offs #AI-300 #lab #domain/5

---

## 🚀 CI/CD Pipeline Blueprint

> [!example]+ Full MLOps CI/CD Workflow Reference

```
┌─────────────────────────────────────────────────────────────┐
│                    MLOps CI/CD Pipeline                      │
├──────────────┬──────────────┬──────────────┬────────────────┤
│   PR Trigger │  Dev Deploy  │  Staging Gate│  Prod Deploy   │
│              │              │              │                │
│ • Lint/Test  │ • Train job  │ • RAI check  │ • Blue/green   │
│ • Unit tests │ • Eval run   │ • Approval   │ • Canary 10%   │
│ • IaC plan   │ • Register   │ • Perf gate  │ • Monitor on   │
└──────────────┴──────────────┴──────────────┴────────────────┘
```

### CI/CD Study Checklist

- [ ] Design a GitHub Actions workflow with 4 stages: CI → train → validate → deploy #AI-300 #cicd
- [ ] Implement reusable GitHub Actions workflows for AML job submission #AI-300 #cicd
- [ ] Store secrets: `AZURE_CREDENTIALS`, `SUBSCRIPTION_ID`, `RESOURCE_GROUP` in GitHub Secrets #AI-300 #cicd
- [ ] Use `azure/login` action with federated OIDC (preferred over service principal secret) #AI-300 #cicd
- [ ] Implement GitHub Environments with required reviewers for production gates #AI-300 #cicd
- [ ] Trigger retraining pipeline on data drift alert via Event Grid → GitHub Actions webhook #AI-300 #cicd
- [ ] Implement model performance regression test: new model must beat baseline by ≥ 1% #AI-300 #cicd
- [ ] Store pipeline YAML in `pipelines/` folder; version in Git alongside model code #AI-300 #cicd
- [ ] Implement IaC drift detection: compare deployed state vs. Bicep template on schedule #AI-300 #cicd
- [ ] Configure branch protection: require passing CI before merge to `main` #AI-300 #cicd

---

## 🛡️ Responsible AI Deep Dive

> [!abstract] Microsoft Responsible AI Principles: **Fairness · Reliability & Safety · Privacy & Security · Inclusiveness · Transparency · Accountability**

### Principles Study Checklist

- [ ] Explain all 6 Microsoft Responsible AI principles with AML implementation examples #AI-300 #rai
- [ ] Use Responsible AI Dashboard: understand Error Analysis tree component #AI-300 #rai
- [ ] Use Responsible AI Dashboard: understand Fairness assessment and disaggregated metrics #AI-300 #rai
- [ ] Use Responsible AI Dashboard: understand Model Explainability (SHAP values, feature importance) #AI-300 #rai
- [ ] Use Responsible AI Dashboard: understand Data Explorer component for cohort analysis #AI-300 #rai
- [ ] Implement a model card documenting: intended use, out-of-scope uses, known limitations #AI-300 #rai
- [ ] Configure automated fairness evaluation metric as a pipeline quality gate #AI-300 #rai
- [ ] Understand differential privacy and its application in ML model training #AI-300 #rai
- [ ] Implement Azure AI Content Safety for harmful content filtering in GenAI apps #AI-300 #rai
- [ ] Understand Transparency Note requirements for Azure AI services #AI-300 #rai
- [ ] Document model lineage: dataset → training run → registered model → deployment #AI-300 #rai

---

## 📐 Azure ML Architecture Quick Reference

> [!tip]+ Key Architectural Patterns to Know for Exam

### Online Endpoint Decision Tree

```
Need real-time inference?
├── Yes → Online Endpoint
│   ├── AML-managed infra → Managed Online Endpoint (default)
│   └── Own AKS cluster  → Kubernetes Online Endpoint
└── No  → Batch Endpoint
    ├── <1000 rows, ad-hoc → Managed Online Endpoint (still ok)
    └── Large files, async → Batch Endpoint (best fit)
```

### Compute Selection Matrix

|Workload|Recommended Compute|Notes|
|---|---|---|
|Interactive dev/exploration|Compute Instance|Stop when not in use|
|Training (parallel)|Compute Cluster|min_nodes=0, auto-scale|
|Training (GPU/large)|Compute Cluster (NC/ND SKUs)|Low-priority for cost|
|Big data processing|Serverless Spark|Pay per use|
|Production inference (RT)|Managed Online Endpoint|Autoscale enabled|
|Production inference (batch)|Batch Endpoint|Compute Cluster backing|
|External frameworks|Attached Compute|Databricks / AKS / Arc|

### MLOps Maturity Levels

- [ ] Understand Level 0: Manual, ad-hoc processes — no automation #AI-300 #architecture
- [ ] Understand Level 1: ML pipeline automation — automated training #AI-300 #architecture
- [ ] Understand Level 2: CI/CD pipeline automation — automated deployment #AI-300 #architecture
- [ ] Map your organization's current maturity and identify gaps #AI-300 #architecture

---

## 🔑 Key Concepts Flash Review

> [!faq]+ Terms to Know Cold

- [ ] **MLflow** — Open-source platform for experiment tracking, model registry, and model serving #AI-300 #flashcard
- [ ] **AML Pipeline** — DAG of reusable Components for repeatable ML workflows #AI-300 #flashcard
- [ ] **Managed Online Endpoint** — AML-managed real-time inference endpoint with autoscale and traffic splitting #AI-300 #flashcard
- [ ] **Batch Endpoint** — Asynchronous endpoint for large-scale batch inference using Compute Clusters #AI-300 #flashcard
- [ ] **AutoML** — Automated algorithm/feature selection via parallelized child runs on a Compute Cluster #AI-300 #flashcard
- [ ] **Sweep Job** — Hyperparameter tuning job type with configurable search space and sampling strategy #AI-300 #flashcard
- [ ] **Data Drift** — Statistical shift between training data distribution and production inference data #AI-300 #flashcard
- [ ] **Responsible AI Dashboard** — AML Studio tool aggregating Error Analysis, Fairness, Explainability, Causal Analysis #AI-300 #flashcard
- [ ] **Prompt Flow** — Foundry's LLM app orchestration framework for building, evaluating, and deploying AI flows #AI-300 #flashcard
- [ ] **Azure AI Foundry** — Unified platform: Model Catalog, Prompt Flow, Evaluations, Safety, GenAIOps #AI-300 #flashcard
- [ ] **PTU** — Provisioned Throughput Unit: pre-purchased, dedicated Azure OpenAI capacity for predictable latency #AI-300 #flashcard
- [ ] **RAG** — Retrieval-Augmented Generation: ground LLM responses in retrieved documents to reduce hallucinations #AI-300 #flashcard
- [ ] **LoRA** — Low-Rank Adaptation: parameter-efficient fine-tuning that trains small rank-decomposition matrices #AI-300 #flashcard
- [ ] **Bicep** — Azure DSL for IaC; compiles to ARM templates for repeatable infrastructure deployment #AI-300 #flashcard
- [ ] **Model Monitor** — AML feature computing drift metrics by comparing production data to a reference baseline #AI-300 #flashcard
- [ ] **Environment** — Versioned, reproducible container spec (Docker + Conda) wrapping training/scoring code #AI-300 #flashcard
- [ ] **Component** — Reusable, versioned pipeline step with declared inputs, outputs, and compute requirements #AI-300 #flashcard
- [ ] **Groundedness** — GenAI quality metric: response only uses facts present in the retrieved context #AI-300 #flashcard
- [ ] **TTFT** — Time to First Token: key latency KPI for streaming GenAI endpoint performance #AI-300 #flashcard
- [ ] **Bandit Policy** — Early termination: cancels runs whose primary metric lags the best run by a slack factor #AI-300 #flashcard

---

## 📖 Study Resources & References

|Resource|Type|Priority|
|---|---|---|
|AI-300 Exam Page (Microsoft Learn)|Official|⭐⭐⭐|
|MS Learn: Operationalize ML Models (MLOps)|Learning Path · 4h 51m · 7 modules|⭐⭐⭐|
|MS Learn: Operationalize GenAI Apps (GenAIOps)|Learning Path · 6h 8m · 6 modules|⭐⭐⭐|
|Azure ML Python SDK v2 Docs|API Reference|⭐⭐⭐|
|Azure AI Foundry Documentation|Reference|⭐⭐⭐|
|Prompt Flow Documentation|Reference|⭐⭐|
|Responsible AI Dashboard Docs|Reference|⭐⭐|
|GitHub Actions for Azure|Reference|⭐⭐|
|Exam Sandbox (demo interface)|Practice|⭐⭐⭐|

### Study Resource Tasks

- [ ] Complete MS Learn Path: Operationalize ML Models (MLOps) — 7 modules #AI-300 #study
- [ ] Complete MS Learn Path: Operationalize GenAI Apps (GenAIOps) — 6 modules #AI-300 #study
- [ ] Walk through Exam Sandbox to familiarize with question format #AI-300 #study
- [ ] Review Azure ML SDK v2 `azure-ai-ml` package changelog for recent additions #AI-300 #study
- [ ] Set up exam in Pearson VUE; confirm ID requirements and remote proctoring setup #AI-300 #study

---

## 📅 Study Schedule Template

```dataview
CALENDAR file.ctime
FROM "AI-300"
WHERE contains(tags, "AI-300")
```

### Week-by-Week Plan

- [ ] **Week 1** — Domain 1 (all 5 modules) + Labs 1.1–1.5 #AI-300 #schedule 📅 2026-06-20
- [ ] **Week 2** — Domain 2 Modules 2.1–2.4 + Labs 2.1–2.5 #AI-300 #schedule 📅 2026-06-27
- [ ] **Week 3** — Domain 2 Modules 2.5–2.8 + Labs 2.6–2.10 #AI-300 #schedule 📅 2026-07-04
- [ ] **Week 4** — Domain 3 (all 5 modules) + Labs 3.1–3.6 #AI-300 #schedule 📅 2026-07-11
- [ ] **Week 5** — Domain 4 (all 4 modules) + Labs 4.1–4.4 #AI-300 #schedule 📅 2026-07-18
- [ ] **Week 6** — Domain 5 (all 3 modules) + Labs 5.1–5.3 #AI-300 #schedule 📅 2026-07-25
- [ ] **Week 7** — Full review: flash cards, exam sandbox, weak-area re-study #AI-300 #schedule 📅 2026-08-01
- [ ] **Week 8** — Mock exam, remaining gaps, exam logistics confirmation #AI-300 #schedule 📅 2026-08-08

---

## ⚠️ Exam Traps & Gotchas

- [ ] Review: `az ml job create` (SDK v2 YAML) vs. `az ml run submit-script` (legacy v1) #AI-300 #gotcha
- [ ] Review: Managed Online Endpoint uses `blue` as default deployment name — conventional, not required #AI-300 #gotcha
- [ ] Review: AutoML primary metric direction (minimize vs. maximize) varies by task type #AI-300 #gotcha
- [ ] Review: Model Monitor requires a **Managed Online Endpoint** — not supported on Batch Endpoints #AI-300 #gotcha
- [ ] Review: Prompt Flow connections are **Hub-scoped**, shared across all Projects in that Hub #AI-300 #gotcha
- [ ] Review: PTUs are reserved at deployment level — switching models requires a new deployment #AI-300 #gotcha
- [ ] Review: `min_instances` on Online Endpoint ≠ `min_nodes` on Compute Cluster — different scaling semantics #AI-300 #gotcha
- [ ] Review: Batch Endpoints invoke asynchronously — response is a job ID, not a prediction result #AI-300 #gotcha

---

## 📝 Running Study Notes

> _(Write freely below — Dataview ignores prose)_

---

_Dashboard · AI-300 Beta · Pass score: 700/1000 · Built for Obsidian + Dataview + Tasks plugins_

```

---

**What you get in this dashboard:**

| Section | Coverage |
|---|---|
| **5 Domain headers** | Exact exam weight callouts, all 25 modules broken out |
| **~190 checkboxes** | Every module topic, lab, CI/CD, RAI, architecture, flashcard, and gotcha |
| **4 Dataview queries** | Global summary table, folder-grouped progress, labs tracker, calendar view |
| **3 Tasks plugin blocks** | Due-today, overdue, and completed-today live queries |
| **CI/CD blueprint** | ASCII pipeline diagram + 10 workflow checklist items |
| **Compute selection matrix** | 7-row reference table for the exam's favorite decision scenario |
| **20 flash-card checkboxes** | Key terms defined in exam-relevant language |
| **8-week study schedule** | Pre-dated tasks the Tasks plugin can track automatically |
| **8 exam gotchas** | Documented traps that commonly trip up candidates |

**Setup tip:** Place this file at `AI-300/AI-300-Dashboard.md` in your vault. Individual lab notes go in `AI-300/Labs/` — the Dataview query on the Labs Tracker will auto-populate from there. If you'd like a companion **daily review note template** or a **per-domain deep-dive note** (e.g., a full MLflow or Prompt Flow reference note), just say the word.