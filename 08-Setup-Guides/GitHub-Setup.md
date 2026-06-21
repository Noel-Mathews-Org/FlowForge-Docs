# GitHub Setup

[← Back to Main Index](../README.md)

This guide outlines the required configuration within the GitHub repository to support the automated CI/CD pipelines.

## Environments
Create the following Environments in the repository settings:
- `dev`
- `prod`

For the `prod` environment, configure **Environment protection rules**:
- Check **Required reviewers** and add the Platform Administrators team. This acts as the manual approval gate for production deployments.

## Secrets
The following Repository / Environment secrets must be configured:

| Secret Name | Purpose | Where it is used |
|-------------|---------|------------------|
| `AZURE_CLIENT_ID` | Client ID of the GitHub Actions Managed Identity | `terraform-gitops.yml`, `prod-release.yml` |
| `AZURE_TENANT_ID` | Your Azure AD Tenant ID | `terraform-gitops.yml`, `prod-release.yml` |
| `AZURE_SUBSCRIPTION_ID` | Your Azure Subscription ID | `terraform-gitops.yml`, `prod-release.yml` |
| `SONAR_TOKEN` | Authentication token for SonarQube | `_ci-reusable.yml` |
| `SNYK_TOKEN` | Authentication token for Snyk | `_ci-reusable.yml` |

## Variables
Configure the following Repository variables:
- `ACR_NAME`: Name of your Azure Container Registry (e.g., `flowforgeacrprod`).
- `TF_STATE_RESOURCE_GROUP`: Name of the resource group holding Terraform state (`Noel-STF`).
- `TF_STATE_STORAGE_ACCOUNT`: Name of the storage account holding Terraform state (`noelstf98`).
- `TF_STATE_CONTAINER`: `statefile`

## Permissions
Ensure the workflows have `id-token: write` and `contents: read` permissions configured at the repository level to enable OIDC federation with Azure.
