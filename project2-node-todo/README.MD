

# 🚀 Node Todo App – Jenkins CI/CD Pipeline

This Jenkins pipeline automates the process of building, testing, and deploying a Node.js TODO app using Docker and DockerHub.

---

## 🛠️ Technologies Used

- 🐳 Docker
- 🧪 Jenkins
- ☁️ DockerHub
- 💻 GitHub
- 🔐 Jenkins Credentials (`dockerHubCreds`)

---

## 🗂️ Pipeline Overview

This pipeline performs the following tasks:

1. ✅ Clones code from GitHub  
2. 🏗️ Builds the Docker image  
3. ☁️ Pushes the image to DockerHub  
4. 🚀 Deploys the app using `docker-compose`  

---

## 🔄 Pipeline Stages

### 1. 📥 **Code Clone**
```groovy
git url: "https://github.com/LondheShubham153/node-todo-cicd.git", branch: "master"
```
- Clones the Node.js app from GitHub repository's `master` branch.

---

### 2. 🧪 **Code Build & Test**
```groovy
sh "docker build -t node-app ."
```
- Builds a Docker image for the app and tags it as `node-app`.

---

### 3. ☁️ **Push To DockerHub**
```groovy
withCredentials([usernamePassword(...)]) {
  sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
  sh "docker image tag node-app:latest ${env.dockerHubUser}/node-app:latest"
  sh "docker push ${env.dockerHubUser}/node-app:latest"
}
```
- Securely logs in to DockerHub using Jenkins credentials (`dockerHubCreds`).
- Tags the image with your DockerHub username and pushes it.

---

### 4. 🚀 **Deploy**
```groovy
sh "docker compose down && docker compose up -d --build"
```
- Stops the current deployment (if running).
- Spins up the latest container using `docker-compose` in detached mode.

---

## 🔐 Credentials Setup

To make the pipeline work, add your DockerHub credentials in Jenkins:

1. Go to **Jenkins > Manage Jenkins > Credentials**
2. Add a new **Username with password** credential
3. ID should be set to: `dockerHubCreds`

---

## 💬 Notes

- Make sure your Jenkins agent (`dev-server`) has Docker & Docker Compose installed.
- Keep your DockerHub credentials secure using Jenkins credentials manager.
- The Docker image is pushed to:  
  `docker.io/your-dockerhub-username/node-app:latest`

