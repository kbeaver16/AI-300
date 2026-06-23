# 📘 Datastores & Data Assets in Azure Machine Learning

_From: 6_Introduction_to_Datastores_and_DataAssets_in_Azure.mp3_

## SUMMARY

Datastores and Data Assets are foundational concepts in Azure Machine Learning.

- **Datastores** provide a secure, governed, and simplified way to connect Azure ML to underlying storage (Blob, ADLS, File Share).
- **Data Assets** provide versioning, lineage, and governance for individual files or folders used in ML workflows.
- **MLTable** is the final, structured representation of processed data used for training and validation.

> “The standardized way of interacting with your underlying storage options is by establishing them as data stores within the Azure Machine Learning workspace.”  
> “With data assets, you enable versioning, lineage tracking… and improve collaboration.”

---

## KEY CONCEPTS

### **Datastore**

A registered connection to an underlying Azure storage resource.  
Purpose:
- Abstracts away authentication, URLs, and credential handling
- Standardizes access to data
- Improves governance and collaboration
- Stores connection secrets securely (via Key Vault)

Supported storage types:
- Azure Blob Storage
- Azure Data Lake Storage Gen2
- Azure File Share

---

### **Data Asset**

A versioned reference to a specific file or folder inside a datastore.

Two types:
1. **File Data Asset** — a single file (e.g., `diabetes.csv`)
2. **Folder Data Asset** — a directory containing multiple files (CSVs, PDFs, images, etc.)

Benefits:
- Versioning
- Lineage tracking
- Reusability across pipelines
- Governance and compliance

---

### **MLTable**

A structured, tabular representation of processed data used for training and validation.

Created when:
- You preprocess raw data (CSV, PDFs, images)
- You convert it into rows/columns
- You register the final dataset as an MLTable asset

MLTable is the **actual dataset** used by training jobs.

---

### **Uniform Resource Identifiers (URIs)**

|URI Type|Purpose|
|---|---|
|**HTTP URI**|Access public data (public Blob, GitHub raw files)|
|**ABFS URI**|Access ADLS Gen2 storage|
|**AzureML URI**|Access data stored in a registered datastore|

Key point:  
Once a datastore is registered, **AzureML URIs replace HTTP/ABFS URIs** for cleaner, governed access.

---

## DEFINITIONS

- **Datastore Registration**  
  The act of linking a storage account to Azure ML so it can be accessed securely and consistently.
- **Lineage**  
  The ability to trace which version of data was used for which experiment or model.
- **Versioning**  
  Tracking changes to data assets over time.
- **Connection Information**  
  Credentials, keys, and metadata stored securely in Key Vault.

---

## STEP‑BY‑STEP PROCESSES

### **Process: Registering a Datastore**

1. Go to **Azure ML Studio → Data → Datastores**
2. Select **Create Datastore**
3. Choose storage type (Blob, ADLS, File Share)
4. Provide:
    - Storage account name
    - Container / filesystem
    - Authentication method (Account Key, SAS, Managed Identity)
5. Save — Azure ML stores credentials in **Key Vault**

---

### **Process: Creating a Data Asset**

1. Ensure datastore is registered
2. Go to **Data → Data Assets**
3. Select **Create Data Asset**
4. Choose:
    - File path (single file)
    - Folder path (multiple files)
5. Assign name + version
6. Register — asset now appears with version history

---

### **Process: Creating an MLTable**

1. Load raw data (CSV, PDFs, images)
2. Process using Pandas or Spark
3. Save processed output as MLTable format
4. Register MLTable as a **Data Asset**
5. Use MLTable in training jobs

---

## EXAMPLES

### Example: Datastore Registration

- Storage account: `mystorage123`
- Container: `raw-data`
- Registered as: `aml_datastore`
- Authentication: Account Key stored in Key Vault

### Example: Data Asset Versioning

- `diabetes.csv` → Data Asset: `diabetes_dataset`
- Versions:
    - v1: Raw CSV
    - v2: Cleaned CSV
    - v3: Feature‑engineered CSV

### Example: MLTable Usage

- After Spark preprocessing:
    - `cleaned_diabetes/MLTable`
    - Registered as `diabetes_mltable`
    - Used in training pipeline

---

## ACTIONABLE TAKEAWAYS

- Always register storage as a **datastore** before accessing it in notebooks or pipelines.
- Use **Data Assets** to track versions of training data — critical for reproducibility.
- Use **MLTable** as the final, structured dataset for training.
- Prefer **AzureML URIs** over HTTP/ABFS for governed access.
- Datastores + Data Assets = foundational building blocks for MLOps in Azure ML.

---

## TAGS

#AzureML #Datastores #DataAssets #MLTable #MLOps #DataEngineering #AzureBlob #ADLS #AI300

---

## BACKLINK SUGGESTIONS

- [[Azure ML – Data Assets & Datastores]]
- [[Azure ML – MLTable Format]]
- [[Azure ML – Training Pipelines]]
- [[Azure ML – Storage Architecture]]
- [[Azure ML – Workspace Architecture]]
- [[Azure ML – Compute Options]]