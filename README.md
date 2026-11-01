# Kubernetes GitOps Configuration

Declarative Kubernetes deployment configuration using Helm and ArgoCD.

## Project Overview

This repository manages three microservices across Development and Production environments using GitOps principles.

### Microservices

- Catalog Service
- Orders Service
- Payments Service

Each service has an independent Helm chart containing:

- Deployment
- ClusterIP Service
- Ingress
- ConfigMap
- Kubernetes Secret
- Horizontal Pod Autoscaler (HPA)

## Repository Structure

```text
k8s-gitops-config/
├── apps/
│   ├── catalog/
│   ├── orders/
│   └── payments/
├── argocd/
│   ├── applications/
│   ├── projects/
│   ├── root-app.yaml
│   └── kustomization.yaml
├── environments/
│   ├── dev/
│   └── prod/
├── monitoring/
│   ├── prometheus/
│   ├── grafana/
│   └── alertmanager/
└── rbac/
```

## GitOps Workflow

1. Developer pushes configuration changes to GitHub.
2. ArgoCD monitors this repository.
3. ArgoCD detects configuration changes.
4. ArgoCD synchronizes the desired state with Kubernetes.
5. Automated sync, self-healing and pruning maintain the declared state.

## Environments

| Environment | Namespace | Replicas |
|---|---|---:|
| Development | staging | 2 |
| Production | production | 3 |

HPA is configured with a minimum of 2 and maximum of 8 replicas, targeting CPU and memory utilization.

## Monitoring

The monitoring configuration uses:

- Prometheus for metrics collection
- Grafana for dashboards
- Alertmanager for notifications
- Node Exporter for node metrics
- kube-state-metrics for Kubernetes resource metrics

## Access Control

Kubernetes RBAC provides:

- Limited developer access in staging
- Read-only access in production

## Validation

Helm chart linting and template rendering were completed for all three services in both environments.

```bash
helm lint apps/catalog
helm lint apps/orders
helm lint apps/payments

helm template catalog-dev apps/catalog -f apps/catalog/values-dev.yaml
```

## Security Notes

- Secret values in this repository are placeholders only.
- Never commit actual API keys or Slack webhook URLs.
- Use AWS Secrets Manager or External Secrets for production secrets.
- Restrict Kubernetes API access and configure appropriate RBAC identities before deployment.

## Deployment Status

This repository contains declarative deployment configuration. It has not been deployed to a live EKS cluster as part of this code-only submission.

## Related Repositories

- [Infrastructure Platform](https://github.com/purnima202/k8s-gitops-platform)
- [Ansible Configuration](https://github.com/purnima202/k8s-ansible-config)
