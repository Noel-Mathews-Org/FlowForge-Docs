# Kubernetes Architecture Overview

[← Back to Main Index](../README.md)

FlowForge is orchestrated on Azure Kubernetes Service (AKS). The application is deployed via a central Helm umbrella chart (`FlowForge-Helm`) which utilizes GitOps principles through ArgoCD.

## Principles

1. **Stateless Workloads**: All deployed applications are stateless `Deployment` resources. There are no `StatefulSets` or `PersistentVolumeClaims`. State is pushed entirely to external PaaS (PostgreSQL, Redis, Blob Storage).
2. **GitOps Driven**: The state of the cluster is defined declaratively in the Helm repository. ArgoCD continuously reconciles the cluster against the `Cloud-Track-dev` and `main` branches.
3. **Secretless Kubernetes**: The cluster relies on Azure Entra Workload Identity. No native Kubernetes `Secret` resources are used to store sensitive application strings.

## Navigation

* [Namespaces](Namespaces.md)
* [Networking & DNS](Networking.md)
* [Ingress](Ingress.md)
* [Secret Management](Secrets.md)
* [ConfigMaps](ConfigMaps.md)
* [Deployments](Deployments.md)
* [Services](Services.md)
* [Autoscaling & PDBs](Autoscaling.md)
* [Storage](Storage.md)
* [Security Contexts](Security.md)
