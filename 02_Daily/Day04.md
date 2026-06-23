# DAY 04 — MODULE
Work with environments in GitHub Actions

## MODULE LINK
- https://learn.microsoft.com/en-us/training/modules/work-environments-github-actions/
## TASKS
- [ ] Complete module
- [ ] Learn dev/test/prod environments
- [ ] Understand deployment promotion

## KEY CONCEPTS
- Environment segmentation
- Safe rollout strategies

## NOTES
# ⚙️ **Workflow Template: GitHub Actions with Environments**

This template shows a typical multi-environment deployment flow with approval gates and environment‑specific secrets.

```yaml
name: mlops-deploy

on:
  push:
    branches:
      - main

jobs:
  train:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Train model
        run: python train.py

      - name: Upload model artifact
        uses: actions/upload-artifact@v4
        with:
          name: model
          path: ./model.pkl

  validate:
    needs: train
    runs-on: ubuntu-latest
    environment:
      name: test
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: model

      - name: Validate model
        run: python validate.py

  deploy:
    needs: validate
    runs-on: ubuntu-latest
    environment:
      name: production
      url: https://your-prod-endpoint.example.com
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: model

      - name: Deploy model
        run: python deploy.py
        env:
          API_KEY: ${{ secrets.API_KEY }}
```

---

# 🧩 **How This Template Maps to the Module**

- **train job** → dev environment (implicit)
- **validate job** → test environment with scoped secrets
- **deploy job** → production environment with approval gates
- **environment blocks** → trigger protection rules and unlock environment secrets
- **artifact passing** → simulates ML model promotion across environments
