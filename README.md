# Movie Picture Pipeline

Movie Picture Pipeline is a full-stack cloud application that demonstrates an automated CI/CD pipeline for a React frontend and Python backend.

The application is containerized using Docker, deployed to Amazon EKS using Kubernetes, stores container images in Amazon ECR, provisions AWS infrastructure using Terraform, and uses GitHub Actions for Continuous Integration and Continuous Deployment.

---

## Project Links

### GitHub Repository

https://github.com/Rriio-09/movie_pipeline_project

### Frontend Application

http://a2ec729a09bf64209946420746744b01-458530145.us-east-1.elb.amazonaws.com

### Backend Movies API

http://a7d41dd03ba43462e847f16f29e56a5a-949129658.us-east-1.elb.amazonaws.com/movies

---

## Project Architecture

The application consists of two independently containerized services:

- **Frontend:** React application
- **Backend:** Python/Flask REST API

Both services are built as Docker images and stored in separate Amazon ECR repositories.

GitHub Actions automatically performs testing, Docker image creation, image publishing, and deployment to Amazon EKS.

The deployed frontend communicates with the backend through the public backend LoadBalancer.

```text
                    GitHub Repository
                           |
                           v
                     GitHub Actions
                    CI             CD
                     |              |
                     v              v
              Tests / Lint     Docker Build
                                    |
                                    v
                               Amazon ECR
                                    |
                                    v
                               Amazon EKS
                         ___________|___________
                        |                       |
                        v                       v
                 Frontend Pod             Backend Pod
                        |                       |
                        v                       v
                  LoadBalancer             LoadBalancer
                        |                       |
                        v                       v
                   React App              /movies API
```

---

## Technologies Used

### Frontend

- React
- JavaScript
- Node.js
- npm
- Docker

### Backend

- Python
- Flask
- Pipenv
- uWSGI
- Docker

### Cloud and DevOps

- Amazon Web Services (AWS)
- Amazon Elastic Kubernetes Service (EKS)
- Amazon Elastic Container Registry (ECR)
- Elastic Load Balancer
- Kubernetes
- Terraform
- Docker
- GitHub Actions
- CI/CD

---

## Repository Structure

