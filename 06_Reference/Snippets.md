# AI-300 SNIPPETS

## CREATE WORKSPACE
az ml workspace create --name ai300-ws --resource-group ai300-rg

---

## DEPLOY MODEL
az ml online-endpoint create --name endpoint1

---

## REGISTER MODEL
az ml model create --name model1 --path ./model

---

## START EXPERIMENT (PYTHON)
mlflow.start_run()

---

## OPENAI CALL (PYTHON)
client.chat.completions.create(...)

---

## PIPELINE YAML
jobs:
  step1:
    command: python train.py

---

## MONITOR METRICS
Use Azure Monitor + App Insights

---

## KEY EXAM FOCUS
- Pipelines
- Deployment automation
- Monitoring
- GenAI evaluation