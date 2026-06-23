# 📘 Lab: Creating & Managing Compute Targets in Azure ML

_From: 5_Lab_Creating_and_Managing_Compute_Targets.mp3_

## SUMMARY

This lab walks through how to create and manage compute targets in Azure Machine Learning Studio. You create your first **managed compute instance**, configure its size, cost, shutdown behavior, and security settings, and learn how to start/stop it to control cost. This compute instance will power notebooks, jobs, and pipelines throughout the course.

> “A managed compute instance will absolutely work in our case… and will suffice for all the development that we’ll be doing.”

---

## KEY CONCEPTS

### **Compute Instance**

A single VM used for:
- Notebook execution
- Lightweight model training
- Running small jobs and pipelines
- Development & debugging

Characteristics:
- CPU or GPU options
- Pay‑per‑hour while running
- Can be manually started/stopped
- Supports idle shutdown scheduling

---

### **Compute Cluster**

(Not created in this lab, but referenced)  
A scalable cluster of VMs used for:
- Large training jobs
- Distributed workloads
- Auto‑scaling

---

### **Serverless Compute**

On‑demand compute for:
- Lightweight jobs
- Cost‑efficient execution
- No infrastructure management

---

### **Kubernetes Clusters (AKS)**

Used for:
- High‑scale inference
- Enterprise‑grade deployments

---

## DEFINITIONS

- **Idle Shutdown**  
  Automatically stops the compute instance after a period of inactivity.
- **VM Family / Size**  
  Determines CPU, RAM, storage, and cost characteristics.
- **Managed Compute Instance**  
  Azure‑provisioned VM with preconfigured ML tooling.
- **Tags**  
  Metadata used for cost tracking, environment labeling, or governance.

---

## STEP‑BY‑STEP PROCESSES

### **Process: Creating a Compute Instance in Azure ML Studio**

---

## EXAMPLES

### Example: Recommended VM Size

- **Standard_DS11_v2**
  - vCPU
  - 14 GB RAM
  - 28 GB storage
  - ~$0.17/hr
  - Ideal for notebooks + small datasets

### Example: When to Stop the Compute Instance
- After finishing a notebook session
- Overnight or weekends
- When running only serverless jobs

---

## ACTIONABLE TAKEAWAYS

- Compute Instances are perfect for development and small ML workloads.
- Always enable **idle shutdown** unless actively recording or presenting.
- Stop compute instances when not in use to avoid unnecessary billing.
- Use tags to track cost by environment (dev/stage/prod).
- This compute instance will power all notebooks, jobs, and pipelines in upcoming labs.

---

## TAGS

#AzureML #Compute #ComputeInstance #MLOps #AI300 #AzureMachineLearning #ModelTraining #NotebookCompute

---

## BACKLINK SUGGESTIONS

- [[Azure ML – Compute Options]]
- [[Azure ML – Training Pipelines]]
- [[Azure ML – Workspace Architecture]]
- [[Azure ML – Cost Optimization]]
- [[Azure ML – Development Environment Setup]]