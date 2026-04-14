# ChatOps AI Platform GitOps Repo

This repository is the source of truth for Kubernetes deployment state.

## Flow

1. App repo builds Docker images
2. Image tag is updated in this GitOps repo
3. Argo CD syncs the cluster
4. Argo Rollouts performs Blue/Green deployment

## Structure

- `apps/chatops/base` → base manifests
- `apps/chatops/overlays/dev` → dev overlay
- `argocd` → Argo CD application
