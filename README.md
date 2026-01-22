# hello-world-cpln-crd-chart

A Helm chart containing Control Plane CRD manifests for deploying a hello-world workload.

## Prerequisites

- Kubernetes cluster with the [Control Plane Kubernetes Operator](https://github.com/controlplane-com/k8s-operator) installed
- [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) (optional, for GitOps deployment)
- Operator authenticated to your Control Plane org

## Resources

This chart deploys:

- **GVC**: Global Virtual Cloud
- **Workload**: Hello world serverless workload

## Usage

### With ArgoCD

Create an ArgoCD Application pointing to this chart:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: hello-world
  namespace: argocd
spec:
  project: default
  destination:
    server: https://kubernetes.default.svc
    namespace: hello-world
  source:
    repoURL: https://github.com/MajidAbuRmila/cpln-crd-hello-world-chart.git
    path: .  # Path to the chart in the repo
    targetRevision: main  # Branch, tag, or commit
    helm:
      values: |
        org: your-org-name
        gvc: your-gvc-name
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

### With Helm directly

```bash
helm install hello-world . --set org=your-org-name --set gvc=your-gvc-name
```

## Values

| Key   | Description                     | Default         |
| ----- | ------------------------------- | --------------- |
| `org` | Control Plane organization name | `"epoch"`       |
| `gvc` | Global Virtual Cloud name       | `"hello-world"` |
