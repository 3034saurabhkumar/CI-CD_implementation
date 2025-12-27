# CI/CD Pipeline – Spring Boot Application 🚀

This repository demonstrates an **end-to-end CI/CD pipeline** using **Jenkins, Docker, GitHub, Kubernetes and Argo CD (Operator-based)**.

---
### 🔖 Key Concepts
`CI/CD` · `GitOps` · `Release Management` · `Docker Image Versioning` . `Kubernetes Deployments` · `Argo CD Sync` · `Jenkins Pipelines`


## 🛠️ Tech Stack & Tools

<p align="left">
  <img src="https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white"/>
  <img src="https://img.shields.io/badge/Spring%20Boot-6DB33F?style=for-the-badge&logo=springboot&logoColor=white"/>
  <img src="https://img.shields.io/badge/Maven-C71A36?style=for-the-badge&logo=apachemaven&logoColor=white"/>
  <img src="https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white"/>
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white"/>
  <img src="https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white"/>
  <img src="https://img.shields.io/badge/Argo%20CD-EF7B4D?style=for-the-badge&logo=argo&logoColor=white"/>
  <img src="https://img.shields.io/badge/GitHub-Actions?style=for-the-badge&logo=github&logoColor=white"/>
</p>

- **Source Control**: GitHub  
- **CI Tool**: Jenkins (Declarative Pipeline)
- **Build Tool**: Maven  
- **Containerization**: Docker  
- **CD Tool**: Argo CD (Installed via Operator & Controllers)
- **Deployment**: Kubernetes

---

## 📁 Repositories Used

| Repository | Purpose | URL |
|----------|--------|-----|
| `myFirstProject` | Spring Boot application source code | https://github.com/3034saurabhkumar/myFirstProject |
| `CI-CD_implementation` | Kubernetes manifests (`deployment.yml`, `service.yml`) | https://github.com/3034saurabhkumar/CI-CD_implementation |


---

## 🔁 CI/CD Workflow Overview
```
Developer Commit
      ↓
GitHub (Release Branch)
      ↓
Jenkins Pipeline
      ↓
Maven Build
      ↓
Docker Image Build & Push
      ↓
Update Deployment Repo
      ↓
Argo CD Sync
      ↓
Kubernetes Deployment
```


---

## 🧩 Jenkins Pipeline Stages

### 1️⃣ Prepare Workspace
- Cleans old workspace
- Fresh Git checkout

### 2️⃣ Create Release Branch
- Creates `release-<version>` branch
- Pushes to GitHub using credentials

### 3️⃣ Build & Test (Dockerized Maven)
- Runs `mvn clean package`
- Generates JAR
- Stashes build artifact

### 4️⃣ Docker Build & Push
- Builds Docker image using JAR
- Pushes image to Docker Hub
- Image tag = release version

### 5️⃣ Update Deployment Repo
- Clones `CI-CD_implementation`
- Updates Docker image tag in `deployment.yml`
- Commits & pushes changes to `main`

### 6️⃣ Argo CD Deployment
- Argo CD detects Git change
- Automatically syncs to Kubernetes cluster

---

## 🔐 Jenkins Credentials

| Credential ID | Type | Usage |
|--------------|------|-------|
| `github-release-creds` | Username + Token | GitHub push & branch creation |
| `docker-cred` | Docker Registry | Docker image push |

---

## 🐳 Docker Image Naming Convention
`saurabh3034/spring-voldemort:<release-version>`

**Example**
`saurabh3034/spring-voldemort:1.0.1`


---

## 🚀 Argo CD Setup

- Installed using **Argo CD Operator**
- CRDs and Controllers installed first
- Application configuration:
  - **Repository**: `CI-CD_implementation`
  - **Branch**: `main`
  - **Path**: `/`
- Sync Policy: **Automated**

---

## 🏗️ Architecture Overview

### High-Level CI/CD + GitOps Architecture
```
┌────────────┐
│ Developer  │
│ Git Push   │
└─────┬──────┘
      │
      ▼
┌────────────────────────┐
│ GitHub – App Repo      │
│ myFirstProject         │
│ (release-x.y.z branch) │
└─────┬──────────────────┘
      │
      ▼
┌────────────────────────┐
│ Jenkins CI             │
│------------------------│
│ • Maven Build          │
│ • Docker Image Build   │
│ • Push to Docker Hub   │
│ • Update Deploy Repo   │
└─────┬──────────────────┘
      │
      ▼
┌────────────────────────┐
│ GitHub – Deploy Repo   │
│ CI-CD_implementation   │
│ (deployment.yml)       │
└─────┬──────────────────┘
      │
      ▼
┌────────────────────────┐
│ Argo CD (Operator)     │
│ GitOps Sync Controller │
└─────┬──────────────────┘
      │
      ▼
┌────────────────────────┐
│ Kubernetes Cluster │
│ Spring Boot App │
└────────────────────────┘
```

