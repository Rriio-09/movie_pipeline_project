# 🎬 Movie Picture Pipeline

A full-stack Movie Picture Pipeline application deployed on AWS using Docker, Kubernetes (EKS), Amazon ECR, Terraform, React, Python/Flask, and GitHub Actions CI/CD.

The project implements automated Continuous Integration and Continuous Deployment pipelines for both the frontend and backend applications.

---

## 🔗 Project Links

### GitHub Repository

https://github.com/Rriio-09/movie_pipeline_project.git

### Frontend Application

http://a2ec729a09bf64209946420746744b01-458530145.us-east-1.elb.amazonaws.com

### Backend Movies API

http://a7d41dd03ba43462e847f16f29e56a5a-949129658.us-east-1.elb.amazonaws.com/movies

---

## 🛠️ Technologies Used

### Frontend

- React
- JavaScript
- Node.js
- npm
- Docker

### Backend

- Python
- Flask
- uWSGI
- Pipenv
- Docker

### DevOps / Cloud

- AWS
- Amazon EKS
- Amazon ECR
- Elastic Load Balancer
- Kubernetes
- Terraform
- Docker
- GitHub Actions
- CI/CD

---

## 📁 Project Structure

```text
movie_pipeline_project/
│
├── .github/
│   └── workflows/
│       ├── frontend-ci.yaml
│       ├── backend-ci.yaml
│       ├── frontend-cd.yaml
│       └── backend-cd.yaml
│
├── starter/
│   ├── frontend/
│   │   ├── Dockerfile
│   │   ├── package.json
│   │   └── src/
│   │
│   └── backend/
│       ├── Dockerfile
│       ├── Pipfile
│       ├── Pipfile.lock
│       └── ...
│
├── setup/
│   ├── terraform/
│   ├── init.sh
│   └── workspace-setup.sh
│
└── README.md
```

---

# 🚀 CI/CD Workflows

The repository contains four GitHub Actions workflows.

## 1. Frontend Continuous Integration

```text
.github/workflows/frontend-ci.yaml
```

The frontend CI workflow performs tasks such as:

- Installing frontend dependencies
- Linting frontend code
- Running frontend tests
- Validating the frontend application

---

## 2. Backend Continuous Integration

```text
.github/workflows/backend-ci.yaml
```

The backend CI workflow performs:

- Python dependency installation
- Backend linting
- Backend tests
- Docker image build validation

---

## 3. Backend Continuous Deployment

```text
.github/workflows/backend-cd.yaml
```

The backend CD pipeline:

1. Checks out the repository
2. Configures AWS credentials
3. Logs in to Amazon ECR
4. Builds the backend Docker image
5. Tags the image using the Git commit SHA
6. Pushes the image to Amazon ECR
7. Configures access to the EKS cluster
8. Deploys the backend to Kubernetes
9. Exposes the backend using an AWS LoadBalancer

The backend is deployed at:

```text
http://a7d41dd03ba43462e847f16f29e56a5a-949129658.us-east-1.elb.amazonaws.com
```

Movies endpoint:

```text
http://a7d41dd03ba43462e847f16f29e56a5a-949129658.us-east-1.elb.amazonaws.com/movies
```

---

## 4. Frontend Continuous Deployment

```text
.github/workflows/frontend-cd.yaml
```

The frontend CD pipeline:

1. Checks out the repository
2. Validates the backend API URL
3. Configures AWS credentials
4. Logs in to Amazon ECR
5. Builds the React Docker image
6. Injects the backend API URL during the Docker build
7. Tags the image using the Git commit SHA
8. Pushes the frontend image to ECR
9. Deploys the frontend to EKS
10. Exposes the frontend through an AWS LoadBalancer

The deployed frontend is available at:

```text
http://a2ec729a09bf64209946420746744b01-458530145.us-east-1.elb.amazonaws.com
```

---

# ☁️ AWS Infrastructure

Terraform is used to provision the required AWS infrastructure.

The infrastructure includes:

- Amazon EKS cluster
- Amazon ECR frontend repository
- Amazon ECR backend repository
- IAM configuration
- Networking resources
- Kubernetes deployment infrastructure

AWS Region:

```text
us-east-1
```

EKS Cluster:

```text
cluster
```

ECR repositories:

```text
frontend
backend
```

---

# ⚙️ Infrastructure Setup

Navigate to the Terraform directory:

```bash
cd setup/terraform
```

Initialize Terraform:

```bash
terraform init
```

Review the infrastructure:

```bash
terraform plan
```

Create the infrastructure:

```bash
terraform apply
```

View Terraform outputs:

```bash
terraform output
```

---

# ☸️ Configure Kubernetes

Configure `kubectl` to communicate with the EKS cluster:

```bash
aws eks update-kubeconfig --name cluster --region us-east-1
```

Verify the connection:

```bash
kubectl get nodes
```

Run the initialization script if required:

```bash
cd setup
./init.sh
```

---

# 🔐 GitHub Actions Secrets

The following repository secrets must be configured under:

```text
GitHub Repository
→ Settings
→ Secrets and variables
→ Actions
```

