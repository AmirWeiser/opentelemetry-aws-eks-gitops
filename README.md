# OpenTelemetry Astronomy Shop on AWS EKS

An end-to-end DevOps project that provisions AWS infrastructure using Terraform and deploys the OpenTelemetry Astronomy Shop microservices application to Amazon EKS.

The project demonstrates Infrastructure as Code, AWS networking, Kubernetes orchestration, containerized microservices deployment, and production-style infrastructure practices.

## Project Status

Current implementation:

- AWS infrastructure provisioning with Terraform
- Custom VPC architecture
- Amazon EKS cluster
- Kubernetes deployment of 20+ microservices
- Kubernetes Services and application configuration
- Local deployment validation with Minikube

Planned improvements:

- GitHub Actions CI
- Helm packaging
- Argo CD continuous delivery
- GitOps workflow
- Security scanning
- Monitoring and observability improvements

## Architecture

The project creates the following deployment flow:

```text
Developer
    |
    v
GitHub Repository
    |
    v
Terraform
    |
    v
AWS Infrastructure
    |
    +-- VPC
    +-- Public Subnets
    +-- Private Subnets
    +-- Internet Gateway
    +-- NAT Gateway
    +-- Route Tables
    +-- Security Groups
    +-- Amazon EKS
            |
            v
Kubernetes Workloads
            |
            +-- Frontend
            +-- Cart Service
            +-- Checkout Service
            +-- Payment Service
            +-- Product Catalog Service
            +-- Recommendation Service
            +-- Additional Microservices

Technologies
- Cloud and Infrastructure
- AWS
- Amazon EKS
- Amazon VPC
- IAM
- Terraform
Containers and Orchestration
- Docker
- Kubernetes
- Minikube
Planned CI/CD and GitOps
- GitHub Actions
- Helm
- Argo CD

Repository Structure
.
├── eks/
│   ├── backend/
│   ├── modules/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── kubernetes/
│   ├── deployments/
│   ├── services/
│   └── application manifests
├── docs/
│   └── images/
├── scripts/
├── .gitignore
└── README.md


Application

This project uses the OpenTelemetry Astronomy Shop as the application workload.

The Astronomy Shop is a distributed microservices application created by the OpenTelemetry project to demonstrate observability and OpenTelemetry instrumentation in a realistic environment.

The application source code was not developed as part of this repository.

This repository focuses on:

AWS infrastructure design
Terraform automation
Amazon EKS provisioning
Kubernetes deployment configuration
Infrastructure validation
Future CI/CD and GitOps automation
Prerequisites

Install and configure:

Git
Terraform
AWS CLI
kubectl
Minikube
An AWS account
AWS credentials with sufficient permissions

Verify the tools:

terraform version
aws --version
kubectl version --client
minikube version

Configure AWS credentials:

aws configure

Verify authentication:

aws sts get-caller-identity




Local Kubernetes Validation with Minikube

The Kubernetes manifests can be validated locally before deploying to AWS.

Start Minikube:

minikube start --cpus=4 --memory=8192

Create a namespace:

kubectl create namespace otel-demo

Deploy the application:

kubectl apply -f kubernetes/ -n otel-demo

Check the Pods:

kubectl get pods -n otel-demo

Check the Services:

kubectl get services -n otel-demo

Watch the deployment process:

kubectl get pods -n otel-demo -w

Access the frontend:

kubectl port-forward service/frontend 8080:8080 -n otel-demo

Open:

http://localhost:8080

Remove the local deployment:

kubectl delete namespace otel-demo

Stop Minikube:

minikube stop






AWS Infrastructure Deployment

Navigate to the Terraform directory:

cd eks

Initialize Terraform:

terraform init

Format the Terraform configuration:

terraform fmt -recursive

Validate the configuration:

terraform validate

Preview the infrastructure changes:

terraform plan

Provision the infrastructure:

terraform apply

Approve the deployment when prompted.

Configure kubectl for Amazon EKS

After the EKS cluster is created, update the local kubeconfig:

aws eks update-kubeconfig \
  --region <aws-region> \
  --name <eks-cluster-name>

Verify cluster access:

kubectl get nodes
Deploy the Application to Amazon EKS

Create the namespace:

kubectl create namespace otel-demo

Apply the Kubernetes manifests:

kubectl apply -f kubernetes/ -n otel-demo

Verify the deployment:

kubectl get pods -n otel-demo
kubectl get services -n otel-demo
Access the Application

Check the frontend Service:

kubectl get service frontend -n otel-demo

Depending on the Service type, use one of the following methods.

Port Forward
kubectl port-forward service/frontend 8080:8080 -n otel-demo

Open:

http://localhost:8080
LoadBalancer

If the frontend Service uses the LoadBalancer type:

kubectl get service frontend -n otel-demo

Wait until an external address is assigned.

Infrastructure Cleanup

Delete the Kubernetes workloads:

kubectl delete namespace otel-demo

Destroy the AWS infrastructure:

cd eks
terraform destroy

Review the destruction plan and approve it.

Security Notes

The repository intentionally excludes:

Terraform state files
AWS credentials
Private keys
Local Terraform provider files
Environment files
Sensitive variable files

Do not commit:

.terraform/
terraform.tfstate
terraform.tfstate.backup
*.tfvars
.env
*.pem
*.key
Project Scope and Attribution

The OpenTelemetry Astronomy Shop application is maintained by the OpenTelemetry project.

My work in this repository focuses on:

Designing AWS infrastructure
Building reusable Terraform configuration
Provisioning Amazon EKS
Configuring Kubernetes workloads
Validating the deployment locally and in AWS
Developing the future CI/CD and GitOps implementation
Roadmap
 Terraform infrastructure
 AWS VPC
 Amazon EKS
 Kubernetes deployment
 Local Minikube validation
 Terraform CI validation
 Kubernetes manifest validation
 Helm chart
 GitHub Actions
 Argo CD
 GitOps deployment
 Container image scanning
 Infrastructure security scanning
 Monitoring dashboards
Author

Amir Weiser

GitHub: https://github.com/AmirWeiser
LinkedIn: https://www.linkedin.com/in/amir-weiser/

