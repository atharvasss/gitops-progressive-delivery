# GitOps-Based Kubernetes Deployment & Progressive Delivery Platform

GitOps CI/CD platform using GitHub Actions, Docker, Trivy, Helm, Argo CD and Kubernetes.

## Architecture

GitHub → GitHub Actions → Docker → Trivy → GHCR → Argo CD → Kubernetes → Progressive Delivery

## Features

- GitOps deployment
- Automated CI/CD
- Docker image builds
- Trivy security scanning
- GHCR image registry
- Argo CD synchronization
- Self-healing
- Canary deployments
- Rollback
- Kubernetes

## GitOps Flow

1. Developer pushes code
2. GitHub Actions builds image
3. Trivy scans image
4. Image pushed to GHCR
5. Argo CD detects desired state
6. Kubernetes deployment occurs
7. Progressive rollout validates release

## Failure Scenarios

- Manual replica modification
- Bad release
- Rollout abort
- Rollback

## Technologies

Kubernetes, Argo CD, Argo Rollouts, Docker, Helm, GitHub Actions, Trivy
