# Movie Pipeline Project

## 🔗 Project Links

| Resource | Link |
|---|---|
| 📁 GitHub Repository | [Open GitHub Repository](https://github.com/Rriio-09/movie_pipeline_project) |
| 🌐 Frontend Application | [Open Frontend Application](http://a2ec729a09bf64209946420746744b01-458530145.us-east-1.elb.amazonaws.com) |
| 🔧 Backend Movies API | [Open Backend API](http://a7d41dd03ba43462e847f16f29e56a5a-949129658.us-east-1.elb.amazonaws.com/movies) |

## Project Overview

This project implements a complete CI/CD pipeline for a frontend and backend application using:

- GitHub Actions
- Docker
- Amazon ECR
- Amazon EKS
- Kubernetes
- AWS Load Balancers

The project contains separate Continuous Integration (CI) and Continuous Deployment (CD) workflows for both the frontend and backend.

---

## CI/CD Workflows

The repository contains four GitHub Actions workflows:

1. Frontend Continuous Integration
2. Frontend Continuous Deployment
3. Backend Continuous Integration
4. Backend Continuous Deployment

### Frontend Continuous Integration

The Frontend CI workflow successfully performs linting, testing, and Docker image build.

<img width="1365" height="683" alt="image" src="https://github.com/user-attachments/assets/f44bd03c-13ea-46ab-8cb0-aa19fa7b4b65" />


### Frontend Continuous Deployment

The Frontend CD workflow successfully builds and publishes the frontend Docker image and deploys it to Amazon EKS.

<img width="1365" height="685" alt="image" src="https://github.com/user-attachments/assets/9c518dda-534f-4ba9-b54f-a3784ceaff8e" />


### Backend Continuous Integration

The Backend CI workflow successfully performs linting, testing, and Docker image build.

<img width="1365" height="679" alt="image" src="https://github.com/user-attachments/assets/4e81bc89-c337-4327-a305-bf0d42682e95" />


### Backend Continuous Deployment

The Backend CD workflow successfully builds and publishes the backend Docker image and deploys it to Amazon EKS.

<img width="1365" height="687" alt="image" src="https://github.com/user-attachments/assets/ffa3e70f-37c9-4730-84e6-cb42d2caabdb" />


---

## Frontend Application

The frontend application was successfully deployed to Amazon EKS and exposed using an AWS LoadBalancer service.

The deployed application successfully displays the movie list.

<img width="1600" height="813" alt="image" src="https://github.com/user-attachments/assets/b936ac7a-0ae0-4ef3-a87d-6959dea6f166" />


---

## Backend Application

The backend application was successfully deployed to Amazon EKS and exposed using an AWS LoadBalancer service.

The `/movies` API endpoint successfully returns movie data in JSON format.

<img width="1600" height="807" alt="image" src="https://github.com/user-attachments/assets/baf81fe7-1fea-41cf-934f-c912f99c3c98" />


---

## Kubernetes Resources

The following screenshot shows the running Kubernetes resources including:

- Frontend Pod
- Backend Pod
- Frontend Service
- Backend Service
- Frontend Deployment
- Backend Deployment
- ReplicaSets

Both frontend and backend pods are in the `Running` state.

<img width="1477" height="757" alt="image" src="https://github.com/user-attachments/assets/c53c23f0-2d09-437a-84f0-aec81b9e88aa" />

<img width="1476" height="751" alt="image" src="https://github.com/user-attachments/assets/048533a4-bcb4-49fd-a33c-00b9731697cc" />

<img width="1477" height="755" alt="image" src="https://github.com/user-attachments/assets/f3bd1b41-bd9a-4c99-94e1-4c89f7ded35b" />

---

## Amazon ECR Repositories

Separate Amazon ECR repositories were created for the frontend and backend Docker images.

<img width="1211" height="601" alt="image" src="https://github.com/user-attachments/assets/a83f7a2b-201e-488d-ae49-b41f1e6c1e06" />

### Frontend ECR Images

The frontend Docker images were successfully pushed to Amazon ECR.

<img width="1365" height="677" alt="image" src="https://github.com/user-attachments/assets/f460f0ad-0619-4bc9-bda0-fea72d2ae712" />


### Backend ECR Images

The backend Docker images were successfully pushed to Amazon ECR.

<img width="1365" height="678" alt="image" src="https://github.com/user-attachments/assets/b424a3c2-27a8-4986-a8e8-775765f9745e" />


## Project Architecture

GitHub Repository  
↓  
GitHub Actions CI  
↓  
Docker Image Build  
↓  
Amazon ECR  
↓  
GitHub Actions CD  
↓  
Amazon EKS  
↓  
Kubernetes Deployment & Service  
↓  
AWS Load Balancer  
↓  
Frontend / Backend Application

---

## Deployment Verification

The deployment was verified using:

```bash
kubectl get all

The command confirmed that:

- Frontend pod is running successfully.
- Backend pod is running successfully.
- Frontend deployment has the required available replica.
- Backend deployment has the required available replica.
- Frontend service is exposed using an AWS LoadBalancer.
- Backend service is exposed using an AWS LoadBalancer.

---

## Deployment Status

| Component | Status |
|---|---|
| Frontend Continuous Integration | ✅ Successful |
| Frontend Continuous Deployment | ✅ Successful |
| Backend Continuous Integration | ✅ Successful |
| Backend Continuous Deployment | ✅ Successful |
| Frontend EKS Deployment | ✅ Running |
| Backend EKS Deployment | ✅ Running |
| Frontend LoadBalancer | ✅ Working |
| Backend LoadBalancer | ✅ Working |
| Frontend ECR Image | ✅ Available |
| Backend ECR Image | ✅ Available |

---

## Review Evidence

This repository includes deployment evidence for the required project review:

- ✅ Public GitHub repository
- ✅ Four GitHub Actions workflows
- ✅ Successful Frontend CI workflow
- ✅ Successful Frontend CD workflow
- ✅ Successful Backend CI workflow
- ✅ Successful Backend CD workflow
- ✅ Working Frontend application with service URL visible
- ✅ Working Backend `/movies` API with service URL visible
- ✅ `kubectl get all` output showing running Kubernetes resources
- ✅ Amazon ECR repositories and Docker images

---

## Project Status

The Movie Pipeline project has been successfully containerized and deployed using an automated CI/CD pipeline.

The frontend and backend Docker images are stored in Amazon ECR and deployed to Amazon EKS using GitHub Actions and Kubernetes.

**Final Status: Deployment Successful**
