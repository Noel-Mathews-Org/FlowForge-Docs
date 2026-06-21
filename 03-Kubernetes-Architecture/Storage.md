# Storage Architecture

[← Back to Kubernetes Architecture](Overview.md)

FlowForge utilizes an externalized storage pattern.

## PersistentVolumeClaims (PVC)
There are **no** `PersistentVolumeClaim` (PVC) resources deployed in the cluster. The AKS nodes do not store any persistent application data on their local disks.

## Azure Blob Storage
File storage operations interface directly with Azure Blob Storage over the network.
- Authentication is determined by environment variables passed from the ConfigMap (`AZURE_STORAGE_ACCOUNT_NAME`, `AZURE_STORAGE_CONTAINER`).
- Security is validated using Azure Workload Identities (`AZURE_STORAGE_USE_MANAGED_IDENTITY: "true"`).
