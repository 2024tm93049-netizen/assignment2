ACEest Fitness CI/CD Pipeline — Summary
🏋️‍♂️ Project Overview

This repository contains the full DevOps CI/CD implementation for ACEest Fitness & Gym, a simulated fitness startup undergoing digital transformation.
The goal is to automate the build, test, and deployment lifecycle of a Flask-based web application using industry-standard DevOps tools.

🧱 CI/CD Pipeline Architecture
flowchart LR
    A[Developer Pushes Code] --> B[GitHub Repository]
    B --> C[Jenkins CI Server]
    C --> D[Pytest Unit Tests]
    D --> E[SonarQube Quality Analysis]
    E --> F[Docker Build & Push to Docker Hub]
    F --> G[Kubernetes Deployment (Minikube/Cloud)]
    G --> H[User Accesses Application via Service URL]

⚙️ Tools & Technologies Used
Stage	Tool	Purpose
Version Control	Git & GitHub	Code versioning and collaboration
CI/CD	Jenkins	Automates builds, testing, and deployment
Testing	Pytest	Unit testing for Flask endpoints
Code Quality	SonarQube	Static analysis and quality gates
Containerization	Docker	Package and isolate the app environment
Deployment	Kubernetes / Minikube	Container orchestration and scaling
Registry	Docker Hub	Store versioned images for deployments
🚀 Pipeline Stages

Source: Jenkins monitors the GitHub repository for new commits.

Build & Test: Installs dependencies and executes automated Pytest cases.

Code Quality: Runs SonarQube analysis to enforce coding standards.

Containerization: Builds a Docker image and tags it with the build number.

Push to Registry: Pushes the image to Docker Hub.

Deploy: Applies Kubernetes manifests using kubectl with a Rolling Update strategy.

Verification: Performs health checks via /health endpoint.

🐳 Docker Commands
# Build image
docker build -t <dockerhub_user>/aceest:1.0 .

# Run container
docker run -p 5000:5000 <dockerhub_user>/aceest:1.0

☸️ Kubernetes Deployment
# Apply deployment and service
kubectl apply -f k8s/rolling-deployment.yaml
kubectl apply -f k8s/service.yaml

# Verify pods and services
kubectl get pods
kubectl get svc


Deployment strategies included:

✅ Rolling Update (default)

🔵🟢 Blue-Green Deployment

🐤 Canary Release (for limited rollout testing)

🧪 Testing

Run tests locally:

pytest -v --cov=app


All tests run automatically inside Jenkins as part of the pipeline.

📊 Code Quality

SonarQube configuration is included via sonar-project.properties.
Jenkins uses the SonarQube scanner for static code analysis.

🧾 Report and Documentation

📄 Assignment Report (PDF)

📘 Assignment Report (DOCX)

The report includes:

CI/CD architecture overview

Automation workflow explanation

Deployment strategies

Challenges & mitigation strategies

Key outcomes and learnings

👨‍💻 Author

morlitarans-lab
GitHub: https://github.com/morlitarans-lab

Date: November 2025