Required secrets:

| Secret | Description |
| --- | --- |
| `AWS_ACCESS_KEY_ID` | AWS access key used by GitHub Actions |
| `AWS_SECRET_ACCESS_KEY` | AWS secret access key used by GitHub Actions |
| `REACT_APP_MOVIE_API_URL` | Public backend base URL |

The backend URL configured for the frontend is:

```text
http://a7d41dd03ba43462e847f16f29e56a5a-949129658.us-east-1.elb.amazonaws.com
```

> `/movies` is not included in the `REACT_APP_MOVIE_API_URL` secret because the frontend application uses the required API endpoint separately.

AWS credentials must never be committed directly to the repository.

---

# 🖥️ Backend Verification

Check the backend Kubernetes resources:

```bash
kubectl get pods
```

Check the backend service:

```bash
kubectl get service backend
```

Check backend logs:

```bash
kubectl logs deployment/backend --tail=100
```

Test the deployed API:

```bash
curl http://a7d41dd03ba43462e847f16f29e56a5a-949129658.us-east-1.elb.amazonaws.com/movies
```

A successful request should return the movie data in JSON format.

---

# 🌐 Frontend Verification

Check the frontend service:

```bash
kubectl get service frontend
```

Get the frontend LoadBalancer hostname:

```bash
kubectl get service frontend -o jsonpath="{.status.loadBalancer.ingress[0].hostname}"
```

Open the frontend application in a browser:

```text
http://a2ec729a09bf64209946420746744b01-458530145.us-east-1.elb.amazonaws.com
```

The frontend communicates with the deployed backend API and displays the movie information.

---

# ☸️ Kubernetes Verification

View all deployed resources:

```bash
kubectl get all
```

Check deployments:

```bash
kubectl get deployments
```

Check pods:

```bash
kubectl get pods
```

Check services:

```bash
kubectl get services
```

The expected application architecture is:

```text
                    GitHub
                       │
                       ▼
                GitHub Actions
                       │
              ┌────────┴────────┐
              │                 │
              ▼                 ▼
        Frontend Image     Backend Image
              │                 │
              ▼                 ▼
         Amazon ECR         Amazon ECR
              │                 │
              └────────┬────────┘
                       │
                       ▼
                  Amazon EKS
                       │
             ┌─────────┴─────────┐
             │                   │
             ▼                   ▼
       Frontend Pod          Backend Pod
             │                   │
             ▼                   ▼
       LoadBalancer          LoadBalancer
             │                   │
             ▼                   ▼
        React App          /movies API
```

---

# ✅ Workflow Verification

Successful GitHub Actions runs should exist for all four workflows:

1. Frontend Continuous Integration
2. Backend Continuous Integration
3. Frontend Continuous Deployment
4. Backend Continuous Deployment

The CI workflows validate application changes.

The CD workflows build Docker images, push them to Amazon ECR, and deploy the applications to Amazon EKS.

---

# 📸 Submission Evidence

Before destroying the AWS infrastructure, capture screenshots of:

### 1. Frontend Application

Open:

```text
http://a2ec729a09bf64209946420746744b01-458530145.us-east-1.elb.amazonaws.com
```

The browser should show the running frontend application and the URL should be visible.

### 2. Backend Movies API

Open:

```text
http://a7d41dd03ba43462e847f16f29e56a5a-949129658.us-east-1.elb.amazonaws.com/movies
```

The endpoint should return the movie data.

### 3. Kubernetes Resources

Run:

```bash
kubectl get all
```

Capture the terminal output.

### 4. GitHub Actions

Capture successful workflow runs for:

```text
Frontend Continuous Integration
Backend Continuous Integration
Frontend Continuous Deployment
Backend Continuous Deployment
```

---

# 🧹 Destroy AWS Infrastructure

After all required screenshots and submission evidence have been collected, destroy the AWS resources to avoid unnecessary AWS charges or credit usage.

Navigate to:

```bash
cd setup/terraform
```

Run:

```bash
terraform destroy
```

Review the resources and confirm the destruction when prompted.

---

# 📋 Final Deployment Information

| Component | Details |
| --- | --- |
| Project | Movie Picture Pipeline |
| Repository | https://github.com/Rriio-09/movie_pipeline_project.git |
| Frontend | http://a2ec729a09bf64209946420746744b01-458530145.us-east-1.elb.amazonaws.com |
| Backend Movies API | http://a7d41dd03ba43462e847f16f29e56a5a-949129658.us-east-1.elb.amazonaws.com/movies |
| AWS Region | `us-east-1` |
| EKS Cluster | `cluster` |
| Frontend ECR Repository | `frontend` |
| Backend ECR Repository | `backend` |

---

## 🎉 Project Status

- Frontend CI — ✅
- Backend CI — ✅
- Frontend CD — ✅
- Backend CD — ✅
- Frontend deployed to Amazon EKS — ✅
- Backend deployed to Amazon EKS — ✅
- Docker images stored in Amazon ECR — ✅
- AWS infrastructure managed with Terraform — ✅
