# Arch Manifest Repository

This repository contains the Kubernetes manifests for the Arch application, managed by ArgoCD for GitOps deployment.

## Structure

```
├── base/
│   └── app/
│       ├── kustomization.yaml      # Base kustomization
│       ├── migration/              # Database migration job
│       ├── rpc-client/            # FastAPI client service
│       └── rpc-server/            # gRPC server service
└── overlays/
    └── dev/
        └── app/
            ├── kustomization.yaml        # Dev environment overlay
            └── migration/
                └── kustomization.yaml    # Dev migration overlay
```

## Image Tags

The image tags are automatically updated by the GitHub Actions workflow in the main `arch` repository:
- **rpc-server**: `az871209/arch:rpc-server-<sha>`
- **rpc-client**: `az871209/arch:rpc-client-<sha>`

## ArgoCD Integration

This repository is monitored by ArgoCD which automatically deploys changes to the Kubernetes cluster:
- **Application**: `arch`
- **Source Path**: `overlays/dev/app`
- **Target Namespace**: `arch`
- **Sync Policy**: Automated with self-heal enabled

## Workflow

1. Code changes are pushed to the main `arch` repository
2. GitHub Actions builds Docker images and pushes to DockerHub
3. GitHub Actions updates the image tags in this repository
4. ArgoCD detects the changes and deploys the updated manifests to Kubernetes

## Manual Deployment

To manually apply the manifests:

```bash
kubectl apply -k overlays/dev/app
```

## Dependencies

The application depends on:
- PostgreSQL (CloudNativePG)
- Redis/Valkey
- Istio service mesh
- OpenTelemetry/Jaeger for tracing