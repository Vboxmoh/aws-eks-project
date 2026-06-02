# AWS EKS Microservices Project

## Project Overview
This project deploys a WordPress microservice on Amazon EKS using Helm as a package manager for Kubernetes. The infrastructure is automatically provisioned by eksctl using CloudFormation, with a Horizontal Pod Autoscaler configured to dynamically scale pods based on CPU utilization, ensuring high availability and performance under varying traffic loads.

## Stack
- Cloud Provider: AWS (Amazon Web Services)
- Region: eu-west-3 (Paris)
- Container Orchestration: Amazon EKS
- IaC: eksctl + CloudFormation
- Package Manager: Helm
- CMS: WordPress (latest)
- Container Runtime: Docker
- Autoscaling: Horizontal Pod Autoscaler

## Project Architecture Overview
Internet
│
▼
[User Browser]
│
│ HTTP (Port 80)
▼
[AWS Load Balancer]
│
▼
[EKS Cluster - eu-west-3]
├── Node 1 (EC2 t3.micro)
│   └── Pod (WordPress Container)
├── Node 2 (EC2 t3.micro)
│   └── Pod (WordPress Container)
└── Node 3 (EC2 t3.micro)
└── Pod (WordPress Container)
└── Horizontal Pod Autoscaler
├── minReplicas: 1
├── maxReplicas: 5
└── CPU threshold: 50%
## Steps Completed

### Step 1 — Install Required Tools
Verified AWS CLI installation and downloaded eksctl — the command-line utility for creating and managing EKS clusters. eksctl simplifies the creation of Kubernetes clusters on AWS by abstracting the complexity of CloudFormation stack creation.

### Step 2 — Set Up AWS CLI Credentials
Configured the AWS CLI with IAM user credentials using aws configure. This links the local environment to the AWS account enabling eksctl and kubectl to create and manage resources. Verified the connection using aws sts get-caller-identity.

### Step 3 — Create the EKS Cluster
Created a managed EKS cluster named enterprise-cluster in eu-west-3 with 3 worker nodes of type t3.micro using eksctl. Under the hood eksctl used CloudFormation to automatically provision the VPC, subnets, security groups, IAM roles and EC2 instances. The cluster creation took approximately 30 minutes to complete.

### Step 4 — Verify the Cluster Creation
Updated the local kubectl configuration to point to the new EKS cluster using aws eks update-kubeconfig. Verified that all 3 worker nodes were in Ready status using kubectl get nodes confirming the cluster was fully operational.

### Step 5 — Install Helm
Installed Helm v3 — the package manager for Kubernetes. Helm simplifies the deployment and management of applications on Kubernetes by using charts which are blueprints that define how applications are deployed.

### Step 6 — Create a Helm Chart for the Microservice
Created a Helm chart named my-microservice using helm create. Configured the values.yaml file to deploy the official WordPress Docker image with a LoadBalancer service type on port 80 and defined CPU and memory resource limits for the pods.

### Step 7 — Deploy the Microservice Using Helm
Deployed the WordPress microservice to the EKS cluster using helm install. Helm packaged all the Kubernetes resources defined in the chart and deployed them to the cluster making the application available on the worker nodes.

### Step 8 — Verify the Microservices Deployment
Verified the Helm release status using helm status confirming STATUS: deployed. Checked running pods using kubectl get pods confirming the WordPress container was Running successfully inside a pod on one of the worker nodes.

### Step 9 — Implement Horizontal Pod Autoscaler
Created and applied a Horizontal Pod Autoscaler configuration using kubectl apply. The HPA was configured to maintain a minimum of 1 pod and scale up to a maximum of 5 pods automatically when CPU utilization exceeds 50 percent.

### Step 10 — Test Autoscaling Using Siege
Deployed a Siege load testing pod to simulate 100 concurrent users hitting the WordPress service for 5 minutes. Kubernetes self-healing was observed in real time — the original pod went into Error state under heavy load and Kubernetes automatically spun up a replacement pod to maintain service availability. Accessed the WordPress application via the Load Balancer external IP confirming successful deployment.

## Key Concepts Learned
- Microservices architecture
- Amazon EKS and Kubernetes cluster management
- Helm charts and package management for Kubernetes
- Horizontal Pod Autoscaling
- Kubernetes self-healing
- Load Balancer configuration
- CloudFormation integration with EKS

## Screenshots
All screenshots are available in the screenshots folder.

## Technologies Used
- Amazon EKS
- eksctl
- kubectl
- Helm
- WordPress
- Docker
- AWS CloudFormation
- Kubernetes HPA
