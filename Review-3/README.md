# FlowForge - Project & DevOps Infrastructure Documentation

## 1. Application Architecture

FlowForge is a multi-tenant microservices platform tailored for DevOps training. It incorporates a comprehensive 4-tier Role-Based Access Control (RBAC) system (platform_admin, org_owner, manager, member), AI-powered project summaries, real-time notifications, and Kanban task management.

### System Diagram

```mermaid
graph TD
    A[Frontend (Next.js)] -->|:3000| B[Gateway :8000]
    B -->|JWT Validation| C[Auth Service :8001]
    B --> D[Project Service :8002]
    B --> E[Task Service :8003]
    B --> F[Analysis Service :8004]
    
    C --> G[(Redis)]
    D --> G
    E --> G
    F --> G
    
    G --> H[Notification Worker]
    H -->|SMTP| I[Email Notifications]
```

### Microservices
- **Frontend**: Next.js 14 App Router UI.
- **Gateway**: FastAPI Proxy handling JWT validation, routing, and rate limiting.
- **Auth Service**: Manages users, sessions, invites, and notifications.
- **Project Service**: Manages project lifecycle, members, and archives.
- **Task Service**: Manages tasks, comments, and approval workflows.
- **Analysis Service**: Handles daily analytics, GPT-powered summaries, and metrics aggregation.
- **Notification Worker**: Consumes Redis streams to dispatch events via SMTP.

---

## 2. CI/CD Workflows & Security

We employ Enterprise GitOps models using GitHub Actions combined with GitHub Environments to fully automate the delivery of both application code and infrastructure.

### Workflows Overview
- **Infrastructure Pipeline (`terraform-gitops.yml`)**:
  - **Triggers**: Push to `Cloud-Track-dev` (triggers Dev) or Push/Merge to `main` (triggers Prod).
  - **Actions**: Validates and formats code, runs Snyk IaC scanning, executes `terraform plan`, and halts at an Environment Gate. Upon manual approval, executes `terraform apply`.
- **Application Pipeline (`prod-release.yml`)**:
  - **Triggers**: Published releases (tags matching `<service>-v<version>`).
  - **Actions**: Retrieves Docker images from Dev, retags and pushes to GitHub Container Registry (GHCR) for Prod, updates Helm `values-prod.yaml`, and sends summary notification emails.
- **Service CI Pipelines**:
  - Push triggers that build and test microservices individually.

### Security & Federated Credentials
To avoid storing long-lived static Azure credentials, we authenticate GitHub Actions via **Azure OpenID Connect (OIDC)** (Federated Credentials).
- **Setup**: An Azure App Registration is configured to trust GitHub Actions from our specific repository and branches (`Cloud-Track-dev` and `main`).
- **Environments**: GitHub Environments (`dev` and `prod`) enforce manual review gates. You must explicitly approve deployments.

### Required Secrets
The following GitHub Repository Secrets are required:
- `AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, `AZURE_SUBSCRIPTION_ID`: For OIDC authentication to Azure.
- `GH_PAT`: GitHub Personal Access Token to clone/commit to related repositories (e.g., Helm repo updates).
- `SNYK_TOKEN`: For Infrastructure as Code security scanning.
- `MAIL_USERNAME`, `MAIL_PASSWORD`, `DEVELOPMENT_TEAM_EMAIL`: Used by the `dawidd6/action-send-mail` action to send pipeline status alerts.

---

## 3. Terraform Infrastructure

Our Terraform setup dictates the underlying cloud resources, enforcing a Single Physical Environment strategy.

### Structure
- **Modules** (`modules/`): Reusable infrastructure components.
- **Environments** (`env/dev` and `env/prod`): Environment-specific configurations tying modules together.

### How it Works
1. **State Management**: Terraform uses Azure Blob Storage as the remote backend. `use_oidc = true` is configured in `providers.tf` so Terraform securely authenticates using GitHub's OIDC token.
2. **Resource Group Scoping**: To limit blast radius, the Service Principal is assigned `Contributor` and `Role Based Access Control Administrator` roles *only* at the specific Resource Group level (e.g., `Noel-RG-Dev` or `Noel-RG-Prod`), rather than the whole subscription.
3. **Cost-Saving Strategy**: Although Dev runs `terraform plan`, we skip approval in Dev to save costs, only applying the infrastructure when merged to `main` (Prod).

---

## 4. Helm Deployment Setup

FlowForge is deployed via Kubernetes using an Umbrella Helm Chart.

### Structure
- **Umbrella Chart** (`Chart.yaml`): The master chart that defines all microservices as dependencies.
- **Subcharts** (`charts/`): Individual Helm charts for `gateway`, `frontend`, `auth-service`, `project-service`, `task-service`, `analysis-service`, and `notification-worker`.
- **Values Files**: Configurations are environment-specific (`values-dev.yaml`, `values-prod.yaml`).

### How it Works
Helm resolves the umbrella dependencies to deploy the entire stack simultaneously. The CI/CD pipelines automate image version tagging. For example, `prod-release.yml` uses `yq` to update the microservice `image.tag` inside `values-prod.yaml` and commits the changes back to the Helm repository, effectively triggering ArgoCD or the respective GitOps continuous delivery agent.

---

## 5. Deployment & Setup Guide

Follow these steps to setup the infrastructure and deploy the application from scratch:

### Step 1: Azure Preparation
1. Create a Resource Group and an Azure Storage Account + Blob Container for Terraform state.
2. Create two deployment Resource Groups (e.g., `RG-Dev`, `RG-Prod`).
3. Create an **App Registration (Service Principal)** in Microsoft Entra ID.
4. Establish **Federated Credentials (OIDC)** under the App Registration for `Cloud-Track-dev` and `main` branches.
5. Assign `Contributor` and `Role Based Access Control Administrator` roles to the App Registration scoped to the deployment Resource Groups.
6. Assign `Storage Blob Data Contributor` to the App Registration on the State Storage Account.

### Step 2: GitHub Secrets
Populate the repository secrets in GitHub:
`AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, `AZURE_SUBSCRIPTION_ID`, `GH_PAT`, `SNYK_TOKEN`, `MAIL_USERNAME`, `MAIL_PASSWORD`, `DEVELOPMENT_TEAM_EMAIL`.

### Step 3: Application Secrets
Application-specific secrets (e.g., `JWT_SECRET`, database passwords) should NOT be in source control.
1. Define them locally in a `secrets.env` file (ignored by git).
2. Connect securely to Azure (e.g., P2S VPN).
3. Use the included `./upload_secrets.sh -v <key-vault-name>` script to securely populate your private Azure Key Vault.

### Step 4: Infrastructure Provisioning
1. Push Terraform code to the `Cloud-Track-dev` branch.
2. Go to GitHub Actions, review the Dev `terraform plan` output.
3. Open a Pull Request to `main`. Once merged, approve the GitHub Environment Gate.
4. Terraform will execute `apply` and provision AKS, Key Vaults, Networking, etc.

### Step 5: Application Deployment
1. Wait for microservice pipelines to build and push images to GHCR.
2. The Helm repository will be automatically updated with new image tags.
3. Apply the Umbrella Helm chart to the AKS cluster (via GitOps/ArgoCD or manually using `helm install flowforge ./ -f values-prod.yaml -n flowforge-prod`).
