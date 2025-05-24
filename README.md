# project2.1
AWS infrastructure + Docker + GitLab CI/CD + Terraform

# DevOps Pet Project: Microservices and AWS Infrastructure

This project demonstrates the deployment and automation of a microservices-based application using **Terraform** and various **AWS services**. It includes multiple services (backend with PostgreSQL and Redis, and a frontend) that are automatically deployed to AWS using modern DevOps tools and best practices.

## Project Structure
├── backend_rds/ # Django backend using PostgreSQL
├── backend_redis/ # Django backend using Redis
├── frontend/ # Frontend (e.g., HTML/JS/React)
└── project-terraform/ # Infrastructure as Code (IaC) with Terraform


## AWS Services Used

### Compute:
- **Amazon ECS** – container orchestration
- **ECS Tasks** – task definitions for:
  - `backend_rds_task`
  - `backend_redis_task`
  - `frontend_task`
- **ECS Services** – for running tasks continuously

### Networking and Security:
- **Amazon VPC** – virtual private cloud for isolation
- **Security Groups** – firewall rules for traffic control
- **Application Load Balancer (ALB)** – distributes incoming traffic to ECS services

### Storage and Databases:
- **Amazon RDS (PostgreSQL)** – relational database for the `backend_rds` service
- **Amazon ElastiCache (Redis)** – in-memory data store for the `backend_redis` service
- **Amazon ECR** – container image registry used by ECS

## How It Works

1. The **infrastructure** is provisioned using Terraform scripts inside the `project-terraform/` directory.
2. Backend and frontend services are **containerized** and pushed to **Amazon ECR**.
3. **ECS** runs these containers as tasks within a cluster.
4. An **Application Load Balancer** manages incoming traffic and routes it to the appropriate services.

## Deployment Guide

> Make sure you have:
> - An AWS account
> - `aws` CLI configured
> - `terraform` installed and initialized

### Step 1: Prepare Environment
cd project-terraform
terraform init

Step 2: Preview the infrastructure
terraform plan
This shows the resources that will be created.

Step 3: Apply the configuration
terraform apply
Confirm with yes to deploy the infrastructure.

Step 4: Deploy Your Containers
Push your Docker images to ECR. Example:

# Authenticate to ECR
aws ecr get-login-password | docker login --username AWS --password-stdin <your-aws-account-id>.dkr.ecr.<region>.amazonaws.com

# Build and push images
docker build -t backend_rds ./backend_rds
docker tag backend_rds:latest <your-ecr-url>/backend_rds:latest
docker push <your-ecr-url>/backend_rds:latest

# Repeat for backend_redis and frontend
Terraform Modules Overview
Each Terraform module is designed for modularity and reuse:
- vpc/: Creates a Virtual Private Cloud, public/private subnets, route tables, and internet gateway.
- security_groups/: Defines firewall rules for backend, frontend, database, and load balancer access.
- alb/: Deploys an Application Load Balancer with target groups and listeners.
- ecr/: Manages Elastic Container Registry repositories for storing Docker images.
- ecs/: Sets up ECS cluster, task definitions, and services for backend and frontend containers.
- rds/: Provisions an RDS PostgreSQL instance for backend_rds.
- elasticache/: Provisions a Redis ElastiCache instance for backend_redis.

Key Outcomes
This project showcases:

Proficiency in building AWS infrastructure using Terraform.

Deployment of microservices in a production-like environment.

Integration of ECS with ALB, RDS, ElastiCache, and ECR.

Secure and scalable architecture using VPC and security groups.

Note:
This project was created for learning purposes and demonstrates practical DevOps skills in a real-world cloud environment.