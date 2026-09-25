# Fully Automated CI/CD Pipeline for Node.js using GitLab CI & Docker

This project demonstrates a production-ready CI/CD pipeline built with **GitLab CI/CD**. It automates the testing, containerization, and deployment of a Node.js web application.

## 🏗 Architecture & Tech Stack
- **Version Control & CI/CD:** GitLab CI
- **Containerization:** Docker (with Alpine Linux for reduced attack surface)
- **Container Registry:** Docker Hub
- **Deployment Strategy:** SSH-based Dry-Run Deployment (Foundation for Blue/Green)

## 🚀 Pipeline Stages

1. **Test (`test`):** 
   Runs inside a clean `node:18-alpine` container. Installs dependencies and runs automated unit tests to ensure code integrity before building.
2. **Build & Push (`build`):** 
   Utilizes Docker-in-Docker (DinD). Builds the Docker image based on the optimized `Dockerfile` and securely pushes it to Docker Hub using masked CI/CD variables.
3. **Deploy (`deploy`):** 
   Triggered *only* on the `main` branch. Simulates a secure SSH connection to a production server to pull the latest image and restart the container.

## 🔐 Security & Best Practices Implemented

- **Secret Management:** No hardcoded credentials. Docker Hub passwords, Server IPs, and SSH Private Keys are injected securely via GitLab CI/CD Variables (`$DOCKER_PASSWORD`, `$SSH_PRIVATE_KEY`).
- **Secure SSH Connection:** The pipeline configures the `ssh-agent`, handles Windows carriage return (`\r`) formatting issues with `tr -d`, and utilizes `ssh-keyscan` to automatically verify the host and prevent manual prompt blocks.
- **Docker Layer Caching:** The `Dockerfile` is structured to copy `package.json` and run `npm install` *before* copying the rest of the source code. This leverages Docker's layer caching to drastically reduce build times when only application code changes.
- **Lightweight Environments:** Used `alpine` base images across the pipeline to ensure fast execution and enhanced security.
