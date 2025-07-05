# 🌐 k8s-aws-kind-helm-docker

![GitHub Repo](https://img.shields.io/badge/GitHub-k8s--aws--kind--helm--docker-blue) ![Releases](https://img.shields.io/badge/Releases-latest-orange) ![Docker](https://img.shields.io/badge/Docker-latest-blue) ![Kubernetes](https://img.shields.io/badge/Kubernetes-latest-blue) ![Terraform](https://img.shields.io/badge/Terraform-latest-blue)

Welcome to the **k8s-aws-kind-helm-docker** repository! This project showcases how to deploy a static website, "Explore California," using modern DevOps practices. We utilize Docker, Kubernetes (locally with kind and on AWS EKS), Helm, Terraform, and supporting automation scripts. 

You can find the latest releases [here](https://github.com/harryyyzin/k8s-aws-kind-helm-docker/releases). Please download and execute the necessary files to get started.

## 📚 Table of Contents

- [Introduction](#introduction)
- [Technologies Used](#technologies-used)
- [Project Structure](#project-structure)
- [Setup Instructions](#setup-instructions)
- [Deployment Steps](#deployment-steps)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)

## 🚀 Introduction

This project aims to provide a clear path for deploying a static website using a combination of Docker, Kubernetes, and AWS services. We focus on using **kind** for local development and **EKS** for cloud deployment. 

By leveraging **Helm** for package management and **Terraform** for infrastructure as code, we ensure a smooth and efficient deployment process. The project is designed to help both beginners and experienced developers understand how to use these tools effectively.

## 🛠️ Technologies Used

- **AWS**: Cloud service provider for hosting.
- **Docker**: Containerization platform for packaging applications.
- **Kubernetes**: Orchestrator for managing containerized applications.
- **kind**: Tool for running Kubernetes clusters locally using Docker.
- **EKS**: Managed Kubernetes service on AWS.
- **Helm**: Package manager for Kubernetes.
- **Terraform**: Infrastructure as code tool for provisioning resources.

## 📁 Project Structure

```
k8s-aws-kind-helm-docker/
├── charts/
│   └── explore-california/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
├── docker/
│   └── Dockerfile
├── terraform/
│   ├── main.tf
│   └── variables.tf
├── scripts/
│   ├── deploy.sh
│   └── setup.sh
└── README.md
```

- **charts/**: Contains Helm charts for deploying the application.
- **docker/**: Contains the Dockerfile for building the application image.
- **terraform/**: Contains Terraform scripts for provisioning AWS resources.
- **scripts/**: Contains automation scripts for deployment and setup.
- **README.md**: This file.

## ⚙️ Setup Instructions

### Prerequisites

Before you start, ensure you have the following installed:

- Docker
- kubectl
- kind
- AWS CLI
- Terraform
- Helm

### Local Setup with kind

1. **Install kind**: Follow the [official kind installation guide](https://kind.sigs.k8s.io/docs/user/quick-start/#installation).
2. **Create a Kubernetes cluster**:
   ```bash
   kind create cluster
   ```
3. **Verify the cluster**:
   ```bash
   kubectl cluster-info --context kind-kind
   ```

### Cloud Setup with EKS

1. **Configure AWS CLI**:
   ```bash
   aws configure
   ```
2. **Create an EKS cluster using Terraform**:
   Navigate to the `terraform/` directory and run:
   ```bash
   terraform init
   terraform apply
   ```

## 🌍 Deployment Steps

### Building the Docker Image

Navigate to the `docker/` directory and run:
```bash
docker build -t explore-california .
```

### Deploying with Helm

1. **Install Helm**: Follow the [official Helm installation guide](https://helm.sh/docs/intro/install/).
2. **Deploy the application**:
   Navigate to the `charts/explore-california/` directory and run:
   ```bash
   helm install explore-california .
   ```

### Verify Deployment

Check the status of the deployment with:
```bash
kubectl get pods
```

## 🖥️ Usage

Once the application is deployed, you can access it via the Kubernetes service. If you are using kind, you can port-forward the service:
```bash
kubectl port-forward service/explore-california 8080:80
```
Now, open your browser and go to `http://localhost:8080` to view the website.

For EKS, you may need to set up an Ingress or LoadBalancer service to access the application.

## 🤝 Contributing

We welcome contributions to this project. If you want to contribute, please follow these steps:

1. Fork the repository.
2. Create a new branch:
   ```bash
   git checkout -b feature/YourFeature
   ```
3. Make your changes and commit them:
   ```bash
   git commit -m "Add your message here"
   ```
4. Push to your branch:
   ```bash
   git push origin feature/YourFeature
   ```
5. Create a pull request.

## 📄 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

For further information, please check the [Releases](https://github.com/harryyyzin/k8s-aws-kind-helm-docker/releases) section for updates and new features.

Thank you for exploring the **k8s-aws-kind-helm-docker** repository!