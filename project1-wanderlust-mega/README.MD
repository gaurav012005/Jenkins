

# 🚀 Wanderlust CI Pipeline (Jenkins)

This Jenkins pipeline automates the CI process for the [Wanderlust-Mega-Project](https://github.com/LondheShubham153/Wanderlust-Mega-Project). It includes code checkout, security scans, code quality checks, Docker image builds, and image push to DockerHub.

---

## 🗂️ Pipeline Overview

```groovy
@Library('Shared') _
pipeline {
    agent { label 'Node' }
    ...
}
```

### 🔧 **Tools Used**
- Jenkins Shared Libraries
- SonarQube
- Trivy (Vulnerability Scanner)
- OWASP Dependency-Check
- DockerHub

---

## ⚙️ Parameters

| Parameter              | Description                                      |
|------------------------|--------------------------------------------------|
| `FRONTEND_DOCKER_TAG`  | Docker tag for frontend image (e.g., `v1.0.1`)   |
| `BACKEND_DOCKER_TAG`   | Docker tag for backend image (e.g., `v1.0.1`)    |

---

## 📜 Pipeline Stages

### ✅ Validate Parameters
Ensures `FRONTEND_DOCKER_TAG` and `BACKEND_DOCKER_TAG` are provided to prevent undefined builds.

---

### 🧹 Workspace Cleanup
Cleans up previous builds using:
```groovy
cleanWs()
```

---

### 🔍 Git Checkout
Clones the source code from GitHub:
```groovy
code_checkout("https://github.com/LondheShubham153/Wanderlust-Mega-Project.git", "main")
```

---

### 🔐 Trivy: Filesystem Scan
Scans code for OS-level vulnerabilities.

---

### 🛡️ OWASP Dependency-Check
Scans for vulnerable dependencies in backend/frontend.

---

### 📊 SonarQube Analysis
Performs static code analysis:
```groovy
sonarqube_analysis("Sonar","wanderlust","wanderlust")
```

---

### ✅ SonarQube Quality Gates
Checks if the code passes defined quality standards.

---

### 🌍 Environment Setup
Executes setup scripts from the `Automations` folder:
- `updatebackendnew.sh`
- `updatefrontendnew.sh`

---

### 🐳 Docker: Build Images
Builds Docker images for:
- `wanderlust-backend-beta`
- `wanderlust-frontend-beta`

Tags are defined via input parameters.

---

### ☁️ Docker: Push Images
Pushes both images to DockerHub:
- `trainwithshubham/wanderlust-backend-beta`
- `trainwithshubham/wanderlust-frontend-beta`

---

## 📦 Post Steps

If the build is **successful**:
- Archives `.xml` reports
- Triggers a downstream **CD pipeline** named `Wanderlust-CD`

```groovy
build job: "Wanderlust-CD", parameters: [...]
```

---

## 📌 Notes
- Shared library functions like `code_checkout`, `docker_build`, etc., are abstracted to keep the pipeline DRY and clean.
- This CI pipeline is intended to be used alongside a **CD job** that handles deployment.

---

