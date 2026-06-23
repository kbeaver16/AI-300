# DAY 02 — MODULE 1
Trigger Azure Machine Learning jobs with GitHub Actions

## MODULE LINK
- https://learn.microsoft.com/en-us/training/modules/trigger-azure-machine-learn-jobs-github-actions/ 

## TASKS
- [x] Complete module 🛫 2026-06-01 ✅ 2026-06-01
- [x] Learn Azure ML CLI v2 usage 🛫 2026-06-01 ✅ 2026-06-01
- [x] Understand job automation flows 🛫 2026-06-01 ✅ 2026-06-01

## KEY CONCEPTS
- CI-triggered ML jobs
- GitHub Actions + Azure ML integration

## NOTES
## **1. Azure ML Job Submission Errors**

### **Root cause: RBAC permissions**

- The error  
    `Microsoft.MachineLearningServices/workspaces/codes/versions/read`  
    means the identity submitting the job **lacks Azure ML–specific permissions**.
- “Contributor” alone is **not enough** for Azure ML.
- Required roles:
    - **AzureML Data Scientist**
    - **AzureML Contributor**
    - (or Owner)

### **Identity mismatch issue**

- GitHub Actions was authenticating as a **different service principal** than the one roles were assigned to.
- Fix:
    - Recreate `AZURE_CREDENTIALS` secret.
    - Confirm identity with:
        
        ```yaml
        - run: az account show
        ```
        

---

## **2. GitHub Actions Variables vs Secrets**

### **Variables**

- Stored under: _Settings → Secrets and variables → Variables_
- Accessed with:
    
    ```
    ${{ vars.NAME }}
    ```
    

### **Secrets**

- Encrypted
- Accessed with:
    
    ```
    ${{ secrets.NAME }}
    ```
    

### **Your workflow fix**

Use a multiline block for the `run:` command:

```yaml
- name: Run Azure Machine Learning training job
  run: |
    az ml job create \
      -f src/job.yml \
      --stream \
      --resource-group ${{ vars.AZURE_RESOURCE_GROUP }} \
      --workspace-name ${{ vars.AZURE_WORKSPACE_NAME }}
```

---

## **3. YAML Multiline Commands**

- YAML does **not** automatically continue lines.
- Must use:
    
    ```
    run: |
    ```
    
- And use `\` for bash line continuation.

---

## **4. Hyperparameters in Your Training Script**

### **Explicit hyperparameter**

- `reg_rate` (regularization rate)
    - Passed via `--reg_rate`
    - Controls model complexity
    - Logged to MLflow

### **Implicit hyperparameters**

- `C = 1 / reg_rate`
- `solver="liblinear"`
- `max_iter` (default)
- `penalty` (default L2)

Hyperparameters = **values chosen before training**, not learned from data.

---

## **5. ROC Curve — What It Represents**

### **Axes**

- **X-axis:** False Positive Rate (FPR)
- **Y-axis:** True Positive Rate (TPR)

### **How you move along the curve**

- By sweeping the **classification threshold** from **1.0 → 0.0**.
- Threshold is **not shown** on the graph.
- Each point = TPR/FPR at a specific threshold.

### **Why both lines meet at (1,1)**

- At threshold = 0:
    - Model predicts **everything positive**
    - TPR = 1 (all positives caught)
    - FPR = 1 (all negatives misclassified)
- Every classifier — good or bad — ends at (1,1).

### **Interpretation of your ROC**

- Curve is above the diagonal → model is better than random.
- Moderate curvature → model has decent but not perfect separation.
- AUC logged separately summarizes the curve.

---

## **6. Key Concepts You Mastered Today**

- Azure ML job submission pipeline
- Service principal authentication
- RBAC roles required for ML workloads
- GitHub Actions variable handling
- YAML multiline syntax
- Hyperparameters vs learned parameters
- ROC curves, thresholds, TPR/FPR, and AUC

---
## HANDS-ON IDEAS
- Map GitHub trigger → ML job pipeline

## LAB
### Automate Model Training with GitHub Actions
[Instructions](https://microsoftlearning.github.io/mslearn-mlops/docs/06-automate-model-training.html)
### Notes
- Variables
	- AZURE_RESOURCE_GROUP:  `rg-ai300-l0ec15c13124845aba7`
	- AZURE_WORKSPACE_NAME: `mlw-ai300-l0ec15c13124845aba7`
	- AZURE_CREDENTIALS:
```json
The output includes credentials that you must protect. Be sure that you do not include these credentials in your code or check the credentials into your source control. For more information, see https://aka.ms/azadsp-cli
{
  "clientId": "125ebb8c-ea09-4a42-be1a-8b09003ad615",
  "clientSecret": "VaK8Q~HS3US2hJJQgh8ZKKzo6SPzhH1_riYXNdfn",
  "subscriptionId": "4664de08-cbd6-47ef-80f4-77d82fb58f33",
  "tenantId": "22381759-e502-459b-b6fc-d571c80c370f",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}

//NOTE When adding to secret in GitHub, only include:
// clientId
// clientSecret
// subscriptionId
// tenantId
```
- Two Issues with Lab
	- Service Principal had to be given the `AzureML Data Scientist` role, not just `Contributor`
	- In `.github/workflows/manual-trigger-job.yml`, the variable names need to be explicitly added to the workflow
	```yaml
	- name: Run Azure Machine Learning training job 
	  run: |
		  az ml job create -f src/job.yml --stream \
		  --resource-group \${{vars.AZURE_RESOURCE_GROUP}} \ 
		  --workspace-name ${{vars.AZURE_WORKSPACE_NAME}}
	```
	