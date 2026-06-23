# AI-300 MASTER TRACKER

## PLAN OVERVIEW
- Duration: 6 Weeks
- Study Time: 1.5–2 hours/day
- Learning Paths:
  - [MLOps Path](https://learn.microsoft.com/en-us/training/paths/operationalize-machinelearning-models/)
  - [GenAIOps Path](https://learn.microsoft.com/en-us/training/paths/operationalize-generativeai-applications/)

---

# WEEK 1 — MLOPS INFRASTRUCTURE (MILESTONE 1)

## OBJECTIVE
Design and implement an MLOps infrastructure  

## MODULES
- [ ] Trigger Azure ML jobs with GitHub Actions  
- [ ] Trigger GitHub Actions with feature-based development  
- [ ] Work with environments in GitHub Actions  

## CONCEPTS
- [ ] Azure ML workspace setup
- [ ] RBAC / Managed identities
- [ ] Bicep + CLI deployment
- [ ] GitHub Actions for ML

## HANDS-ON
- [ ] Deploy Azure ML workspace (IaC)
- [ ] Configure private endpoint
- [ ] Configure RBAC roles
- [ ] Create GitHub Actions workflow

## CHECKPOINT
- [ ] I can explain full MLOps infrastructure setup

---

# WEEK 2 — ML TRAINING + PIPELINES (MILESTONE 2)

## OBJECTIVE
Implement ML model lifecycle (training + orchestration)

## MODULES
- [ ] Experiment with Azure Machine Learning  
- [ ] Perform hyperparameter tuning  
- [ ] Run pipelines in Azure ML  

## CONCEPTS
- [ ] AutoML
- [ ] MLflow tracking
- [ ] Responsible AI dashboard
- [ ] Pipeline components + scheduling

## HANDS-ON
- [ ] Run AutoML experiment
- [ ] Compare MLflow vs AutoML
- [ ] Create pipeline:
  - [ ] Data prep
  - [ ] Train
  - [ ] Evaluate
- [ ] Schedule pipeline job

## CHECKPOINT
- [ ] I can design and run training pipelines

---

# WEEK 3 — MODEL DEPLOYMENT + OPERATIONS (MILESTONE 3)

## OBJECTIVE
Complete ML lifecycle (deployment + monitoring)

## MODULES
- [ ] Deploy a model with GitHub Actions  

## SELF-STUDY (CRITICAL)
- [ ] Model registry + versioning
- [ ] Online vs batch endpoints
- [ ] Data drift detection
- [ ] Retraining triggers

## HANDS-ON
- [ ] Build CD pipeline:
  - [ ] Register model
  - [ ] Deploy endpoint
  - [ ] Run smoke test
  - [ ] Implement rollback
- [ ] Enable drift monitoring
- [ ] Configure alert trigger

## CHECKPOINT
- [ ] I can explain full ML production pipeline

---

# WEEK 4 — GENAIOPS INFRASTRUCTURE (MILESTONE 4)

## OBJECTIVE
Design generative AI infrastructure

## MODULES
- [ ] Plan GenAIOps solution  
- [ ] Manage prompts in Foundry with GitHub  

## CONCEPTS
- [ ] Microsoft Foundry
- [ ] Prompt versioning
- [ ] Foundation model selection
- [ ] Serverless vs managed inference

## HANDS-ON
- [ ] Create Foundry project
- [ ] Deploy LLM endpoint
- [ ] Create prompt versions:
  - [ ] Variant A
  - [ ] Variant B
- [ ] Compare outputs

## CHECKPOINT
- [ ] I can explain GenAI infrastructure design

---

# WEEK 5 — GENAI QUALITY + OBSERVABILITY (MILESTONE 5)

## OBJECTIVE
Implement evaluation + monitoring

## MODULES
- [ ] Evaluate and optimize agents  
- [ ] Automate evaluations with GitHub Actions  
- [ ] Monitor GenAI application  

## CONCEPTS
- [ ] Evaluation metrics:
  - Groundedness
  - Relevance
  - Coherence
  - Fluency
- [ ] Safety + risk detection
- [ ] Cost monitoring (tokens)

## HANDS-ON
- [ ] Run structured evaluation
- [ ] Build evaluation pipeline (CI/CD)
- [ ] Add monitoring:
  - [ ] Latency
  - [ ] Cost
  - [ ] Output quality

## CHECKPOINT
- [ ] I can evaluate and monitor GenAI systems

---

# WEEK 6 — OPTIMIZATION + EXAM PREP (MILESTONE 6)

## OBJECTIVE
Optimize GenAI + prepare for exam

## MODULES
- [ ] Analyze and debug GenAI app with tracing  

## CONCEPTS
- [ ] OpenTelemetry tracing
- [ ] RAG optimization
- [ ] Embeddings tuning
- [ ] Hybrid search

## HANDS-ON
- [ ] Enable tracing
- [ ] Introduce issue → debug
- [ ] Optimize RAG:
  - [ ] Chunk size
  - [ ] Retrieval strategy
- [ ] Implement hybrid search

## FINAL PREP
- [ ] Review exam domains  
  - https://learn.microsoft.com/en-us/credentials/certifications/exams/ai-300/
- [ ] Take exam sandbox  
  - https://examlabs.microsoft.com/en/sandbox
- [ ] Take mock test (40–60 Q)
- [ ] Focus review:
  - Milestone 2 + 3 (highest weight)

## CHECKPOINT
- [ ] I can explain full AI system end-to-end

---

# FINAL READINESS SCORECARD

- [ ] MLOps Infrastructure ✅
- [ ] ML Lifecycle ✅
- [ ] Deployment + Monitoring ✅
- [ ] GenAIOps ✅
- [ ] Evaluation + Observability ✅
- [ ] Optimization ✅

---

# CAPSTONE (RECOMMENDED)

## BUILD COMPLETE SYSTEM
- [ ] ML pipeline (train + deploy)
- [ ] GenAI app (LLM + retrieval)
- [ ] Monitoring + evaluation
- [ ] CI/CD pipeline (end-to-end)