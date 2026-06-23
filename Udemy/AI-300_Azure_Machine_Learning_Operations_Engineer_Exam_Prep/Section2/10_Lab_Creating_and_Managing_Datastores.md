# 📘 Lab: Creating & Managing Datastores and Data Assets in Azure ML

_From: 10_Lab_Creating_and_Managing_Datastores.mp3_

## SUMMARY

This lab walks through the full, end‑to‑end workflow of creating a **new Azure Blob Storage account**, registering it as a **Datastore**, uploading a CSV file, and registering it as both a **URI File Data Asset** and an **MLTable Data Asset**. You also learn how the Azure ML SDK v2 interacts with datastores, how default datastores work, and why MLTable is the preferred format for training.

> “We aim to cover the entire journey of building things up from scratch… and that should hopefully solidify your concepts.”

---

## KEY CONCEPTS

### **Datastore**

A registered connection to an Azure storage resource (Blob, ADLS).  
Purpose:
- Simplifies authentication
- Standardizes data access
- Stores credentials securely in Key Vault
- Enables governance & collaboration

---

### **Data Asset**

A versioned reference to a file or folder inside a datastore.

Types:
- **URI File** — single file
- **URI Folder** — folder of files
- **MLTable** — structured dataset used for training

---

### **MLTable**

A structured, versioned dataset created from raw data (CSV, images, PDFs).  
Used for:
- Training
- Validation
- Pipelines

MLTable is the **preferred dataset format** in Azure ML.

---

### **Default Datastore**

The datastore used automatically by:
- Notebooks
- Jobs
- MLTable creation
- Data uploads

Important nuance:  
Even if you change the workspace default datastore, **your notebook still uses the datastore backing the compute instance** unless explicitly overridden.

---

### **Azure ML SDK v2**

Used to:
- Authenticate
- Create MLClient
- Register datastores
- Register data assets
- Create MLTable assets

---

## DEFINITIONS

- **Workspace Blob Store**  
    The default storage created with the workspace.
- **AML Data Store**  
    The custom datastore created in this lab.
- **URI**  
    A path used to reference data (AzureML://, abfs://, https://).
- **MLClient**  
    The SDK v2 client used to interact with Azure ML.

---

## STEP‑BY‑STEP PROCESSES

### **Process: Creating a New Azure Blob Storage Account**

1. Go to Azure Portal → Create Storage Account
2. Use same resource group as workspace
3. Name: `amldatastore` (lowercase only)
4. Region: same as workspace
5. Enable **Blob Anonymous Access** (for demo simplicity)
6. Create a container named `datastore`
7. Upload `diabetes.csv`

---

### **Process: Registering the Storage Account as a Datastore**

1. Collect:
    - Storage account name
    - Container name
    - Account key
2. Use SDK v2 to create `AzureBlobDatastore`
3. Register via `ml_client.datastores.create_or_update()`
4. Verify in Studio → Data → Datastores

---

### **Process: Setting the New Datastore as Default**

1. In Studio → Datastores
2. Select `amldatastore`
3. Click **Set as Default**
4. Verify via SDK: `ml_client.datastores.get_default()`

---

### **Process: Registering a URI File Data Asset**

1. Construct AzureML URI:  
    `azureml://datastores/<datastore_name>/paths/diabetes.csv`
2. Create Data Asset of type `uri_file`
3. Register via SDK
4. Verify in Studio → Data Assets

---

### **Process: Creating & Registering an MLTable**

1. Use AzureML URI to load CSV
2. Create MLTable locally on compute instance
3. Save MLTable folder locally
4. Register MLTable as a Data Asset
5. Verify in Studio → Data Assets → Explore

---

## CODE BLOCKS

### **Create MLClient**

```python
from azure.ai.ml import MLClient
from azure.identity import DefaultAzureCredential

ml_client = MLClient.from_config(credential=DefaultAzureCredential())
```

### **Register Datastore**

```python
from azure.ai.ml.entities import AzureBlobDatastore

datastore = AzureBlobDatastore(
    name="aml_datastore",
    account_name=storage_account_name,
    container_name=container_name,
    credentials={"account_key": account_key}
)

ml_client.datastores.create_or_update(datastore)
```

### **Register URI File Data Asset**

```python
from azure.ai.ml.entities import Data

ds_path = f"azureml://datastores/{datastore_name}/paths/diabetes.csv"

data_asset = Data(
    name="diabetes-data",
    path=ds_path,
    type="uri_file",
    description="Diabetes dataset uploaded to datastore"
)

ml_client.data.create_or_update(data_asset)
```

### **Create & Register MLTable**

```python
from azure.ai.ml.entities import Data
from mltable import MLTable

paths = [
    {
        "file": f"azureml://subscriptions/{ml_client.subscription_id}/"
                f"resourcegroups/{ml_client.resource_group_name}/"
                f"workspaces/{ml_client.workspace_name}/"
                f"datastores/{datastore_name}/paths/diabetes.csv"
    }
]

mltable = MLTable(paths=paths)
mltable.save("diabetes_mltable")

mltable_asset = Data(
    name="diabetes-mltable",
    path="diabetes_mltable",
    type="mltable"
)

ml_client.data.create_or_update(mltable_asset)
```

---

## EXAMPLES

### Example: Data Asset Versions

- `diabetes-data`
    - v1: raw CSV
- `diabetes-mltable`
    - v1: structured MLTable

### Example: Why MLTable Uses Workspace Blob Store

- Notebook runs on compute instance
- Compute instance uses workspace blob store for local file storage
- MLTable saved locally → uploaded to workspace blob store

---

## ACTIONABLE TAKEAWAYS

- Always register storage as a **Datastore** before using it in notebooks or pipelines.
- Use **URI File** assets for raw files; use **MLTable** for training.
- MLTable is the preferred dataset format in Azure ML.
- Default datastore for the workspace ≠ datastore used by the notebook.
- Use SDK v2 for all modern Azure ML workflows.

---

## TAGS

#AzureML #Datastores #DataAssets #MLTable #MLOps #AzureBlob #SDKv2 #AI300 #DataEngineering

---

## BACKLINK SUGGESTIONS

- [[Azure ML – Datastores & Data Assets]]
- [[Azure ML – MLTable Format]]
- [[Azure ML – SDK v2 Usage]]
- [[Azure ML – Training Pipelines]]
- [[Azure ML – Workspace Architecture]]
- [[Azure ML – Compute Instances]]

---

If you want, I can also generate **atomic notes** for each concept (Datastore, Data Asset, MLTable, AzureML URIs, etc.) or build a **visual workflow diagram** for your vault.