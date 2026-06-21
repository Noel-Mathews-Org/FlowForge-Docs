# Terraform Deployment

[← Back to Main Index](../README.md)

## Infrastructure as Code Deployment Sequence

1. **Initialization**:
   The state backend is stored in Azure Blob Storage. The backend configuration relies on Entra ID authentication (`use_oidc = true`).
   
2. **Terraform Variables**:
   Update the `.tfvars` files in `env/dev/` and `env/prod/` with the Object IDs from your Entra ID setup:
   - `devops_group_object_id`
   - `devtest_group_object_id`

3. **Deployment**:
   Push changes to the `Cloud-Track-dev` branch. The `terraform-gitops.yml` GitHub Action will automatically:
   - Run `terraform init`
   - Run `terraform plan`
   - Run `terraform apply`

4. **Validation Steps**:
   - Verify the Hub and Spoke VNets exist.
   - Verify the private AKS cluster is in a Succeeded state.
   - Verify the Application Gateway is provisioned and listening on public IPs.