---

## 🧪 Jenkins Pipeline Explanation

### ❓ How does your CI/CD pipeline work?

> The pipeline follows a **release-based CI model** with **GitOps-driven CD**, ensuring traceability, versioning, and automated Kubernetes deployments.

---

### 🔹 Stage 1: Prepare Workspace
- Cleans Jenkins workspace
- Performs fresh SCM checkout

**Why?**
- Prevents stale artifacts
- Ensures reproducible builds

---

### 🔹 Stage 2: Create Release Branch
- Jenkins creates `release-<version>` branch
- Pushes branch to GitHub using stored credentials

**Why?**
- Each release is immutable and traceable
- Enables safe rollback

---

### 🔹 Stage 3: Build & Test (Dockerized Maven)
- Uses official `maven` Docker image
- Builds Spring Boot JAR
- Stashes artifact for later stages

**Why?**
- Tool isolation
- No dependency on Jenkins host setup

---

### 🔹 Stage 4: Docker Image Build & Push
- Unstashes JAR
- Builds Docker image
- Tags image with release version
- Pushes image to Docker Hub

**Why?**
- Immutable versioned images
- Environment consistency

---

### 🔹 Stage 5: Update Deployment Repository
- Jenkins updates `deployment.yml`
- Replaces image tag with release version
- Commits changes to deployment repository

**Why?**
- Git becomes the single source of truth
- Enables GitOps

---

### 🔹 Stage 6: Continuous Deployment with Argo CD
- Argo CD watches deployment repo
- Detects Git changes
- Syncs Kubernetes automatically

**Why?**
- No direct `kubectl apply`
- Safe, auditable deployments

---

## 🔁 Release Flow Documentation

### Example Release Version: `1.0.1`

---

### 1️⃣ Trigger Jenkins Pipeline
- Manual trigger or Git push
- Provide parameter:
`RELEASE_VERSION=1.0.1`

---

### 2️⃣ Jenkins CI Actions
- Creates branch: `release-1.0.1`
- Builds JAR
- Builds Docker image:
`saurabh3034/spring-voldemort:1.0.1`
- Pushes image to Docker Hub

---

### 3️⃣ Deployment Repository Update

**Before:**
```image: saurabh3034/spring-voldemort:replaceImageTag```
**After:**
```image: saurabh3034/spring-voldemort:1.0.1```
**Committed to:**
`CI-CD_implementation/main`

### 4️⃣ Argo CD Sync

- Detects Git commit
- Syncs Kubernetes manifests
- Performs rolling update

### 5️⃣ Kubernetes Deployment

- Old pods terminated
- New pods created
- Zero or minimal downtime

## 🔙 Rollback Strategy
✅ **Git Revert (Recommended):**
```git revert <commit-id>```
- Argo CD automatically rolls back the deployment.

✅ **Image Rollback:**
- Change image tag to previous version and commit.

---
## 🔐 Security & Best Practices

- Jenkins credentials store for secrets
- No hardcoded tokens
- Dockerized build environments
- GitOps-based deployment control
- Immutable artifacts

🧠 One-Line Summary

> “I implemented a Jenkins-based CI pipeline with GitOps-driven CD using Argo CD, enabling secure, versioned, and fully automated Kubernetes deployments.”



## ✅ Outcome

- Every release triggers:
  - Create Release branch
  - New Docker image build
  - Push Docker image on DockerHub
  - Updated Kubernetes manifests
  - Automatic deployment via Argo CD

---

## 🧠 Key Concepts Covered

- Branch Strategy
- Artifact sharing in Jenkins (`stash / unstash`)
- Secure GitHub authentication
- Multi-repository CI/CD
- GitOps using Argo CD
- Operator-based Argo CD installation

---

## 📌 Future Improvements

- Helm charts
- Image scanning (Trivy)
- Manual approval gates
- Slack notifications
- Ingress with TLS

---


