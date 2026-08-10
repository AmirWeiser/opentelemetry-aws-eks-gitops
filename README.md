# OpenTelemetry Astronomy Shop on AWS EKS

An end-to-end DevOps project that provisions AWS infrastructure using Terraform and deploys the OpenTelemetry Astronomy Shop microservices application to Amazon EKS.

The project demonstrates Infrastructure as Code, AWS networking, Kubernetes orchestration, containerized microservices deployment, and production-style infrastructure practices.

## Project Overview

This repository contains the infrastructure and Kubernetes configuration required to deploy the OpenTelemetry Astronomy Shop application.

The project currently includes:

- AWS infrastructure provisioning with Terraform
- A custom AWS VPC
- Public and private subnets
- Internet Gateway
- NAT Gateway
- Route tables
- Security groups
- Amazon EKS
- Kubernetes manifests
- Deployment of more than 20 microservices
- Local validation with Minikube

Future versions of the project will include:

- Helm
- Security scanning
- Monitoring improvements

## Architecture

The current deployment flow is:

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
    |
    +-- Public Subnets
    |
    +-- Private Subnets
    |
    +-- Internet Gateway
    |
    +-- NAT Gateway
    |
    +-- Route Tables
    |
    +-- Security Groups
    |
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
            +-- OpenTelemetry Collector
            +-- Additional Microservices
