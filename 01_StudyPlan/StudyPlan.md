# AI-300 STUDY PLAN (MICROSOFT LEARN ALIGNED)

> Primary reference:  
- [AI-300 Official Study Guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)

---

# WEEK 1 — FOUNDATIONS AND ARCHITECTURE

## DAY 01 — EXAM ORIENTATION
- [ ] Read study guide overview 
- [ ] Identify exam domains: 
  - MLOps
  - GenAIOps
  - Monitoring / Optimization
- [ ] Create study notebook 

---

## DAY 02 — INTRO TO AI OPS
- [ ] Learn MLOps fundamentals 
  - https://learn.microsoft.com/en-us/azure/machine-learning/concept-model-management-and-deployment
- [ ] Learn DevOps vs MLOps comparison 
- [ ] Document differences 

---

## DAY 03 — AZURE AI SERVICES
- [ ] Azure AI overview 
  - https://learn.microsoft.com/en-us/azure/ai-services/what-are-ai-services
- [ ] Azure OpenAI overview 
  - https://learn.microsoft.com/en-us/azure/ai-services/openai/overview
- [ ] Create service mapping diagram 

---

## DAY 04 — MODEL LIFECYCLE
- [ ] ML lifecycle overview 
  - https://learn.microsoft.com/en-us/azure/machine-learning/concept-ml-pipelines
- [ ] Map lifecycle to Azure services 

---

## DAY 05 — ARCHITECTURE DESIGN
- [ ] Draw end-to-end ML system 
- [ ] Identify: 
  - Inputs / outputs
  - Compute / storage
  - Monitoring points

---

## DAY 06 — HANDS-ON (LIGHT)
- [ ] Create Azure ML workspace 
  - https://learn.microsoft.com/en-us/azure/machine-learning/quickstart-create-resources
- [ ] Explore UI: 
  - Models
  - Compute
  - Endpoints

---

## DAY 07 — REVIEW
- [ ] Write summary: 
  - "How AI systems run in Azure"Study/AI-300/01_StudyPlan/StudyPlan.md

---

# WEEK 2 — MLOPS

## DAY 08 — PIPELINES
- [ ] ML pipelines 
  - https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-machine-learning-pipelines
- [ ] Sketch pipeline flow 

---

## DAY 09 — MODEL REGISTRY
- [ ] Model registry concepts 
  - https://learn.microsoft.com/en-us/azure/machine-learning/how-to-manage-models
- [ ] Document versioning strategy

---

## DAY 10 — DEPLOYMENT
- [ ] Real-time endpoints 
  - https://learn.microsoft.com/en-us/azure/machine-learning/how-to-deploy-managed-online-endpoints
- [ ] Batch endpoints overview 
- [ ] Compare both approaches 

---

## DAY 11 — CI/CD FOR ML
- [ ] ML CI/CD concepts 
  - https://learn.microsoft.com/en-us/azure/machine-learning/how-to-cicd-deploy
- [ ] Map pipeline triggers 

---

## DAY 12 — INFRA AS CODE
- [ ] Bicep intro 
  - https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/overview
- [ ] Identify resources to automate 

---

## DAY 13 — HANDS-ON BUILD
- [ ] Deploy model endpoint 
- [ ] Test API call 
- [ ] Capture logs 

---

## DAY 14 — REVIEW
- [ ] Summary: 
  - "How to productionize ML models"

---

# WEEK 3 — GENERATIVE AI

## DAY 15 — AZURE OPENAI
- [ ] Learn OpenAI concepts  
  - https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models
- [ ] Understand:
  - Tokens
  - Deployments

---

## DAY 16 — PROMPT ENGINEERING
- [ ] Prompt design  
  - https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/prompt-engineering
- [ ] Create 3 prompts
- [ ] Evaluate outputs

---

## DAY 17 — RAG
- [ ] Azure AI Search overview  
  - https://learn.microsoft.com/en-us/azure/search/search-what-is-azure-search
- [ ] RAG pattern  
  - https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/use-cases#retrieval-augmented-generation
- [ ] Draw architecture

---

## DAY 18 — BUILD GENAI APP (PART 1)
- [ ] Quickstart  
  - https://learn.microsoft.com/en-us/azure/ai-services/openai/quickstart
- [ ] Build API → LLM call

---

## DAY 19 — BUILD GENAI APP (PART 2)
- [ ] Add retrieval layer
- [ ] Test improved responses

---

## DAY 20 — SYSTEM DESIGN
- [ ] Identify:
  - Latency risks
  - Failure modes
- [ ] Update architecture

---

## DAY 21 — REVIEW
- [ ] Summary:
  - "How GenAI apps are built and deployed"

---

# WEEK 4 — MONITORING AND OBSERVABILITY

## DAY 22 — MODEL MONITORING
- [ ] Monitoring concepts  
  - https://learn.microsoft.com/en-us/azure/machine-learning/concept-model-monitoring
- [ ] Define metrics

---

## DAY 23 — EVALUATION
- [ ] Evaluate LLM outputs  
  - https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/evaluation
- [ ] Define quality checks

---

## DAY 24 — LOGGING
- [ ] Application Insights  
  - https://learn.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview
- [ ] Design telemetry plan

---

## DAY 25 — RESPONSIBLE AI
- [ ] Responsible AI overview  
  - https://learn.microsoft.com/en-us/azure/ai-services/responsible-use-of-ai-overview
- [ ] Identify system risks

---

## DAY 26 — OBSERVABILITY
- [ ] Design alerts + dashboards
- [ ] Map signals → actions

---

## DAY 27 — HANDS-ON ENHANCEMENT
- [ ] Add logging to app
- [ ] Simulate failures

---

## DAY 28 — REVIEW
- [ ] Summary:
  - "How to monitor AI systems"

---

# WEEK 5 — OPTIMIZATION + EXAM PREP

## DAY 29 — PERFORMANCE
- [ ] Optimize latency  
  - https://learn.microsoft.com/en-us/azure/architecture/best-practices/performance-efficiency
- [ ] Identify bottlenecks

---

## DAY 30 — COST OPTIMIZATION
- [ ] Azure cost optimization  
  - https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/cost-management-best-practices
- [ ] Document savings strategies

---

## DAY 31 — SCALING
- [ ] Scaling apps  
  - https://learn.microsoft.com/en-us/azure/architecture/best-practices/scalability
- [ ] Add scaling design

---

## DAY 32 — FULL REVIEW
- [ ] Rebuild system from memory

---

## DAY 33 — PRACTICE
- [ ] Review all domains
- [ ] Identify weak areas

---

## DAY 34 — FINAL REVIEW
- [ ] Focus:
  - Tradeoffs
  - Architecture decisions

---

## DAY 35 — EXAM READINESS
- [ ] Explain:
  - Full AI system end-to-end
- [ ] Validate:
  - Confidence in all domains