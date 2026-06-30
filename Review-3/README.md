# FlowForge: Comprehensive Platform & Infrastructure Documentation

![FlowForge App Architecture](../FlowForge/App%20Architecture.png)
![Kubernets Architecture](../FlowForge-Helm/Kubernets%20White.png)
![IaC Architecture](../FlowForge-Terraform/Cloud%20white.png)

This document is the **Single Source of Truth** for the entire FlowForge ecosystem. It integrates the application architecture, Terraform infrastructure, Helm/Kubernetes deployments, and the comprehensive set of GitOps CI/CD workflows used to run this Microsoft-Native Self-Hosted Project Management Suite.

---

## 1. Executive Summary & Application Architecture

FlowForge is a decoupled, multi-tenant microservices platform tailored for DevOps training. It integrates natively with Azure services (Entra ID, PostgreSQL Flexible Server, Azure Cache for Redis, Azure AI Foundry, Blob Storage, Key Vault) leveraging **Passwordless Authentication** via Managed Identities.

### 1.1 Microservices Layout
- **`frontend`** (Next.js 14): React App Router UI. Entra SSO params are baked in at build time.
- **`gateway`** (FastAPI): BFF Proxy handling JWT validation, Redis rate limiting, CORS, routing, and header injection (`X-User-ID`, `X-Org-ID`).
- **`auth-service`** (FastAPI): User management, Entra ID SSO, invitations, refresh tokens.
- **`project-service`** (FastAPI): Project CRUD, member management.
- **`task-service`** (FastAPI): Kanban boards, task lifecycle, manager approval gates.
- **`analysis-service`** (FastAPI): AI summaries via Azure AI Foundry, reports generation to Azure Blob.
- **`notification-worker`** (Python): Consumes Redis `audit_log` streams and dispatches HTML SMTP emails.

### 1.2 4-Tier Role-Based Access Control
FlowForge uses an Entra ID Security Group-driven RBAC system:
`platform_admin → org_owner → manager → member`
Roles are dynamically resolved at login; object IDs of these Entra Groups are synced securely via Key Vault.

---

## 2. Azure Infrastructure as Code (Terraform)

The physical cloud layer is provisioned via Terraform utilizing a Hub-and-Spoke topology designed for high security and ephemeral environment management.

### 2.1 Bootstrapping & Environment Strategy
- **`scripts/bootstrap_backend.ps1`**: A PowerShell script executed locally to bootstrap the remote state storage account and grant `Storage Blob Data Contributor` to the IaC App Registration.
- **`env/og-prod` (Bootstrap State)**: Used for initial persistent resources, like Azure Container Registry (ACR) and User Assigned Managed Identities.
- **`env/prod` / `env/dev`**: The ephemeral application clusters. They treat identities as `data` sources, allowing `terraform destroy` without deleting production container images or breaking OIDC federations.

### 2.2 Hub-and-Spoke Network Architecture
- **Hub VNet**: Contains the Management Subnet (Jumpbox VM) and VPN Gateway (Entra ID-authenticated Point-to-Site VPN).
- **Spoke VNet**: Contains:
  - **AKS Subnet**: For the Kubernetes cluster.
  - **App Gateway Subnet**: For the Application Gateway Ingress Controller (AGIC).
  - **Private Endpoints Subnet (`pe_subnet`)**: Hosts Private Links for Key Vault, Blob Storage, PostgreSQL, Redis, and AKS Control Plane. All traffic stays on Azure's private backbone.
- **Azure Firewall**: All AKS egress traffic routes through the firewall via User Defined Routes (UDR), strictly filtering traffic to allowed FQDNs.

### 2.3 Managed Identities & OIDC
We completely eliminate static secrets using Entra ID Workload Identities:
- `mi-github-actions-prod`: Used by CI to push images to ACR.
- `mi-flowforge-app-<env>`: Used by pods to read Key Vaults and Blob Storage.
- `mi-arc-runner-<suffix>`: Used by the self-hosted GitHub Actions runner to sync Key Vault secrets.
- `mi-flowforge-otel-<suffix>`: Used to push OpenTelemetry to Application Insights.
- `mi-ai-<env>`: Used for AI Foundry model inferences.
- `mi-aks-cp`: AKS Control plane identity to manage routing/DNS.

### 2.4 Database Entra ID Bootstrapping (`db-permission`)
Postgres authentication is passwordless. Administrators use `db-permission/run-setup.ps1` to request a short-lived OAuth 2.0 token via Azure CLI (`az account get-access-token --resource-type oss-rdbms`), which compiles into a Kubernetes batch job. This job logs into PostgreSQL and executes SQL to `GRANT` table schemas to the `mi-flowforge-app` and `mi-ai` managed identities.

---

## 3. Kubernetes Deployment & Helm (GitOps)

The deployment configuration is maintained in the `FlowForge-Helm` repository.

### 3.1 Umbrella Helm Chart
Located in `Helm/`, the chart resolves all 7 microservices as subcharts (`charts/`). It supports environment-specific values (`values-dev.yaml`, `values-prod.yaml`).
- **Global Config**: `global-config.yaml` provides shared ConfigMaps across pods.
- **Network Policies**: A namespace-wide egress/ingress firewall (`network-policy.yaml`) strictly blocks unexpected pod traffic, allowing only internal DNS, IMDS tokens, PostgreSQL, Redis, and SMTP.
- **Ingress**: `ingress.yaml` utilizes `azure-application-gateway` class to map the public domain.

### 3.2 GitOps with ArgoCD
- **`argocd/argocd-dev-app.yaml`**: Targets the `flowforge-dev` namespace, syncing automatically from the `Cloud-Track-dev` branch.
- **`argocd/argocd-prod-app.yaml`**: Targets the `flowforge-prod` namespace, syncing from the `main` branch.

