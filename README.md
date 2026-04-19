# 🚀 ChatOps AI Platform — GitOps (Deployment & Monitoring)

---

## 🧠 Overview

This repository manages Kubernetes deployments for the ChatOps AI Platform using GitOps.

Instead of manually deploying using kubectl, I used Argo CD to continuously sync this repository with the EKS cluster. So any change here automatically reflects in the cluster.

Along with deployments, this repo also includes rollout strategy and monitoring setup.

---

## 🎯 What this repo manages

- Application deployments (frontend + backend)  
- Kubernetes manifests  
- Argo CD applications  
- Argo Rollouts (deployment strategy)  
- Monitoring stack (Prometheus, Grafana, Alertmanager)  
- Logging setup (ELK stack)  

---

## ⚙️ Tech Stack

- Argo CD (GitOps)  
- Argo Rollouts (Blue/Green deployment)  
- Kubernetes (EKS)  
- Helm (for monitoring setup)  
- Prometheus + Grafana (metrics)  
- ELK Stack (logs)  
- AWS CloudWatch (infra monitoring)  

---

## 🔄 GitOps Workflow

1. Application image is updated (via CI)  
2. GitOps repo is updated with new image tag  
3. Argo CD detects the change  
4. Syncs manifests to Kubernetes  
5. Argo Rollouts handles deployment  
6. Monitoring tools track system behavior  

No manual deployment needed after setup.

---

## 🚀 Deployment Strategy

I used Blue/Green deployment using Argo Rollouts:

- New version is deployed alongside current version  
- Traffic is initially routed to old version  
- After validation, traffic is switched  
- Old version is terminated  

This ensures zero downtime during deployments.

---

## 🛠️ Commands

### 🔹 Argo CD Deployment

```bash
kubectl apply -f argocd/chatops-dev-app.yaml
kubectl get applications -n argocd
kubectl describe application chatops-dev -n argocd
```

---

### 🔹 Application & Rollouts

```bash
kubectl get all -n chatops
kubectl argo rollouts get rollout chatops-frontend -n chatops
kubectl argo rollouts get rollout chatops-frontend -n chatops --watch
```

---

### 🔹 Ingress & Load Balancer

```bash
kubectl get ingress -A
kubectl describe ingress <name> -n <namespace>
```

---

### 🔹 Monitoring

```bash
kubectl get pods -n monitoring
kubectl port-forward svc/<grafana-service> -n monitoring 3000:80
kubectl port-forward svc/<prometheus-service> -n monitoring 9090:9090
```

---

### 🔹 Logs & Debugging

```bash
kubectl logs <pod-name> -n <namespace>
kubectl describe pod <pod-name> -n <namespace>
kubectl logs -n kube-system deployment/aws-load-balancer-controller
```

---

### 🔹 API Testing

```bash
curl -X POST http://<service-ip>:8000/api/chat \
  -H "Content-Type: application/json" \
  -d '{"message":"hello"}'
```

---

## 📊 Monitoring & Observability

### Metrics (Prometheus + Grafana)

- Prometheus collects metrics from cluster and workloads  
- Grafana is used to visualize dashboards  

---

### Logging (ELK Stack)

- Logs are collected from containers  
- Stored and visualized using Elasticsearch and Kibana  

---

### AWS Monitoring (CloudWatch)

- Tracks ALB metrics (latency, errors)  
- Monitors EC2 and EKS nodes  
- Helps in infrastructure-level visibility  

---

## ⚠️ Challenges I faced

### Argo CD sync issues

- Applications were not syncing  
- Root cause → incorrect repo path / manifest issues  
- Fixed by validating paths and YAML  

---

### ALB not getting created

- Ingress was created but no load balancer  

Root cause:
- IAM role (IRSA) misconfiguration  

Fix:
- Corrected OIDC setup and trust policy  

---

### Monitoring metrics missing

- Grafana dashboards showing no data  

Root cause:
- ServiceMonitor selector mismatch  

Fix:
- Updated Prometheus configuration  

---

### Load Balancer Controller issues

- Controller not working initially  

Fix:
- Created proper IAM role  
- Attached correct policies  
- Verified service account  

---

## 🧪 Troubleshooting Commands

```bash
kubectl logs -n kube-system deployment/aws-load-balancer-controller
kubectl describe ingress <name> -n <namespace>
kubectl describe application chatops-dev -n argocd
```

---

## 🎯 Outcome

- Fully automated GitOps-based deployment  
- Blue/Green deployment working  
- ALB-based ingress configured  
- Monitoring and logging fully integrated  
- End-to-end production-style deployment flow 

---

## 👨‍💻

Ram Polarapu
