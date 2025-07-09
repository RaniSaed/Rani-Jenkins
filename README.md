# 🚀 Jenkins CI/CD Pipeline with Disaster Recovery – Smart Retail Inventory System

This pipeline is a **complete CI/CD automation** solution for the Smart Retail Inventory System's **backend**, including:

- 🔄 Build & Deployment automation
- ✅ Health checks
- 🌐 Kubernetes image update
- ⚠️ DR Failover handling
- 📩 Slack notifications
- 📊 DR Readiness report

---

## 📌 Pipeline Overview

### 1️⃣ Clone Repositories
- Clones both:
  - `smart-retail-dev` – App source code
  - `smart-retail-config` – Kubernetes configs

### 2️⃣ Detect Backend Changes
- Detects changes in `backend/` directory.
- **Aborts the pipeline** if no relevant changes are found.

### 3️⃣ Build & Push Docker Image
- Builds a Docker image with the new backend version.
- Tags image using the build number (e.g., `build-15`).
- Pushes to Docker Hub (`rani19/backend:build-15` + `latest`).

### 4️⃣ Update Kubernetes Deployment YAML
- Replaces the image name in `k8s/backend/deployment.yaml` with the new version.
- Commits & pushes the updated YAML to GitHub.

### 5️⃣ Health Check
- Sends up to 15 curl requests to `/health` endpoint on port `5000`.
- If healthy → proceeds. Otherwise → pipeline fails.

---

## 🌐 Disaster Recovery Automation

### 🔁 DR Failover & Data Sync
- If primary frontend/backend containers are not running:
  - Starts DR containers (`gogo-dr-*`)
  - Runs `seed.py` to sync data from live Kubernetes backend

### 🔬 Verifies:
- DR Frontend health on port `3002`
- DR Backend health on port `5002`
- Records DR status to environment variables

---

## 📄 DR Readiness Report

- Generates a `dr-readiness-report.txt` with:
  - Build number, timestamp, test result
  - Frontend & backend DR statuses
  - Data sync status

- Archives report as artifact in Jenkins

---

## 🔔 Slack Notifications

Sends status to Slack using webhook at the end of pipeline:

| Status     | Message |
|------------|---------|
| ✅ Success | CI/CD complete + healthy deployment |
| ⚠️ Aborted | No backend changes detected |
| ❌ Failure | Pipeline failed or DR test failed |

---

## 📌 Summary

This Jenkinsfile is:
- 🔁 Fully automated CI/CD pipeline for production-grade reliability
- 🧠 Smart with change detection and dynamic decisions
- 🛡️ DR-resilient with failover & syncing logic
- 📩 Communicative with real-time Slack alerts and logs

Perfectly suited for **real-world DevOps**, ensuring backend deployments are safe, monitored, and self-healing.

---

> 👨‍💻 Author: [Rani Saed](mailto:rani.saed19@gmail.com)  
> 🐳 Docker Hub: `rani19/backend`  
> 🔗 GitHub Repos: `smart-retail-dev`, `smart-retail-config`

