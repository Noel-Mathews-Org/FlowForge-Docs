# Terraform Pipelines

[← Back to CI/CD Architecture](Overview.md)

The Terraform deployment is fully automated via GitHub Actions in the `FlowForge-Terraform` repository.

## `terraform-gitops.yml`
Runs on pushes to `Cloud-Track-dev` (targeting dev) and `main` (targeting prod).

1. **Continuous Integration (CI)**: Triggers on PR or push. Runs `terraform fmt`, `terraform validate`, and `terraform plan`. Outputs the plan as an artifact. Uses Checkov for security scanning and Infracost for pricing estimation.
2. **Continuous Deployment (CD)**: Triggers on push. Downloads the plan artifact and runs `terraform apply -auto-approve tfplan`.

## `terraform-destroy.yml`
Provides a mechanism for manual teardown of environments via `workflow_dispatch`. Requires explicit confirmation, and utilizes GitHub Environment protection rules as an approval gate.
