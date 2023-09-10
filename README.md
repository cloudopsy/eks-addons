# EKS Addons Repository for ArgoCD

This repository contains Helm charts and configurations for various addons to be deployed on Amazon EKS using ArgoCD's "App of Apps" pattern.

## Directory Structure

- `addons/`: Individual Helm charts for each addon.
    - `external-dns/`: Helm chart for the External DNS.
        - `Chart.yaml`: Metadata and version info for the External DNS chart.
        - `values.yaml`: Default configuration values for deploying External DNS.
    - `karpenter/`: Helm chart for Karpenter, the flexible node autoscaler.
        - `Chart.yaml`: Metadata and version info for the Karpenter chart.
        - `values.yaml`: Default configuration values for deploying Karpenter.
    - `metrics-server/`: Helm chart for the Metrics Server.
        - `Chart.yaml`: Metadata and version info for the Metrics Server chart.
        - `values.yaml`: Default configuration values for deploying Metrics Server.
- `chart/`: Unified Helm chart that can deploy all addons at once. Useful for an "App of Apps" approach.
    - `Chart.yaml`: Metadata and version info for the combined chart.
    - `values.yaml`: Default configuration values for deploying combined addons.
    - `templates/`: Helm templates to generate Kubernetes manifests.
        - `external-dns.yaml`: Helm template for External DNS.
        - `karpenter.yaml`: Helm template for Karpenter.
        - `metrics-server.yaml`: Helm template for Metrics Server.

## Using with ArgoCD "App of Apps"

The idea behind the "App of Apps" pattern is to have a main ArgoCD Application (`master-app`) that manages other ArgoCD Applications (child apps). Each child app can correspond to an addon in this repository.

1. Set up your ArgoCD instance and make sure you have `argocd` CLI tool installed.
2. For each addon, create an ArgoCD Application definition pointing to its location in this repo.
3. Create a `master-app` definition that includes all the child app definitions.
4. Apply the `master-app` definition to your cluster:

```bash
kubectl apply -f master-app.yaml
```