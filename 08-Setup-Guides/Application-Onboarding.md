# Application Onboarding

[← Back to Main Index](../README.md)

When creating a new microservice for FlowForge, follow these steps:

1. **Source Code**: Create a new directory in the `FlowForge` repository (e.g., `billing-service`).
2. **Pipeline**: Copy an existing `ci-*.yml` file, rename it to `ci-billing-service.yml`, and update the `paths` trigger to watch your new directory.
3. **Helm Chart**: Create a new sub-chart in `FlowForge-Helm/charts/billing-service`.
4. **Umbrella Registration**: Add the new sub-chart as a dependency in `FlowForge-Helm/Chart.yaml`.
5. **Config**: Add the internal service URL to `FlowForge-Helm/templates/global-config.yaml`.
6. **Deploy**: Push to `Cloud-Track-dev`. ArgoCD will detect the new sub-chart and automatically provision it into the cluster.
