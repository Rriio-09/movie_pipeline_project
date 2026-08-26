# Movie Picture Pipeline — Submission Checklist

This repository contains four GitHub Actions workflows required by the project rubric:

- `.github/workflows/frontend-ci.yaml` — Frontend Continuous Integration
- `.github/workflows/backend-ci.yaml` — Backend Continuous Integration
- `.github/workflows/frontend-cd.yaml` — Frontend Continuous Deployment
- `.github/workflows/backend-cd.yaml` — Backend Continuous Deployment

## 1. Create AWS infrastructure

From the project workspace:

```bash
cd setup/terraform
terraform init
terraform apply
terraform output
```

Keep the output values available. The supplied Terraform creates ECR repositories named `frontend` and `backend` and an EKS cluster named `cluster` in `us-east-1`.

Then authorize the GitHub Actions IAM user in the cluster:

```bash
aws eks update-kubeconfig --name cluster --region us-east-1
cd ../../setup
./init.sh
```

## 2. Add GitHub Actions secrets

In GitHub, open **Repository → Settings → Secrets and variables → Actions** and add:

| Secret | Value |
| --- | --- |
| `AWS_ACCESS_KEY_ID` | Access key for `github-action-user` |
| `AWS_SECRET_ACCESS_KEY` | Secret key for `github-action-user` |
| `REACT_APP_MOVIE_API_URL` | Public backend service URL, for example `http://<backend-load-balancer>` |

Do not place AWS credentials directly in workflow YAML files.

## 3. Run backend CD first

Push/merge a backend change to `main`, or manually run **Backend Continuous Deployment**.

After deployment, obtain the backend load balancer hostname:

```bash
kubectl get service backend
```

Wait until `EXTERNAL-IP`/hostname is assigned. Verify:

```bash
curl http://<backend-load-balancer>/movies
```

Expected JSON contains the movie list.

Set the full backend URL as the GitHub secret `REACT_APP_MOVIE_API_URL`.

## 4. Run frontend CD

Push/merge a frontend change to `main`, or manually run **Frontend Continuous Deployment**. The Docker build injects `REACT_APP_MOVIE_API_URL` using a Docker build argument and tags the ECR image with the commit SHA.

Then get the frontend URL:

```bash
kubectl get service frontend
```

Open:

```text
http://<frontend-load-balancer>
```

The page should display the movies returned by the backend API.

## 5. Verify all four workflows

Under the repository **Actions** tab, ensure successful runs exist for:

1. Frontend Continuous Integration
2. Backend Continuous Integration
3. Frontend Continuous Deployment
4. Backend Continuous Deployment

CI workflows trigger for pull requests to `main` when their application paths change. CD workflows trigger for pushes/merges to `main` when their application paths change. All four can also be run manually.

## 6. Capture required evidence

Before destroying AWS resources, save screenshots showing:

1. Frontend application open in a browser with the frontend service URL visible.
2. Backend `/movies` endpoint open/verified with the backend service URL visible.
3. Terminal output from:

```bash
kubectl get all
```

Also keep screenshots of the four successful GitHub Actions workflow runs if desired.

## 7. Tear down AWS resources

After collecting the evidence:

```bash
cd setup/terraform
terraform destroy
```

This avoids unnecessary AWS credit usage.
