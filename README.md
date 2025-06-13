[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://linkedin.com/in/shrejae)

# Explore California: Kubernetes Project

This project demonstrates deploying a static website ("Explore California") using modern DevOps practices, including Docker, Kubernetes (both local with kind and on AWS EKS), Helm, Terraform, and supporting automation scripts.

---

## Table of Contents

- [Explore California: Kubernetes Project](#explore-california-kubernetes-project)
  - [Table of Contents](#table-of-contents)
  - [Project Overview](#project-overview)
  - [Architecture](#architecture)
  - [Prerequisites](#prerequisites)
  - [Local Development \& Testing](#local-development--testing)
    - [1. Build and Run Locally with Docker](#1-build-and-run-locally-with-docker)
    - [2. Local Kubernetes with kind](#2-local-kubernetes-with-kind)
  - [Kubernetes on AWS (EKS)](#kubernetes-on-aws-eks)
    - [1. AWS Setup](#1-aws-setup)
    - [2. Provision EKS Cluster](#2-provision-eks-cluster)
    - [3. Deploy the Application](#3-deploy-the-application)
    - [4. Verify](#4-verify)
  - [Kubernetes with kind (Local)](#kubernetes-with-kind-local)
  - [Helm Chart](#helm-chart)
  - [Terraform Infrastructure](#terraform-infrastructure)
  - [Project Structure](#project-structure)
  - [Cleanup](#cleanup)
  - [Cost Warning](#cost-warning)
  - [References](#references)

---

## Project Overview

- **Website**: A static HTML/CSS website for "Explore California" (in `website/`).
- **Containerization**: Uses Docker and NGINX to serve the site.
- **Kubernetes**: Deploys the site to Kubernetes clusters, both locally (kind) and on AWS EKS.
- **Infrastructure as Code**: Uses Terraform for AWS infrastructure provisioning.
- **Helm**: Manages Kubernetes manifests and deployments.
- **Automation**: Makefile and shell scripts automate setup, deployment, and teardown.

---

## Architecture

- **website/**: Static site assets (HTML, CSS, images, etc.)
- **Dockerfile**: Builds an NGINX image serving the website.
- **docker-compose.yml**: For local multi-container development/testing.
- **chart/**: Helm chart for Kubernetes deployment.
- **infra/**: Terraform code for AWS EKS and networking.
- **create_cluster.sh / delete_cluster.sh**: Scripts to provision and destroy AWS EKS clusters.
- **Makefile**: Automates local Kubernetes (kind) setup and deployment.

---

## Prerequisites

- **General**:
  - Docker & Docker Compose
  - [kubectl](https://kubernetes.io/docs/tasks/tools/)
  - [kind](https://kind.sigs.k8s.io/) (for local clusters)
  - [Helm](https://helm.sh/)
  - [AWS CLI](https://aws.amazon.com/cli/)
  - [Terraform](https://terraform.io/)
  - AWS account with admin permissions

- **For AWS/EKS**:
  - AWS credentials configured (`aws configure`)
  - IAM user and role with `AdministratorAccess`
  - S3 bucket for Terraform state

---

## Local Development & Testing

### 1. Build and Run Locally with Docker

```sh
make run_website
# or manually:
docker build -t explorecalifornia.com .
docker run -p 5000:80 --rm explorecalifornia.com
```
Visit [http://localhost:5000](http://localhost:5000).

### 2. Local Kubernetes with kind

- Install kind and kubectl (see Makefile targets).
- Create a local registry and cluster:

```sh
make create_kind_cluster_with_registry
make install_ingress_controller
make install_app
```

- Access the site at [http://localhost](http://localhost).

---

## Kubernetes on AWS (EKS)

### 1. AWS Setup

- Create an AWS account and configure credentials.
- Create an IAM user and role with `AdministratorAccess`.
- Set up your shell with temporary credentials (see README for details).

### 2. Provision EKS Cluster

```sh
# Create S3 bucket for Terraform state
aws s3 mb s3://<your-bucket>_kubernetes_fundamentals

# Create the cluster (takes ~20 minutes)
TERRAFORM_S3_BUCKET=<your-bucket>_kubernetes_fundamentals TERRAFORM_S3_KEY=state ./create_cluster.sh
```

### 3. Deploy the Application

- Update kubeconfig:
  ```sh
  aws eks update-kubeconfig --cluster-name explore-california-cluster
  ```
- Deploy with Helm:
  ```sh
  helm upgrade --atomic --install explorecalifornia.com ./chart
  ```

### 4. Verify

```sh
kubectl get nodes
kubectl get svc
kubectl get ingress
```

---

## Kubernetes with kind (Local)

- See the [Makefile](Makefile) for targets to automate:
  - Cluster creation
  - Local registry setup
  - Ingress controller installation
  - App deployment

---

## Helm Chart

- Located in `chart/`
- Values can be customized in `chart/values.yaml`
- Templates for Deployment, Service, and Ingress in `chart/templates/`

---

## Terraform Infrastructure

- Located in `infra/`
- Provisions VPC, EKS cluster, node groups, and ECR repository.
- Uses S3 for remote state.

---

## Project Structure

```
.
├── chart/                # Helm chart for Kubernetes deployment
├── infra/                # Terraform code for AWS infrastructure
├── website/              # Static website files
├── Dockerfile            # NGINX Docker image for the website
├── docker-compose.yml    # Local development/testing
├── Makefile              # Automation for local Kubernetes (kind)
├── create_cluster.sh     # Script to provision AWS EKS cluster
├── delete_cluster.sh     # Script to destroy AWS EKS cluster
├── kind_config.yaml      # kind cluster configuration
├── kind_configmap.yaml   # kind local registry config
├── nginx.conf            # NGINX configuration
├── terraform.Dockerfile  # Dockerfile for Terraform + AWS CLI
└── README.md             # This file
```

---

## Cleanup

- **Delete AWS resources to avoid charges:**
  ```sh
  TERRAFORM_S3_BUCKET=<your-bucket>_kubernetes_fundamentals TERRAFORM_S3_KEY=state ./delete_cluster.sh
  ```

- **Delete local kind cluster:**
  ```sh
  kind delete cluster --name explorecalifornia.com
  ```

---

## Cost Warning

> **AWS resources created by this project will incur costs.**  
> Remember to destroy your EKS cluster and related resources when done.

---

## References

- [Kubernetes](https://kubernetes.io/)
- [kind](https://kind.sigs.k8s.io/)
- [Helm](https://helm.sh/)
- [Terraform](https://terraform.io/)
- [AWS EKS](https://aws.amazon.com/eks/)

---
