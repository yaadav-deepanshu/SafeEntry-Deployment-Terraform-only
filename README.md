AWS ECS Fargate Deployment with Terraform

This project provisions a complete AWS infrastructure to deploy a containerized application using Terraform.
The setup is designed to automatically pull a Docker image from Amazon ECR and run it on Amazon ECS with Fargate, fronted by an Application Load Balancer (ALB).

Architecture Overview

ECR (Elastic Container Registry): Stores the Docker image.

ECS Cluster with Fargate: Runs containers serverlessly without managing EC2 instances.

Application Load Balancer: Routes traffic to ECS tasks and provides a public DNS endpoint.

CloudWatch Logs: Captures container logs for observability.

IAM Roles & Security Groups: Ensure secure communication between services.

Workflow:

Push a Docker image to ECR.

ECS automatically pulls the image.

ECS Fargate runs the container and exposes it via the ALB DNS name.

Project Structure
.
├── main.tf          # Main Terraform configuration
├── variables.tf     # Input variables
├── outputs.tf       # Output values (like ALB DNS)
├── .gitignore       # Git ignore rules for Terraform

Prerequisites

Terraform
 installed

AWS CLI configured with valid credentials (aws configure)

Docker installed (for building and pushing images)

Usage

Initialize Terraform

terraform init


Plan the resources

terraform plan


Apply the configuration

terraform apply -auto-approve


Build and push your Docker image to ECR

aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account_id>.dkr.ecr.<region>.amazonaws.com

docker build -t <app_name> .
docker tag <app_name>:latest <account_id>.dkr.ecr.<region>.amazonaws.com/<app_name>:latest
docker push <account_id>.dkr.ecr.<region>.amazonaws.com/<app_name>:latest


Access your application
Get the Load Balancer DNS name from Terraform outputs:

terraform output

Cleanup

To destroy all created resources:

terraform destroy -auto-approve

Next Steps

The next iteration of this project will integrate a Jenkins CI/CD pipeline to automate:

Pulling code from the main branch

Building and pushing a Docker image to ECR

Updating ECS to pull the latest image automatically

Repository

GitHub Repo: https://github.com/yaadav-deepanshu/SafeEntry-Deployment-Terraform-only
