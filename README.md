# GitOps Progressive Delivery Platform

A GitHub-based Kubernetes deployment platform with a secure CI pipeline, GitOps-driven continuous delivery, and canary progressive delivery.

**Stack:** GitHub Actions · Trivy · GHCR · Argo CD · Argo Rollouts · Kubernetes

---

## Architecture

```
Developer Push → GitHub Actions (Build → Test → Trivy Scan → Push to GHCR)
              → Git manifests updated → Argo CD syncs cluster
              → Argo Rollouts performs canary release
```

---

## 1. Container Security Scan (Trivy)

Every image is scanned for `HIGH` and `CRITICAL` vulnerabilities before it is pushed to the registry.

```bash
trivy image --severity HIGH,CRITICAL gitops-demo:v1
```

![Trivy vulnerability scan](docs/screenshots/1.png)

---

## 2. CI Pipeline (GitHub Actions)

The `Build-Test-Scan-Push` workflow runs on every push to `master`: checkout, build image, Trivy scan, login to GHCR, and push image.

![GitHub Actions CI pipeline](docs/screenshots/2.png)

---

## 3. GitOps Deployment (Argo CD)

Argo CD watches the Git repository and keeps the cluster in sync. The application shows **Healthy** and **Synced** with auto-sync enabled.

![Argo CD application synced and healthy](docs/screenshots/3.png)

---

## 4. Self-Healing Demo

The deployment is manually scaled down to 1 replica. Argo CD detects the drift from Git and restores it to the desired 3 replicas automatically.

```bash
kubectl scale deployment gitops-demo -n gitops-demo --replicas=1
kubectl get deployment -n gitops-demo -w
```

![Self-healing: replicas restored to 3/3](docs/screenshots/4.png)

---

## 5. Canary Progressive Delivery (Argo Rollouts)

The application is deployed as an Argo Rollout using a canary strategy. A new revision is promoted step by step until it reaches 100% traffic, and the old ReplicaSet is scaled down.

![Argo Rollouts canary completed](docs/screenshots/5.png)

---

## 6. Rollout Status and Revision History

Rollout status is verified with the Argo Rollouts kubectl plugin. The revision history shows the stable ReplicaSet and the previous revisions scaled down, which are available for rollback.

```bash
kubectl argo rollouts status gitops-demo -n gitops-demo
kubectl argo rollouts get rollout gitops-demo -n gitops-demo
```

![Rollout status and revision history](docs/screenshots/6.png)

---

## Key Features

- Automated build, scan, and push pipeline with GitHub Actions
- Vulnerability scanning with Trivy (HIGH and CRITICAL)
- Images published to GitHub Container Registry (GHCR)
- GitOps continuous delivery with Argo CD (auto-sync, self-heal)
- Canary releases with Argo Rollouts
- Revision history for fast rollback
