# 📘 Introduction to Compute Targets in Azure Machine Learning

_From: 4_Introduction_to_Compute_Targets_in_Azure_ML.mp3_

## SUMMARY

Azure Machine Learning provides several **compute targets** used across the ML lifecycle: data processing, experimentation, training, and deployment. Each compute type has different performance, cost, and scalability characteristics. Choosing the right compute depends on model size, workload type, concurrency needs, and budget.

> “This visual is really beautiful just to identify the different types of computes… and where you would deploy them…”

---

## KEY CONCEPTS

### **Compute Instance**

A single VM used for:
- Notebook development
- Interactive experimentation
- Lightweight model training
- Debugging

Best for:
- Individual data scientists
- Small models
- Cost‑constrained scenarios

---

### **Compute Cluster**

A scalable cluster of VMs used for:
- Large‑scale training
- Distributed jobs
- Batch scoring
- Auto‑scaling workloads

Key features:
- Horizontal scaling
- Ideal for large models or pipelines
- Can scale to zero when idle

---

### **Serverless Spark**

A serverless data‑processing engine used for:
- Big data preprocessing
- Transformations on millions of rows
- Parallel data engineering workloads

Benefits:
- No infrastructure management
- Pay only for execution time
- Ideal before training (ETL stage)

---

### **Serverless Compute**

Used for:
- On‑demand job execution
- Lightweight batch inference
- Cost‑efficient workloads

You pay only for compute used during execution.

---

### **Kubernetes (AKS)**

Enterprise‑grade deployment target for:
- High‑concurrency real‑time inference
- Low‑latency production workloads
- Large‑scale traffic
- Auto‑scaling (HPA, VPA, event‑driven scaling)

Best for:
- Mission‑critical production systems
- High availability requirements

---

### **Managed Endpoints (ACI / Managed Compute)**

Used for:
- Real‑time inference
- Batch inference
- Model serving with managed infrastructure

Supports:
- Custom environments
- Containerized deployments
- Application Insights monitoring

---

## DEFINITIONS

- **Compute Target**  
  A compute resource used for training, inference, or data processing.
- **Auto‑Scaling**  
  Automatic adjustment of VM count based on workload.
- **Real‑Time Endpoint**  
  An HTTPS endpoint serving low‑latency predictions.
- **Batch Endpoint**  
  A scheduled or on‑demand job that processes large datasets.
- **Environment**  
  A containerized runtime with dependencies required to run a model.

---

## STEP‑BY‑STEP PROCESSES

### Choosing the Right Compute Target

Below is a clear, sequential decision guide for selecting compute in Azure ML.

---

## EXAMPLES

### Example: When to Use Compute Instance

- You’re prototyping a scikit‑learn model
- You need a Jupyter notebook environment
- You want low cost and no distributed training

### Example: When to Use Compute Cluster

- Training a deep learning model on large datasets
- Running hyperparameter sweeps
- Executing pipelines with multiple steps

### Example: When to Use AKS

- Your model must handle thousands of requests per second
- You need <50ms latency
- You require enterprise‑grade uptime

---

## ACTIONABLE TAKEAWAYS

- Use **Compute Instance** for development; **Compute Cluster** for training.
- Use **Serverless Spark** for data engineering and preprocessing.
- Use **Managed Endpoints** for simple deployments; **AKS** for high‑scale production.
- Always enable **autoscaling** for cost efficiency.
- Use **Application Insights** to monitor deployed models.

---

## TAGS

#AzureML #Compute #MLOps #MachineLearning #AKS #Serverless #Spark #ModelDeployment #AI300

---

## BACKLINK SUGGESTIONS

- [[Azure ML – Compute Options]]
- [[Azure ML – Model Deployment]]
- [[Azure ML – Data Processing]]
- [[Azure ML – Training Pipelines]]
- [[Azure ML – Real‑Time Endpoints]]
- [[Azure ML – Batch Endpoints]]