```text
movie_pipeline_project/
|
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
│   │   ├── package-lock.json
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

# CI/CD Pipelines

The repository contains four GitHub Actions workflows.

## Frontend Continuous Integration

Workflow:

```text
.github/workflows/frontend-ci.yaml
```

The Frontend CI pipeline validates frontend changes before they are merged.

The workflow performs:

1. Repository checkout
2. Node.js dependency installation
3. Frontend linting
4. Frontend automated tests
5. Frontend Docker image build validation

The workflow runs for frontend changes associated with pull requests to the `main` branch and can also be executed manually.

---

## Backend Continuous Integration

Workflow:

```text
.github/workflows/backend-ci.yaml
```

The Backend CI pipeline validates backend changes.

The workflow performs:

1. Repository checkout
2. Python dependency installation
3. Backend linting
4. Backend automated tests
5. Backend Docker image build validation

The Docker build runs only after the required validation jobs complete successfully.

The workflow runs for backend changes associated with pull requests to the `main` branch and can also be executed manually.

---

## Backend Continuous Deployment

Workflow:

```text
.github/workflows/backend-cd.yaml
```

The Backend CD pipeline deploys the backend application to AWS.

The deployment process includes:

1. Checkout repository
2. Run backend validation
3. Configure AWS credentials
4. Authenticate with Amazon ECR
5. Build the backend Docker image
6. Tag the Docker image using the Git commit SHA
7. Push the image to the backend ECR repository
8. Configure access to the Amazon EKS cluster
9. Update the Kubernetes backend deployment
10. Verify the Kubernetes rollout

The backend is exposed publicly using a Kubernetes `LoadBalancer` service.

### Backend API

```text
http://a7d41dd03ba43462e847f16f29e56a5a-949129658.us-east-1.elb.amazonaws.com/movies
```

The `/movies` endpoint returns the movie data used by the frontend application.

---

## Frontend Continuous Deployment

Workflow:

```text
.github/workflows/frontend-cd.yaml
```

The Frontend CD pipeline deploys the React application to AWS.

The deployment process includes:

1. Checkout repository
2. Run frontend validation
3. Validate the backend API URL
4. Configure AWS credentials
5. Authenticate with Amazon ECR
6. Build the frontend Docker image
7. Inject the backend URL using `REACT_APP_MOVIE_API_URL`
8. Tag the Docker image using the Git commit SHA
9. Push the image to the frontend ECR repository
10. Deploy the new image to Amazon EKS
11. Verify the Kubernetes rollout

The frontend is exposed publicly through a Kubernetes `LoadBalancer` service.

### Frontend Application

```text
http://a2ec729a09bf64209946420746744b01-458530145.us-east-1.elb.amazonaws.com
```

---

# AWS Infrastructure

Terraform is used to provision the AWS infrastructure required by the application.

The infrastructure includes resources required for:

- Amazon EKS
- Amazon ECR
- IAM
- Kubernetes worker infrastructure
- Networking
- Application deployment

### AWS Region

```text
us-east-1
```

### EKS Cluster

```text
cluster
```

### ECR Repositories

```text
frontend
backend
```

---

# Creating the AWS Infrastructure

Navigate to the Terraform directory:

```bash
cd setup/terraform
```

Initialize Terraform:

```bash
terraform init
```

Validate the Terraform configuration:

```bash
terraform validate
```

Review the infrastructure plan:

```bash
terraform plan
```

Create the infrastructure:

```bash
terraform apply
```

Review the Terraform outputs:

```bash
terraform output
```

---

# Configuring Amazon EKS

Configure `kubectl` to communicate with the EKS cluster:

```bash
aws eks update-kubeconfig --name cluster --region us-east-1
```

Verify that the worker nodes are available:

```bash
kubectl get nodes
```

The project initialization script can then be used to configure the required Kubernetes access:

```bash
cd setup
./init.sh
```

---

# GitHub Actions Secrets

Repository secrets are configured under:

```text
Repository
→ Settings
→ Secrets and variables
→ Actions
```

The following secrets are required:

| Secret | Purpose |
| --- | --- |
| `AWS_ACCESS_KEY_ID` | AWS access key used by the deployment workflows |
| `AWS_SECRET_ACCESS_KEY` | AWS secret key used by the deployment workflows |
| `REACT_APP_MOVIE_API_URL` | Public base URL of the backend application |

AWS credentials are stored using GitHub Actions Secrets and are not hard-coded in the workflow files.

### Backend URL used by the React application

```text
http://a7d41dd03ba43462e847f16f29e56a5a-949129658.us-east-1.elb.amazonaws.com
```

The `REACT_APP_MOVIE_API_URL` value contains the backend base URL.

The `/movies` endpoint is accessed by the frontend application when retrieving movie data.

---

# Docker

Both application components have separate Docker images.

## Build Backend Image Locally

From the repository root:

```bash
docker build -t movie-backend:local starter/backend
```

## Build Frontend Image Locally

```bash
docker build \
  --build-arg REACT_APP_MOVIE_API_URL=http://localhost:5000 \
  -t movie-frontend:local \
  starter/frontend
