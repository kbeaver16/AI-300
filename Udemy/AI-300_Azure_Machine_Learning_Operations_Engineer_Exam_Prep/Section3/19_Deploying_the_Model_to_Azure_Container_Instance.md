# 19 – Deploying the Model to Azure Container Instance

---

## 📌 Key Concepts

- **Inferencing a deployed model:** sending a POST REST API call to an endpoint and receiving a prediction
- **REST API Endpoint:** the URL that exposes the trained model for real-time consumption
- **Primary / Secondary Keys:** authentication credentials required in the API request `Authorization` header
- **cURL:** command-line HTTP client used to test the REST API endpoint
- **HTTP POST Request:** the method used to send feature data and receive a prediction
- **Status Code 200:** confirms the API call was successfully processed
- **Predicted Price:** the Y-variable (dependent variable) returned as the API response
- **Container Logs:** accessible from the endpoint tab; used for debugging API call issues
- **Consume Tab:** section of the endpoint page providing ready-made code snippets (Python, C#, cURL)
- **steps.md (GitHub repo):** contains the sample cURL request template for testing the endpoint

---

## 📖 Definitions

| Term | Definition |
|------|------------|
| REST API Endpoint | URL path where the deployed model listens for incoming POST requests |
| Primary Key | Authentication token used as Bearer token in the `Authorization` header |
| cURL | Command-line tool for making HTTP requests; used here for model inferencing |
| POST Request | HTTP method for sending data in the request body to an endpoint |
| HTTP 200 | Standard success response code indicating the request was processed correctly |
| Container Logs | Log files from the Azure Container Instance showing request/response history |
| Consume Tab | Azure ML endpoint UI tab containing endpoint URL, keys, and code snippets |
| Body / Payload | JSON or CSV data carrying the feature variables (X) sent to the model |

---

## 💡 Examples

- **Sample predicted output from the lab:**
  - Input: feature values for one automobile (make, fuel type, RPM, city MPG, highway MPG, etc.)
  - Output: `predicted_price = 13,936` (units in dollars)
- **HTTP Status 200** was returned after the cURL POST call — confirming successful inference
- The prediction was returned "under a couple of seconds" — demonstrating ACI's fast response time

---

## 🔢 Step-by-Step Processes

### Querying the Deployed Model via cURL

1. Go to **Assets → Endpoints** → open the `automobile-price-prediction` endpoint
2. Click the **Consume** tab
3. Note the **REST Endpoint URL** and **Primary Key**
4. Open the course GitHub repo → `machine-learning-models/linear-regression-model/steps.md`
5. Scroll to the bottom → locate the **sample cURL POST request template**
6. Copy the sample request → paste into **Notepad** (or any text editor)
7. Replace `<endpoint>` placeholder with the copied REST Endpoint URL
8. Replace `<API-key>` placeholder with the copied Primary Key
9. Open **Command Prompt** (CMD) on Windows
10. Paste the completed cURL command → press **Enter**
11. Observe the returned JSON response — look for `predicted_price` value
12. Verify the HTTP response shows **status 200**
13. Optionally: go to the endpoint → **Logs** tab → confirm the POST request appears in container logs

---

## 💻 Code Blocks

```bash
# Sample cURL POST request (fill in your endpoint URL and primary key)
curl -X POST "<YOUR_ENDPOINT_URL>/score" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <YOUR_PRIMARY_KEY>" \
  -d '{
    "data": [
      {
        "symboling": 3,
        "normalized-losses": null,
        "make": "alfa-romero",
        "fuel-type": "gas",
        "aspiration": "std",
        "num-of-doors": "two",
        "body-style": "convertible",
        "drive-wheels": "rwd",
        "engine-location": "front",
        "wheel-base": 88.6,
        "city-mpg": 21,
        "highway-mpg": 27
      }
    ]
  }'
```

```json
// Expected response format
{
  "predicted_price": 13936.0
}
```

---

## 📝 Summary

> "That's a pretty good serving of your model on Azure Container Instance. These instances are very lightweight and they work pretty fast — pretty damn fast."

This video demonstrates the end-to-end inferencing workflow: obtaining the REST endpoint URL and authentication key from the Azure ML Consume tab, constructing a cURL POST request with automobile feature data, and receiving a real-time price prediction back within seconds. The HTTP 200 status and container log entry confirm successful model execution. This completes the full training-to-inference lifecycle started in Labs 17 and 18.

---

## ✅ Actionable Takeaways

- Always copy both the **REST endpoint URL** and the **Primary Key** from the Consume tab before running any API test
- Use the `steps.md` template from the GitHub repo as your cURL starting point — don't write it from scratch
- Replace placeholders carefully: `<endpoint>` with the full URL, `<API-key>` with the actual key
- Test via **Command Prompt (CMD)** or PowerShell — not Jupyter — for quick validation
- Confirm success by checking: (1) a predicted price value in the response, and (2) HTTP 200 status
- Use the **Logs tab** on the endpoint page to debug any failed or unexpected API calls
- Delete/stop the ACI endpoint when not in use to avoid unnecessary Azure charges

---

## 🏷️ Tags

`#azure-ml` `#aci` `#azure-container-instance` `#rest-api` `#curl` `#inference` `#deployment` `#endpoint` `#lab` `#automobile-price` `#post-request` `#real-time`

---

## 🔗 Backlink Suggestions

- [[18 – Lab Creating a Realtime Inference Pipeline]] — prerequisite: the inference pipeline deployed in this video
- [[17 – Lab Training a Cost Function Prediction Model with Designer]] — the original trained model
- [[20 – Anatomy of Azure ML Jobs Experiments and Runs]] — the job framework that organised the pipeline runs
- [[23 – Introduction to AutoML]] — AutoML-generated models can also be deployed to ACI endpoints the same way
- [[24 – Lab Automated Training with AutoML]] — AutoML lab also produces a deployable endpoint
