# DAY 05 — HANDS-ON: MLOPS INFRASTRUCTURE

## OBJECTIVE  
Build a secure, production-style Azure ML environment with identity, CI/CD, and networking

---
## PART 1 — DEPLOY AZURE ML WORKSPACE

### GOAL  
Stand up the core ML infrastructure

### STEPS
1. Navigate to Azure Portal
2. Create new resource → Machine Learning
3. Configure:
    - Resource group (new or existing)
    - Workspace name (unique)
    - Region (same across all resources)
    - Leave defaults for:
        - Storage
        - Key Vault
        - App Insights
4. Create resource
5. After deployment:
    - Launch ML Studio (ml.azure.com)
    - Verify workspace loads
    - Open "Compute" tab
6. Create compute instance:
    - CPU-based VM
    - Default size is fine

### CHECKPOINT
- Workspace created and accessible
- ML Studio launches successfully
- Compute instance exists

---

## PART 2 — CONFIGURE IDENTITY + RBAC

### GOAL  
Control access to ML workspace and resources

### STEPS
1. Go to ML workspace → Access control (IAM)
2. Review existing role assignments
3. Add role assignment:
    - Role: Contributor (or ML-specific role)
    - Assign to: Your user account
4. Locate Managed Identity:
    - Go to "Identity" blade
    - Ensure system-assigned identity is ON
5. Validate permissions:
    - Confirm workspace identity has access to:
        - Storage account
        - Key Vault
        - Container Registry

### CHECKPOINT

- You can access all workspace resources
- Managed identity exists and is enabled

### KEY UNDERSTANDING
- RBAC controls WHO can access
- Managed Identity controls HOW services authenticate

---

## PART 3 — CREATE GITHUB WORKFLOW (CI/CD)

### GOAL  
Automate ML operations using GitHub Actions

STEPS
1. Create GitHub repo (if not already done)
2. Add workflow file:
    - Path: .github/workflows/mlops.yml
3. Create basic workflow:
    - Trigger: push OR manual trigger
    - Job: run Azure CLI or ML job
``` yaml
#Example structure (minimal):

name: AML Workflow
on: [push]
jobs: build: runs-on: ubuntu-latest
steps:
  - uses: actions/checkout@v3
  - name: Login to Azure
    run: echo "Login step here"
  - name: Run ML job
    run: echo "Trigger AML job"
```
4. Configure authentication:
    - Use service principal OR OIDC
    - Add secrets:
        - AZURE_CLIENT_ID
        - AZURE_TENANT_ID
        - AZURE_SUBSCRIPTION_ID

### CHECKPOINT

- Workflow file exists
- GitHub Actions runs successfully
- Repo is connected to pipeline

### KEY UNDERSTANDING

- This is your CI/CD layer
- Automates training + deployment

---

## PART 4 — RESTRICT NETWORK (PRIVATE ENDPOINT)

### GOAL  
Secure workspace using private connectivity

STEPS
1. Create Virtual Network:
    - Address space: e.g., 10.0.0.0/16
    - Create subnet
2. Go to ML workspace → Networking
3. Configure:
    - Disable public access (if available)
4. Create Private Endpoint:
    - Link to ML workspace
    - Select VNet + subnet
5. Approve private endpoint connection
6. Validate:
    - Workspace accessible only via VNet
    - Public endpoint access restricted

### CHECKPOINT
- Private endpoint exists
- Workspace tied to VNet
- Public access restricted or disabled

### KEY UNDERSTANDING
- Private endpoint = network isolation
- Prevents data exfiltration

---

## FINAL VALIDATION — END-TO-END MLOPS INFRASTRUCTURE

**YOU SHOULD NOW BE ABLE TO EXPLAIN:**
- Workspace is the central control plane
- RBAC controls access
- Managed identity handles service auth
- GitHub Actions automates workflows
- Private endpoint secures network access

---

*OPTIONAL (HIGH VALUE FOR EXAM + REAL SKILL)*

DO IF TIME REMAINS

- Create Bicep or ARM template for workspace
- Trigger ML job from GitHub workflow
- Add separate dev/test/prod environments

---

## TASKS
- [x] Deploy Azure ML workspace ✅ 2026-06-05
- [x] Configure RBAC ✅ 2026-06-05
- [x] Create GitHub workflow ✅ 2026-06-05
- [x] Restrict network (private endpoint) ✅ 2026-06-05

## KEY CONCEPTS
- Identity + networking
- Infrastructure as code

## CHECKPOINT
- [x] Can explain MLOps infrastructure end-to-end ✅ 2026-06-05