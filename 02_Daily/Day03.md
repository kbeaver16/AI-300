# DAY 03 — MODULE 2
Trigger GitHub Actions with feature-based development

## MODULE LINK
- https://learn.microsoft.com/en-us/training/modules/work-environments-github-actions/

## TASKS
- [x] Complete module 📅 2026-06-02 ✅ 2026-06-02
- [x] Understand trunk-based dev 🛫 2026-06-02 ✅ 2026-06-02
- [x] Learn CI trigger patterns 🛫 2026-06-02 ✅ 2026-06-02

## KEY CONCEPTS
- Branching strategies
- PR-driven CI/CD

## NOTES
# 🧩 **Work with Environments in GitHub Actions — Notes**

## 🎯 **Purpose of the Module**

This module teaches how to use **GitHub Environments** as part of an **MLOps workflow**—specifically how to train, test, and deploy machine learning models using controlled, gated environments.

---

## 📘 **Learning Objectives**

You should be able to:

- **Set up environments** in GitHub.
- **Use environments** inside GitHub Actions workflows.
- **Add approval gates** (required reviewers) before promoting a model to the next environment.

---

## 🧱 **Prerequisites**

- Programming experience in **Python or R**
- Experience developing/training ML models
- Familiarity with **Azure Machine Learning** basics

---

## 🧭 **Where This Module Fits**

Part of the learning path:  
**Operationalize machine learning models (MLOps)**

---

# 🧩 **Core Concepts**

## 🏗️ What Are GitHub Environments?

A **GitHub Environment** is a named deployment target (e.g., _dev_, _test_, _prod_) that provides:

- **Scoped secrets**
- **Protection rules**
- **Approval gates**
- **Deployment history**

Environments help enforce **safe, controlled promotion** of ML models or applications.

---

## 🔐 Environment Secrets

- Secrets can be **environment‑specific** (e.g., dev vs. prod credentials).
- They override repository secrets when used inside workflows.
- Useful for ML pipelines where each environment has different:
    - compute targets
    - storage accounts
    - model registries

---

## 🚦 Protection Rules

Environments support rules such as:

- **Required reviewers** (approval gates)
- **Wait timers**
- **Branch restrictions**

These rules prevent accidental or unauthorized deployments—critical in MLOps where model promotion must be controlled.

---

## ⚙️ Using Environments in GitHub Actions

Inside a workflow, you specify an environment like:

```yaml
environment:
  name: production
```

When a job targets an environment:

- GitHub enforces protection rules
- Secrets for that environment become available
- Deployment history is recorded

This is essential for ML pipelines where you want to:

- Train in _dev_
- Validate in _test_
- Deploy to _prod_ only after approval

---

## 👥 Approval Gates

Approval gates allow you to:

- Require specific people or teams to approve a deployment
- Ensure model promotion follows governance rules
- Prevent unreviewed changes from reaching production

This is a key MLOps practice for responsible AI deployment.

---

# 🧪 Exercise (High-Level Summary)

The module includes a hands-on exercise where you:

- Create environments
- Configure secrets
- Add protection rules
- Update a workflow to deploy to those environments

This reinforces how environments control the flow of ML model deployment.

---

# 📝 Summary

GitHub Environments provide:

- **Separation of concerns** (dev/test/prod)
- **Security** via scoped secrets
- **Governance** via approval gates
- **Traceability** via deployment logs

They are a foundational component of **MLOps pipelines** built on GitHub Actions.

# 📘 **Vocabulary**

### **Environment**

A named deployment target in GitHub (e.g., _dev_, _test_, _prod_) that provides scoped secrets, protection rules, and deployment history.

### **Environment Secrets**

Secrets stored at the environment level. They override repository‑level secrets when a job targets that environment.

### **Protection Rules**

Policies applied to an environment, such as required reviewers, wait timers, or branch restrictions, that control when deployments can occur.

### **Required Reviewers**

Approval gates that must be satisfied before a job can deploy to an environment.

### **Deployment**

A GitHub Actions job that targets an environment. Deployments are tracked in the environment’s history.

### **Deployment History**

A log of all deployments to an environment, including who approved them and when.

### **Job Environment**

A job-level configuration in a workflow that specifies which environment the job deploys to.

### **Scoped Secrets**

Secrets that are only available to jobs running in a specific environment.

### **MLOps**

Operationalizing machine learning workflows, including training, testing, and deploying models using automated pipelines.