```

## Technologies

### Cloud and Infrastructure

- AWS
- Amazon EKS
- Amazon VPC
- IAM
- Terraform

### Containers and Orchestration

- Docker
- Kubernetes
- Minikube

### Infrastructure Components

- Public Subnets
- Private Subnets
- Internet Gateway
- NAT Gateway
- Route Tables
- Security Groups
- EKS Managed Node Group

### CI/CD and GitOps

- GitHub Actions
- Argo CD
- GitOps

## Repository Structure

```text
.
├── eks/
│   ├── backend/
│   ├── modules/
│   │   ├── vpc/
│   │   └── eks/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── providers.tf
│
├── kubernetes/
│   ├── deployments/
│   ├── services/
│   ├── configmaps/
│   └── additional-manifests/
│
├── docs/
│   └── images/
│
├── .gitignore
└── README.md
```

The exact Kubernetes directory structure may differ depending on how the manifests are currently organized.

## Application

This project uses the OpenTelemetry Astronomy Shop as the application workload.

The OpenTelemetry Astronomy Shop is a distributed microservices application created by the OpenTelemetry project. It is designed to demonstrate OpenTelemetry instrumentation and observability in an environment that resembles a real-world distributed system.

The application source code was not developed as part of this repository.

This repository focuses on:

- AWS infrastructure design
- Terraform automation
- Amazon EKS provisioning
- Kubernetes workload deployment
- Infrastructure validation
- Local Kubernetes testing
- Future CI/CD implementation
- Future GitOps implementation

## Project Scope and Attribution

The OpenTelemetry Astronomy Shop application is maintained by the OpenTelemetry project.

My work in this repository focuses on:

- Designing the AWS infrastructure
- Building the Terraform configuration
- Creating reusable Terraform modules
- Provisioning the VPC and networking components
- Provisioning Amazon EKS
- Configuring Kubernetes workloads
- Adapting Kubernetes manifests for deployment
- Validating the deployment on Minikube
- Deploying the application on Amazon EKS
- Developing future CI/CD and GitOps workflows

Some application manifests and container configurations are based on the original OpenTelemetry Demo project.

## Prerequisites

Before using this project, install the following tools:

- Git
- Terraform
- AWS CLI
- kubectl
- Minikube
- Docker
- An AWS account
- AWS credentials with sufficient permissions

Verify the installations:

```bash
git --version
terraform version
aws --version
kubectl version --client
minikube version
docker version
```

## AWS Authentication

Configure the AWS CLI:

```bash
aws configure
```

You will be asked to enter:

```text
AWS Access Key ID
AWS Secret Access Key
Default region
Default output format
```

Verify that authentication works:

```bash
aws sts get-caller-identity
```

Do not store AWS credentials inside the repository.

## Local Kubernetes Validation with Minikube

The Kubernetes manifests can be tested locally before deploying them to Amazon EKS.

### Start Minikube

For this application, Minikube should have enough CPU and memory to run more than 20 microservices.

```bash
minikube start --cpus=4 --memory=8192
```

If your computer has enough available resources, you can allocate more:

```bash
minikube start --cpus=6 --memory=12288
```

### Verify Minikube

```bash
minikube status
```

Verify that the Kubernetes node is available:

```bash
kubectl get nodes
```

### Create a Namespace

```bash
kubectl create namespace otel-demo
```

### Deploy the Application

From the repository root:

```bash
kubectl apply -f complete-deploy/ -n otel-demo
```

### Watch the deployment process:

```bash
kubectl get pods -n otel-demo -w
```

### Check the Services

```bash
kubectl get services -n otel-demo
```

### Access the Frontend

First, identify the frontend Service:

```bash
kubectl get services -n otel-demo
```

```bash
kubectl port-forward service/frontendproxy 8080:8080 -n otel-demo
```

Open the application in the browser:

```text
http://localhost:8080
```

Keep the terminal running while using the application.

Stop the port forwarding with:

```text
Ctrl + C
```

### Remove the Local Deployment

Delete the namespace and all resources inside it:

```bash
kubectl delete namespace otel-demo
```

Stop Minikube:

```bash
minikube stop
```

To permanently delete the Minikube cluster:

```bash
minikube delete
```

## Terraform Infrastructure

The Terraform configuration provisions the AWS infrastructure required for the project.

The infrastructure includes:

- AWS VPC
- Public subnets
- Private subnets
- Internet Gateway
- NAT Gateway
- Route tables
- Security groups
- Amazon EKS
- EKS node group
- IAM roles and policies

## Terraform Backend

The project uses a remote Terraform backend for storing Terraform state.

Terraform state files must never be committed to Git.

## Deploy the Terraform Backend

The `eks/backend` directory creates the backend infrastructure, enter it first:

```bash
cd eks/backend
```

### Change configuration to match your AWS region and unique bucket name and then Initialize Terraform:

```bash
terraform init
```

Format the configuration:

```bash
terraform fmt -recursive
```

Validate the configuration:

```bash
terraform validate
```

Create a Terraform plan:

```bash
terraform plan
```

Apply the configuration:

```bash
terraform apply
```

Review the plan and enter:

```text
yes
```

Return to the repository root:

```bash
cd ../..
```

## Deploy the AWS Infrastructure

Enter the main Terraform directory:

```bash
cd eks
```

Initialize Terraform:

```bash
terraform init
```

Format the Terraform files:

```bash
terraform fmt -recursive
```

Validate the Terraform configuration:

```bash
terraform validate
```

Create an execution plan:

```bash
terraform plan
```

Provision the infrastructure:

```bash
terraform apply
```

Review the Terraform plan carefully before approving it.

When prompted, enter:

```text
yes
```

Provisioning Amazon EKS may take several minutes.

## Terraform Outputs

After the deployment completes, display the outputs:

```bash
terraform output
```

The outputs may include:

- VPC ID
- Public subnet IDs
- Private subnet IDs
- EKS cluster name
- EKS cluster endpoint
- AWS region

## Configure kubectl for Amazon EKS

Connect to the EKS control panel and update the local kubeconfig:

```bash
aws eks update-kubeconfig \
  --region <aws-region> \
  --name <eks-cluster-name>
