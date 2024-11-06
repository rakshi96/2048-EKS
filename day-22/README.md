 # 2048 Application Deployment on EKS Cluster

This project explains how to deploy 2048 game Application on Amazon EKS by providing access through Load Balancer, and exposing it to real world using Ingress and Ingress Controller (ALB controller)

![26](https://github.com/user-attachments/assets/3602cdbe-6b7b-4c6a-869a-7f26555c8497)

Prerequisites: 
- kubectl
- eksctl
- AWS CLI

Components and Tools used:
- EKS Cluster: AWS managed Kubernetes service that simplifies the deployment, management, and scaling of containerized applications.
- Fargate Profile: Defines the configuration for running Kubernetes pods on AWS Fargate, allowing for serverless compute without managing EC2 instances.
- Ingress: It manages the external access to Kubernetes cluster such as HTTP or HTTPS traffic.
- Ingress Controller: It is a Kubernetes Controller that manages and implements the rules defined in Ingress resources to route external traffic to the appropriate 
  services.
- IAM OIDC Provider: It is an Identity provider allows AWS to authenticate users or services using OpenID Connect (OIDC) tokens.
- Deployment & Service: Deploy.yaml and Service.yaml file is created which defines deployments, services, labels and selectors.
  





