🚀 vProfile Java WebApp Deployment on Kubernetes using kOps (AWS)
This project showcases the end-to-end deployment of a Java-based microservices application (vProfile) on an AWS-hosted Kubernetes cluster, set up using kOps.

🛠️ Tech Stack
Kubernetes (kOps)

AWS: EC2, S3, Route53, EBS

Docker, Docker Hub (private repo)

Ingress-NGINX Controller

Microservices: Tomcat (Java App), MySQL, RabbitMQ, Memcached

🧩 Architecture Overview

User → Domain (GoDaddy)
     → Route53 NS Records (AWS)
     → Load Balancer (Ingress NGINX)
     → Application Service (Tomcat)
     → vProApp Pod
       ├── RabbitMQ Service → RabbitMQ Pod
       ├── Memcached Service → Memcached Pod
       └── MySQL Service → DB Pod (PVC backed by AWS EBS)
📦 Components & Configurations
☸️ Kubernetes Cluster Setup
Created EC2 instance to install kOps, kubectl, and awscli

Created S3 bucket to store kOps cluster state

Configured Route53 Hosted Zone

Mapped GoDaddy domain (kubevpro.sasidevops.site) to Route53 NS records

🔐 IAM & Security
Created an IAM user for AWS CLI access

Created Kubernetes Secrets for:

MySQL credentials

RabbitMQ credentials

DockerHub private repo access

🗃️ Persistent Volumes
Used AWS EBS volume for MySQL

Applied a workaround using an initContainer to delete lost+found directory (which blocks DB init)

🐳 Containerization
Custom Docker images for:

vproapp (Tomcat app)

vprodb (MySQL)

Pushed to DockerHub (private)

Used official images for:

RabbitMQ

Memcached

📑 YAML Manifests
Deployments: vproapp, vprodb, vprormq, vpromc

Services: ClusterIP services for all microservices

Ingress: NGINX Ingress resource routing traffic to app service

Init Containers:

Wait for DNS resolution (nslookup) before app pod starts

Remove EBS unwanted lost+found file

🌐 Domain & Ingress
Domain: vprofile.sasidevops.site (GoDaddy)

Ingress Controller: nginx (with AWS NLB)

Ingress routes all traffic to the application service running on port 8080

✅ Final Result
Deployed and tested the app using the domain vprofile.sasidevops.site

Verified connectivity with:

RabbitMQ queue test

Memcached connection

MySQL data persistence with EBS

End-to-end working full-stack microservices architecture on Kubernetes

✨ Key Learnings
kOps cluster provisioning with S3 and Route53

Managing private DockerHub images in Kubernetes

Using initContainers for conditional startup and volume cleanup

Ingress Controller integration with NLB + external DNS

Full microservices app orchestration in Kubernetes
