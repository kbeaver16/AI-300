# 18 – Lab: Creating a Real-Time Inference Pipeline

---

## 📌 Key Concepts

- **Real-Time Inference Pipeline:** a variant of the training pipeline that accepts live API input and returns predictions
- **Web Service Input / Output:** auto-generated Designer components that handle incoming API data and return responses
- **Enter Data Manually:** component used to define the expected input schema (CSV format) for the API
- **Execute Python Script:** component that post-processes the scored output (renames `Scored Labels` → `predicted_price`)
- **Score Model:** component that runs the trained model against incoming data to produce predictions
- **Evaluate Model is removed:** not needed at inference time — only relevant during training
- **Apply Transformation:** component from training pipeline; must be removed in inference pipeline (causes errors if price column is absent)
- **Azure Container Instance (ACI):** the lightweight compute target used to host the real-time endpoint
- **ACI Contributor Role:** a required IAM role assigned to the Azure ML system-managed identity before deployment
- **Experiment Scoping:** inference pipeline submitted as a new experiment separate from the training experiment

---

## 📖 Definitions

| Term | Definition |
|------|------------|
| Real-Time Inference Pipeline | A pipeline built from a trained model to serve live predictions via an API endpoint |
| Web Service Input | Designer component that receives incoming request data |
| Web Service Output | Designer component that returns prediction results to the caller |
| Enter Data Manually | Component for defining and typing in the input schema CSV for the model |
| Execute Python Script | Component for running custom Python code within the pipeline |
| Scored Labels | The column name output by the Score Model component containing predictions |
| Apply Transformation | Training-time component that breaks inference if the label column (price) is absent from input |
| Azure Container Instance (ACI) | Lightweight, serverless Azure container used to host real-time endpoints |
| ACI Contributor Role | IAM role giving Azure ML workspace permission to create and manage ACI deployments |
| steps.md | GitHub repository markdown file containing all lab assets (CSV schemas, Python scripts) |

---

## 💡 Examples

- **Input schema (CSV entered manually):** columns match all feature X variables (e.g. `symboling`, `normalized-losses`, `make`, `fuel-type`, etc.) — but **not** the `price` column
- **Renamed output:** `Scored Labels` → `predicted_price` via the Execute Python Script component
- **Error encountered:** `column with name or index price does not exist in dataset but exists in transformation` → resolved by deleting the Apply Transformation and Clean Missing Data components

---

## 🔢 Step-by-Step Processes

### Converting Training Pipeline → Real-Time Inference Pipeline

1. Go to **Jobs** → open the completed automobile price prediction experiment → open latest job
2. Click ⋮ → **Create Inference Pipeline** → **Real-Time Inference Pipeline**
3. Designer auto-generates the new pipeline with **Web Service Input** and **Web Service Output** components
4. **Delete** the following components (not needed at inference time):
   - `Automobile Price - Store Data`
   - `Convert to Dataset`
5. Add **Enter Data Manually** component:
   - Open `steps.md` in GitHub → find the CSV schema → paste it in
6. Connect **Enter Data Manually** → **Select Columns in Dataset**
7. **Delete** the **Evaluate Model** component
8. Add **Execute Python Script** component:
   - Place between Score Model and Web Service Output
   - Delete the connection between Web Service Output and Score Model
   - Connect: Score Model → Execute Python Script → Web Service Output
9. Paste the Python renaming script from `steps.md` into Execute Python Script
10. Save the pipeline (name: `automobile pipeline real-time inference`)
11. Click ⋮ → **Configure and Submit** → create experiment `automobile real-time pipeline`
12. If the run fails with Apply Transformation error:
    - Delete **Apply Transformation** and **Clean Missing Data** components
    - Re-connect **Select Columns in Dataset output** → **Score Model input**
    - Re-submit the run (same experiment, job name: `automobile pipeline real-time inference - take two`)
13. Once the pipeline completes (all green ticks):
    - Assign **ACI Contributor** role to Azure ML workspace system-managed identity in the resource group's IAM
14. Click ⋮ → **Deploy** → **New Real-Time Endpoint**
    - Endpoint name: `automobile-price-prediction`
    - Compute type: **Azure Container Instance**
15. Monitor endpoint status in **Assets → Endpoints** until state changes to **Healthy**

---

## 💻 Code Blocks

```python
# Execute Python Script — post-processing component
# Renames 'Scored Labels' to 'predicted_price' and returns it as API output

import pandas as pd

def azureml_main(dataframe1=None, dataframe2=None):
    # Rename scored labels column to a human-readable name
    dataframe1 = dataframe1.rename(columns={'Scored Labels': 'predicted_price'})
    # Return only the predicted price column
    return dataframe1[['predicted_price']]
```

```csv
# Enter Data Manually — example input schema (CSV pasted into component)
symboling,normalized-losses,make,fuel-type,aspiration,num-of-doors,body-style,...
3,?,alfa-romero,gas,std,two,convertible,...
```

---

## 📝 Summary

> "We added this enter data manually CSV component that sits in the data type that the model could expect. And then rest of the components were from the previous pipeline that we created for training the model."

This lab transforms the trained automobile price prediction pipeline into a deployable real-time inference service. The key modifications are: replacing the training dataset with **Enter Data Manually** (to define the API input schema), removing the **Evaluate Model** component, and inserting an **Execute Python Script** to clean the output. A critical IAM step — granting the **ACI Contributor** role — is required before deploying to an Azure Container Instance endpoint.

---

## ✅ Actionable Takeaways

- Always start from a **completed training pipeline job** when creating an inference pipeline — not from the designer directly
- Delete `Apply Transformation` and `Clean Missing Data` from inference pipelines — they reference the training label column which won't be in incoming requests
- Use `steps.md` in the course GitHub repo for the pre-built CSV schema and Execute Python Script code
- Grant the **ACI Contributor** IAM role to the ML workspace managed identity **before** attempting to deploy
- Submit the inference pipeline as a **separate experiment** from the training experiment
- Verify the endpoint transitions from **Provisioning → Unhealthy → Healthy** before querying it

---

## 🏷️ Tags

`#azure-ml` `#inference-pipeline` `#real-time-endpoint` `#pipeline-designer` `#aci` `#api` `#lab` `#deployment` `#execute-python-script` `#web-service` `#automobile-price`

---

## 🔗 Backlink Suggestions

- [[17 – Lab Training a Cost Function Prediction Model with Designer]] — prerequisite: the training pipeline converted here
- [[19 – Deploying the Model to Azure Container Instance]] — next step: querying the deployed endpoint with curl
- [[20 – Anatomy of Azure ML Jobs Experiments and Runs]] — explains the experiment/job model used in this lab
- [[16 – Understanding Linear Algorithms]] — the linear regression model powering this inference pipeline
