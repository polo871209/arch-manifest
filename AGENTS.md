# Agent Guidelines for arch-manifest

## Commands

- **Validate YAML only**: `kubectl apply --dry-run=client -k overlays/dev`
- **Security scan**: `docker run -i kubesec/kubesec:v2 scan /dev/stdin < deployment.yaml`
- **DO NOT APPLY MANUALLY** - ArgoCD handles all deployments

## Repository Structure

This is a Kubernetes GitOps manifest repository using Kustomize, managed by ArgoCD. Files are YAML manifests for:
- `base/app/`: Base Kubernetes resources (deployments, services, configmaps, secrets)
- `overlays/dev/`: Environment-specific overlays with image tags

## Kubernetes Expert Guidelines

You are a Kubernetes expert. Ensure all manifests follow best practices:
- **DRY Principle**: Use Kustomize bases and overlays to avoid duplication
- **Consistency**: Resource names, labels, annotations must be consistent across all manifests
- **Security**: Proper resource limits, security contexts, non-root containers
- **Observability**: Health checks, monitoring labels, proper logging configuration
- **Standards**: Follow Kubernetes API conventions and naming standards

## YAML Guidelines

- Use 2-space indentation consistently
- Follow Kubernetes API conventions for field ordering (apiVersion, kind, metadata, spec)
- Use kebab-case for resource names and labels
- Include standard labels: `app.kubernetes.io/name`, `app.kubernetes.io/component`, `app.kubernetes.io/part-of`, `app.kubernetes.io/managed-by`
- Resources should be namespaced under `arch`
- Image tags are automatically updated by CI/CD - don't manually edit overlays/dev/kustomization.yaml
- **NEVER add labels directly to manifests** - Use Kustomize `labels` section in kustomization.yaml only
- **DO NOT set resource requests** - Only set limits for local development

## Key Principles

- ArgoCD manages all deployments - changes trigger automatic sync
- Image tags follow pattern: `az871209/arch:rpc-{client|server}-<sha>`
- Maintain Kustomize base/overlay structure for environment consistency
- All deployments require health checks, resource limits, and proper configuration management

