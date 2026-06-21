# Networking & DNS

[← Back to Kubernetes Architecture](Overview.md)

## Internal Service Communication
Inside the AKS cluster, microservices communicate with one another using unencrypted HTTP over native Kubernetes DNS routing.

The endpoints for each internal service are explicitly defined in the global ConfigMap.
Example: `http://auth-service.{{ .Release.Namespace }}.svc.cluster.local:80`

Because the cluster uses `calico` network policies (defined in Terraform), internal network traffic can be segmented, but all application traffic strictly adheres to the ClusterIP services.

## CNI
The cluster is deployed using the `azure` Container Network Interface (CNI), meaning each pod receives a direct IP address from the Azure Virtual Network (`snet-aks`). This allows the Application Gateway to route directly to Pod IPs.
