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

# Step1: Create EKS Cluster using eksctl command
The above command created entire EKS Cluster which includes Fargate instances, VPC, private and public subnets, route tables, IAM roles.

![6](https://github.com/user-attachments/assets/2e55a340-7752-4c42-aeb9-e8ceed662328)



Below command is executed to update the kubeconfig file for your local Kubernetes client(kubectl) with the necessary configuration to connect to an EKS cluster named 'demo-cluster'. It allows you to interact with the EKS cluster using kubectl commands


![9](https://github.com/user-attachments/assets/e27f7562-9069-4465-a446-822527bee11a)


# Step2: Create Fargate profile with namespace game-2048
![10](https://github.com/user-attachments/assets/3a70f9b7-9bf2-47c4-a3a4-b377ddd06dc5)

It creates a fargate profile named alb-sample-app in the EKS cluster.


![11](https://github.com/user-attachments/assets/d6a05d8f-487a-44e1-b516-0736ac54d453)

# Step3: Deploy YAML configuration file (Deploy, Service, Ingress)
YAML file is created which defines Deployments, services, ingress required for application.

![13](https://github.com/user-attachments/assets/5ca4128c-c0a4-40ce-83e6-8c14b6fa1984)

Deployment.yaml:

- Created namspace game-2048
- Created Deployment in game-2048
- Created labels and selectors with the name 'app-2048'. Selector in service.yaml should match with selector(label) in deployment.yaml. Service will be able to discover pods using lebels and selectors.
- Defined the no of pod replicas for this deployment.
- Updated the image with latest application's docker image.

Service.yaml:

- Used namespace game-2048 for this service.
- Defined 'nodeport' service type mode.
- Target port is container port ( service) of pod

Ingress.yaml:

- In this case we want the ingress to be internet facing so that our application inside cluster can be by outside world.
- IngessClass name is alb

We can apply this file configurations to create deployment, service and Ingress resource for 2048 game application using 'kubectl-apply'command.

![12](https://github.com/user-attachments/assets/13dc68d7-b5df-4c2a-b415-a101cd3c11f6)

All the above configurations are applied.

![14](https://github.com/user-attachments/assets/28ed1acb-777c-4e64-a05a-7ee18b0b5567)



![15](https://github.com/user-attachments/assets/9455cf59-8c4e-4a16-b2fa-d5fc072184a2)

Here we do not have the address in the Ingress resource to access it from outside world, this is because we didn't configure Ingress Controller.Ingress controller reads Ingress resource and creates Application LB with target groups and ports.
If external user is trying to access LB, then ingress controller reads the ingress resource & whenever it finds matching rules it will forward request to service-2048. It will then forward the request to Delpoyment. 

Before deploying the ingress contoller(ALB) we need to create IAM OIDC provider to grant access to AWS account so that ALB within AWS account can be integrated to gain access.












  