### 3.3 Infrastructure & Telemetry Services (`infra/`)
- **Cert-Manager**: `issuer.yaml` uses `letsencrypt-prod` ACME HTTP-01 to automate SSL/TLS certificates.
- **OpenTelemetry**: `otel-dev/prod.yaml` collects traces and metrics, exporting them to Azure Monitor.
- **Prometheus & Grafana**: `prom-values.yaml` configures the Prometheus-Community stack, adding custom Alertmanager alerts for `PodCrashLoopingCustom` and high API error rates.
- **Actions Runner Controller (ARC)**: Self-hosted runners in `arc-system` namespace.

---

## 4. Comprehensive CI/CD Workflows (GitHub Actions)

FlowForge utilizes robust, centralized GitHub Actions pipelines leveraging GitHub Environments and manual approval gates.

### 4.1 Application CI Pipeline (`Central .github / _ci-reusable.yml`)
Triggers on microservice push to `Cloud-Track-dev`.
1. **Quality Check**: Runs **SonarQube** code analysis.
2. **SCA Check**: Runs **Snyk** Security Scan & Monitor on dependencies.
3. **Docker Build**: Fetches `ENTRA_CLIENT_ID` from the Helm dev values and builds the container image.
4. **Vulnerability Scan**: Runs **Trivy** vulnerability scanner on the built image (breaks on CRITICAL/HIGH).
5. **ACR Push**: Authenticates to Azure via OIDC and pushes the image.
6. **GitOps CD**: Automatically updates the specific microservice `image.tag` inside `Helm/values-dev.yaml` and commits back to `FlowForge-Helm` repository, triggering ArgoCD.
7. **Email Notification**: Uses `dawidd6/action-send-mail` to email the development team with CI results and commit details.

### 4.2 Application Prod Release Pipeline (`prod-release.yml`)
Triggers on published GitHub Release tags (e.g., `<service>-v1.0.0`). Retrieves dev images from ACR, retags for prod, updates `values-prod.yaml` on `main`, and triggers the Prod ArgoCD application.

### 4.3 Infrastructure GitOps Pipeline (`terraform-gitops.yml`)
Triggers on IaC pushes to `Cloud-Track-dev` (Dev) and `main` (Prod).
1. Runs `terraform fmt` and `terraform init`.
2. Generates `terraform plan`.
3. Runs **Checkov** to validate IaC security compliance.
4. Runs **Infracost** to estimate cloud bill changes.
5. Sends an Approval Notification Email summarizing costs and security.
6. Hits the **GitHub Environment Gate** (`dev-create` or `prod-create`), requiring manual Administrator approval.
7. Executes `terraform apply -auto-approve`.

*(Note: There is also a manually dispatched `terraform-destroy.yml` pipeline that features double-safety gating by requiring manual text input matching the environment name).*

### 4.4 Key Vault Secrets Sync Pipeline (`update-keyvault-secrets.yml`)
Manually dispatched from the Terraform repository.
- **Runs on Self-Hosted ARC Runner** inside the AKS cluster.
- Uses `mi-arc-runner` Workload Identity to securely authenticate.
- Reads GitHub Repository Secrets (e.g., `REDIS_URL`, `JWT_SECRET`, `ENTRA_CLIENT_SECRET`, `SMTP_PASSWORD`).
- Inserts/updates these secrets directly into the private Azure Key Vault.
- Automatically disables previous versions of the secrets to maintain hygiene.

---

## 5. End-to-End Setup & Deployment Runbook

### Step 1: Azure Bootstrapping
1. Run `scripts/bootstrap_backend.ps1` to create the remote state storage account.
2. Create an App Registration in Microsoft Entra ID.
3. Establish Federated Credentials (OIDC) mapped to GitHub Environments: `dev-create`, `dev-destroy`, `prod-create`, `prod-destroy`.
4. Grant the App Registration `Contributor` and `Role Based Access Control Administrator` on the target deployment Resource Group.

### Step 2: GitHub Secrets Configuration
Populate Repository & Environment Secrets:
- `AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, `AZURE_SUBSCRIPTION_ID`
- `POSTGRES_ADMIN_USERNAME`, `POSTGRES_ADMIN_PASSWORD`, `JUMPBOX_ADMIN_PASSWORD`
- `MAIL_USERNAME`, `MAIL_PASSWORD`, `DEVELOPMENT_TEAM_EMAIL`
- `SNYK_TOKEN`, `SONAR_TOKEN`, `INFRACOST_API_KEY`
- All application secrets (JWT, Redis URL, Entra Secrets, Azure Foundry API Keys) intended for the ARC Secret Sync.

### Step 3: Infrastructure Provisioning
1. Push to `Cloud-Track-dev` in the `FlowForge-Terraform` repo.
2. Review Checkov/Infracost outputs via the notification email.
3. Approve the `dev-create` deployment in GitHub Environments to trigger `apply`.

### Step 4: ARC Setup & Secret Sync
1. Apply the Helm `infra/` ARC runner configurations to deploy the runner pod.
2. Trigger the `update-keyvault-secrets.yml` pipeline manually to seed the Azure Key Vault from GitHub.

### Step 5: Database Permissions Setup
1. Execute `db-permission/run-setup.ps1` to dynamically construct the Kubernetes Job using an Entra ID OAuth token.
2. Apply the compiled Job manifest to grant `mi-flowforge-app` passwordless access to PostgreSQL.

### Step 6: Application Deployment
1. Ensure all microservices have pushed images to ACR via their Dev CI pipelines.
2. `values-dev.yaml` will be auto-updated.
3. Deploy the `argocd/argocd-dev-app.yaml` manifest. ArgoCD will synchronize the cluster with the Git repository and roll out the entire FlowForge suite.