```

The production CI/CD pipelines build and publish the images automatically.

---

# Kubernetes Deployment

The frontend and backend are deployed independently to Amazon EKS.

Check the running pods:

```bash
kubectl get pods
```

Check deployments:

```bash
kubectl get deployments
```

Check services:

```bash
kubectl get services
```

Check all Kubernetes resources:

```bash
kubectl get all
```

Both applications are exposed through Kubernetes `LoadBalancer` services.

---

# Backend Verification

Check the backend service:

```bash
kubectl get service backend
```

Check backend logs:

```bash
kubectl logs deployment/backend --tail=100
```

Verify the public backend API:

```bash
curl http://a7d41dd03ba43462e847f16f29e56a5a-949129658.us-east-1.elb.amazonaws.com/movies
```

A successful request returns the movie list as JSON.

---

# Frontend Verification

Check the frontend service:

```bash
kubectl get service frontend
```

Get the public frontend hostname:

```bash
kubectl get service frontend -o jsonpath="{.status.loadBalancer.ingress[0].hostname}"
```

The deployed frontend is available at:

```text
http://a2ec729a09bf64209946420746744b01-458530145.us-east-1.elb.amazonaws.com
```

The React application retrieves its movie information from the deployed backend API.

---

# Workflow Verification

Successful GitHub Actions runs should exist for all four required workflows:

| Workflow | Purpose |
| --- | --- |
| Frontend Continuous Integration | Lint, test and validate frontend changes |
| Backend Continuous Integration | Lint, test and validate backend changes |
| Frontend Continuous Deployment | Build, publish and deploy the frontend |
| Backend Continuous Deployment | Build, publish and deploy the backend |

The CI pipelines are used to validate application changes.

The CD pipelines publish Docker images to Amazon ECR and deploy the corresponding application to Amazon EKS.

---

# Deployment URLs

| Component | URL |
| --- | --- |
| Frontend | http://a2ec729a09bf64209946420746744b01-458530145.us-east-1.elb.amazonaws.com |
| Backend Movies API | http://a7d41dd03ba43462e847f16f29e56a5a-949129658.us-east-1.elb.amazonaws.com/movies |

---

# Submission Evidence

Before destroying the AWS infrastructure, screenshots should be captured showing the successful deployment.

## Frontend

Open the deployed application:

```text
http://a2ec729a09bf64209946420746744b01-458530145.us-east-1.elb.amazonaws.com
```

The screenshot should show the application and the public URL.


<img width="1365" height="682" alt="image" src="https://github.com/user-attachments/assets/883e1065-5670-4bdb-84e2-72c8f8d1740a" />

## Backend

Open or verify:

```text
http://a7d41dd03ba43462e847f16f29e56a5a-949129658.us-east-1.elb.amazonaws.com/movies
```

The response should show the movie JSON.
<img width="1365" height="683" alt="Screenshot 2026-08-26 222443" src="https://github.com/user-attachments/assets/4083f792-c310-44cd-9a19-dbd1ebb9be8c" />

## Kubernetes

Run:

```bash
kubectl get all
```

Capture the output showing the deployed frontend and backend resources.

## GitHub Actions

Successful runs should be visible for:

- Frontend Continuous Integration
- Backend Continuous Integration
- Frontend Continuous Deployment
- Backend Continuous Deployment

---

# Destroying the Infrastructure

After all required deployment evidence has been captured, the AWS infrastructure can be destroyed to avoid unnecessary resource usage.

Navigate to:

```bash
cd setup/terraform
```

Run:

```bash
terraform destroy
```

Review the resources and confirm the operation when prompted.

---

# Final Project Information

| Item | Value |
| --- | --- |
| Project | Movie Picture Pipeline |
| GitHub Repository | https://github.com/Rriio-09/movie_pipeline_project |
| Frontend | http://a2ec729a09bf64209946420746744b01-458530145.us-east-1.elb.amazonaws.com |
| Backend Movies API | http://a7d41dd03ba43462e847f16f29e56a5a-949129658.us-east-1.elb.amazonaws.com/movies |
| AWS Region | `us-east-1` |
| EKS Cluster | `cluster` |
| Frontend ECR Repository | `frontend` |
| Backend ECR Repository | `backend` |

---

## Project Status

- Frontend Continuous Integration — Successful
- Backend Continuous Integration — Successful
- Frontend Continuous Deployment — Successful
- Backend Continuous Deployment — Successful
- Frontend deployed to Amazon EKS
- Backend deployed to Amazon EKS
- Docker images stored in Amazon ECR
- Infrastructure managed using Terraform
