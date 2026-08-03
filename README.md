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