```

Replace:

```text
<aws-region>
```

with the AWS region used by the Terraform configuration.

Replace:

```text
<eks-cluster-name>
```

with the EKS cluster name created by Terraform.

Verify the current Kubernetes context:

```bash
kubectl config current-context
```

Verify access to the EKS cluster:

```bash
kubectl get nodes
```

The EKS worker nodes should appear with the status:

```text
Ready
```

## Deploy the Application to Amazon EKS

Return to the repository root if necessary:

```bash
cd ..
```

Create a namespace:

```bash
kubectl create namespace otel-demo
```

Deploy the Kubernetes manifests:

```bash
kubectl apply -f complete-deploy/ -n otel-demo
```

Watch the Pods during startup:

```bash
kubectl get pods -n otel-demo -w
```

Verify the Services:

```bash
kubectl get services -n otel-demo
```

Verify the Deployments:

```bash
kubectl get deployments -n otel-demo
```

Display all namespaced resources:

```bash
kubectl get all -n otel-demo
```

## Access the Application on EKS

Check the frontend Service:

```bash
kubectl get service frontendproxy -n otel-demo
```

The access method depends on the Service type.

### Port Forward

```bash
kubectl port-forward service/frontendproxy 8080:8080 -n otel-demo
```

Open:

```text
http://localhost:8080
```

### LoadBalancer

You can change the frontendproxy Service to use the `LoadBalancer` type:

```bash
kubectl get service frontendproxy -n otel-demo -w
```

Wait until an external hostname appears under:

```text
EXTERNAL-IP
```

Open the external hostname in the browser.

## Verification

Check that all Pods are running:

```bash
kubectl get pods -n otel-demo
```

Expected Pod status:

```text
Running
```

## Security Notes

The repository intentionally excludes sensitive and generated files.

Do not commit:

```text
.terraform/
terraform.tfstate
terraform.tfstate.backup
*.tfstate
*.tfstate.*
*.tfvars
*.tfplan
.env
.env.*
*.pem
*.key
AWS credentials
Private keys
Access tokens
```

Terraform provider files inside `.terraform` can be hundreds of megabytes and must not be committed.

Terraform downloads them again when running:

```bash
terraform init
```

## Recommended .gitignore

```gitignore
# Terraform working directories
**/.terraform/

# Terraform state files
*.tfstate
*.tfstate.*

# Terraform variable files
*.tfvars
*.tfvars.json

# Terraform plan files
*.tfplan
*.plan

# Terraform crash logs
crash.log
crash.*.log

# Terraform override files
override.tf
override.tf.json
*_override.tf
*_override.tf.json

# Terraform CLI configuration
.terraformrc
terraform.rc

# Environment files
.env
.env.*

# Credentials and private keys
*.pem
*.key
*credentials*
*access keys*

# Operating system files
.DS_Store
Thumbs.db
```

The `.terraform.lock.hcl` file should normally remain in Git because it records the selected Terraform provider versions.

## Screenshots

Screenshots will be added to demonstrate the deployed environment.


- Application frontend:
  <img width="951" height="472" alt="צילום מסך 2026-07-23 144252" src="https://github.com/user-attachments/assets/9beb00be-ecea-4142-a39a-c584b8d1b773" />

- Kubernetes complete-deploy:
  <img width="921" height="466" alt="צילום מסך 2026-07-23 143410" src="https://github.com/user-attachments/assets/cf0bf85d-c18c-470c-9edd-60134ee47dff" />

- Amazon EKS cluster:
  <img width="947" height="385" alt="צילום מסך 2026-07-23 142120" src="https://github.com/user-attachments/assets/e30e5698-b60c-4bb5-b020-a7b9744a7ce6" />

- AWS VPC:
  <img width="626" height="314" alt="צילום מסך 2026-07-23 144753" src="https://github.com/user-attachments/assets/fa25b4e0-22da-4085-8107-9972985c09e2" />

- Terraform outputs:
  <img width="554" height="78" alt="צילום מסך 2026-07-23 142012" src="https://github.com/user-attachments/assets/3c32c21d-61ab-4e23-914c-ea31bc426d46" />

Planned Screnshots:

- Argo CD dashboard
- GitHub Actions workflow

## Learning Goals

This project was created to strengthen practical experience in:

- AWS cloud infrastructure
- Infrastructure as Code
- Terraform modules
- AWS networking
- Amazon EKS
- Kubernetes
- Microservices deployment
- Cloud-native architecture
- Troubleshooting
- CI/CD
- GitOps
- Argo CD
- Helm
- Monitoring
- DevSecOps

## Author

**Amir Weiser**

DevOps and Infrastructure Engineer

- GitHub: [github.com/AmirWeiser](https://github.com/AmirWeiser)
- LinkedIn: [linkedin.com/in/amir-weiser](https://www.linkedin.com/in/amir-weiser/)
