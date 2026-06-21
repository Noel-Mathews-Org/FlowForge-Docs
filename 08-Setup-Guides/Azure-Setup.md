# Azure Setup

[← Back to Main Index](../README.md)

This guide walks a new engineer through the prerequisite manual setup required in Azure Entra ID before the Terraform deployment can be executed.

## 1. Create Entra Groups
You must create Entra ID Security Groups to map to the internal RBAC roles.
- Create group `FlowForge-Platform-Admins`
- Create group `FlowForge-Org-Owners`
- Create group `FlowForge-Managers`
- Create group `FlowForge-Members`
*Capture the Object IDs for these groups. They will be passed as Terraform variables.*

## 2. Create App Registrations
Create an App Registration for the Frontend application.
- **Name**: `FlowForge-Frontend`
- **Supported Account Types**: Single Tenant

## 3. Configure Redirect URIs
In the App Registration -> Authentication:
- Add a platform: **Single-page application (SPA)**
- Add Redirect URIs for your environments:
  - `http://localhost:3000/api/auth/callback/azure-ad`
  - `https://dev.flowforge.yourdomain.com/api/auth/callback/azure-ad`
  - `https://flowforge.yourdomain.com/api/auth/callback/azure-ad`

## 4. Configure Group Claims
In the App Registration -> Token Configuration:
- Click **Add groups claim**.
- Select **Security groups**.
- Choose to emit group IDs. This allows the API Gateway to map the user's groups to their internal role.

## 5. Configure Managed Identities
Create the required User-Assigned Managed Identities manually if they are not managed entirely by the Terraform root module (though Terraform typically provisions these). Ensure you have `mi-github-actions-prod`.

## 6. Configure Federated Credentials
To allow GitHub Actions to deploy to Azure without secrets:
1. Navigate to the `mi-github-actions-prod` Managed Identity.
2. Go to **Federated credentials** -> **Add credential**.
3. Scenario: **GitHub Actions deploying Azure resources**.
4. Organization: `Noel-Mathews-Org`
5. Repository: `FlowForge`
6. Entity Type: **Branch**
7. Based on Branch: `main` (and repeat for `Cloud-Track-dev`).
