# GitOps Progressive Delivery Platform

**Kubernetes · GitHub Actions · Trivy · GHCR · Argo CD · Argo Rollouts**

A GitHub-based Kubernetes deployment and progressive delivery platform that takes code from commit to production through a secure CI pipeline, GitOps-driven deployment, and canary releases.

## Architecture

```text
Developer Push to GitHub
→ GitHub Actions (Build → Trivy Scan → Push to GHCR)
→ Git Repository (Kubernetes manifests)
→ Argo CD (GitOps sync)
→ Kubernetes Cluster
→ Argo Rollouts (Canary Progressive Delivery)
```

## What I Built

- Built a GitHub Actions CI pipeline to build, scan, and push container images.
- Integrated Trivy to scan container images for HIGH and CRITICAL vulnerabilities.
- Published container images to GitHub Container Registry (GHCR).
- Deployed the application to Kubernetes using Argo CD with auto-sync enabled.
- Demonstrated GitOps self-healing by manually changing the replica count and watching the cluster return to the Git-defined state.
- Implemented canary progressive delivery using Argo Rollouts.
- Verified rollout status and revision history for safe rollbacks.

## Delivery Workflow

**BUILD → SCAN → DEPLOY → SELF-HEAL → PROMOTE → VERIFY**

## 1. Container Security Scan

![Trivy Scan](docs/screenshots/1.png)

Trivy scanning the container image for HIGH and CRITICAL vulnerabilities.
The scan reported 2 HIGH findings (`libexpat` and `pcre2`), both with fixed versions available, and 0 CRITICAL findings. Scanning before the push catches known vulnerabilities early.

## 2. CI Pipeline

![GitHub Actions Pipeline](docs/screenshots/2.png)

The GitHub Actions `Build-Test-Scan-Push` workflow completing successfully.
Each push to `master` triggers the build, Trivy scan, GHCR login, and image push steps automatically.

## 3. GitOps Deployment

![Argo CD Application](docs/screenshots/3.png)

The Argo CD application showing a Healthy and Synced state.
Argo CD watches the Git repository and keeps the cluster in sync with the declared manifests, with auto-sync enabled.

## 4. Self-Healing Demonstration

![Self-Healing](docs/screenshots/4.png)

The deployment manually scaled down to 1 replica and restored to 3/3 automatically.
This demonstrates the GitOps principle that Git is the source of truth: any manual drift in the cluster is corrected back to the desired state.

## 5. Canary Progressive Delivery

![Argo Rollouts Canary](docs/screenshots/5.png)

An Argo Rollouts canary deployment completing all steps (5/5) at 100% weight.
The new revision is promoted gradually, and the previous ReplicaSet is scaled down once the new version is stable.

## 6. Rollout Status & Revision History

![Rollout Status and History](docs/screenshots/6.png)

Rollout status verified with the Argo Rollouts kubectl plugin.
The stable ReplicaSet is serving all traffic, while earlier revisions are kept scaled down and available for rollback.

## Key Learning

- GitHub Actions CI pipeline design
- Container image security scanning with Trivy
- Publishing images to GitHub Container Registry
- GitOps with Argo CD (auto-sync and self-healing)
- Canary progressive delivery with Argo Rollouts
- Rollout verification and rollback readiness
- End-to-end delivery: BUILD → SCAN → DEPLOY → SELF-HEAL → PROMOTE → VERIFY
