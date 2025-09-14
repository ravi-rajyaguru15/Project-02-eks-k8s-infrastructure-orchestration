#  Project 2: Kubernetes Deployment of Multi-Service Java App on AWS EKS

[![EKS](https://img.shields.io/badge/Platform-AWS_EKS-blue)](#)
[![Kubernetes](https://img.shields.io/badge/Orchestration-Kubernetes-informational)](#)
[![Status](https://img.shields.io/badge/Deployment-Complete-brightgreen)](#)

---

##  Part of a 3-Project DevOps Progression

This project is the **second milestone** in a deliberately structured, three-part DevOps transformation system designed to reflect how modern applications evolve from basic containerization to full infrastructure automation and observability.

| Stage       | Project                     | Focus                                |
|-------------|-----------------------------|--------------------------------------|
| 🚢 Project 1 | Docker Compose on AWS EC2   | Containerization + Deployment        |
| ☸️ Project 2 | Kubernetes on AWS EKS       | Orchestration + Declarative Infra    |
| ⚙️ Project 3 | Terraform + CI/CD + Observability | Automation + Monitoring + Delivery |

---

##  Project Overview

This project transitions a previously containerized, multi-service Java web application (from Project 1) into a **modular, production-style Kubernetes deployment** on AWS. The focus here is on orchestration, service networking, persistent storage, and secure credential handling — all managed via **declarative Kubernetes manifests**.

The infrastructure runs entirely on **AWS Elastic Kubernetes Service (EKS)**, provisioned using `eksctl` with built-in support for IAM roles and EBS CSI drivers. Custom Docker images for the core services are built and pulled from Docker Hub, while service dependencies are tightly managed using `initContainers`. The app is publicly exposed using an **NGINX Ingress Controller**, integrated with AWS load balancing.

This project showcases a real-world shift from local container coordination (Docker Compose) to **cloud-native service orchestration** on Kubernetes, including ownership of the deployment lifecycle, image pipelines, storage, and networking.

Summary:

- Migrates a previously containerized Java web application to **Kubernetes**
- Deploys to a **managed EKS cluster** using `eksctl`
- Implements **multi-service orchestration**, including:
  - Web frontend
  - MySQL (PVC-backed)
  - RabbitMQ (messaging)
  - Memcached (caching)
- Uses **NGINX Ingress**, **AWS IAM**, and **EBS CSI Driver**
- All deployments are fully declarative via **YAML manifests**


---
##  Application Stack

| Layer      | Component        | Purpose                          |
|------------|------------------|----------------------------------|
| Web        | `web-app`        | Java Spring Boot app via Tomcat  |
| Database   | `mysql`          | PVC-backed relational DB         |
| Messaging  | `rabbitmq`       | Queue-based messaging            |
| Caching    | `memcached`      | In-memory key-value store        |
| Ingress    | `nginx`          | HTTP routing & reverse proxy     |

---

##  Infrastructure Overview

| Component      | Role                                                       |
|----------------|------------------------------------------------------------|
| **EKS**        | Fully managed Kubernetes cluster                           |
| **EC2**        | Worker nodes for running pods                              |
| **EBS**        | Persistent backend for MySQL PVC                           |
| **S3**         | Mounted via CSI driver for long-term MySQL storage         |
| **IAM Role**   | Grants access to EBS/S3 storage classes                    |
| **ELB**        | Provisioned via Ingress for public web access              |

---

## Architecture Overview
![Project 2 architecture](images/architecture/project-2-architecture.png)

---

## Repository Structure
```text
Project_02/
│      
├── kubernetes/
│   ├── deployments/                               # Pods for app, mysql, rabbitmq, memcached and initContainers
│   │   ├── app-deployment.yaml
│   │   ├── mysql-deployment.yaml
│   │   ├── rabbitmq-deployment.yaml
│   │   └── memcached-deployment.yaml
│   │
│   ├── services/                                  # ClusterIP services      
│   │   ├── web-app-service.yaml
│   │   ├── mysql-service.yaml
│   │   ├── rabbitmq-service.yaml
│   │   └── memcached-service.yaml
│   │
│   ├── ingress/                                   # NGINX ingress resource
│   │   └── nginx-ingress.yaml
│   │
│   ├── secrets/                                   # Base64 encoded secrets containing MySQL and RabbitMQ credntials
│   │   └── secrets.yaml                                
│   │
│   └── persistentVolumeClaim/                     # MySQL PVC
│       └── mysql-pvc.yaml
│      
├── images/
|   ├── screenshots/...                            
│   └── architecture/
|         └── project-2-architecture.png 
│      
├── legacy/                                        # Files and artifacts carried over from project 1 for continuity 
│   ├── app/...                                      purposes, but are not an active part of project 2. 
│   ├── mysql/... 
│   ├── nginx/...
│   ├── images/...                         
│   ├── .env                                                                           
│   └── docker-compose.yaml  
│                                 
├── project2-eksctl-config.yaml                    # Cluster definition configuration file for eksctl     
└── README.md                                      # Project README (You are here) (Inception!)
```

## How to Deploy to AWS EC2
1. Provision an EC2 instance (Example: Ubuntu Server 24.04 LTS, t2.medium, 25 GB gp storage), with a key-pair login and appropriate security group.
2. Login to the EC2 instance via SSH using generated key.  
3. Install Docker and Docker Compose.  
4. Copy the project files to the EC2 instance via git clone, and navigate into the project folder.   
5. Run:
    ```bash
    docker compose up --build -d
    ```
6. Verify running containers:
    ```bash
    docker ps
    ```
7. Access the application using the EC2 Public IP: 
    ```bash
    http://<EC2-public-IP>:80
    ```
8. After verification of web app, stop and clean the docker compose services:
    ```bash
    docker-compose down -v --rmi all
    ```
9. Terminate the EC2 instance if not needed to avoid incurring unnecessary AWS costs.    

## Key Features
- Kubernetes orchestration of 4 production-style services
- Secure credential injection via K8s Secrets
- Persistent storage via EBS (PVC-bound)
- Public ingress routing via AWS ELB
- Modular manifest structure (prod-aligned)
- Debugged real-world issues:
     - CrashLoopBackOff
     - IAM binding failures
     - PVC mount errors


## Engineering Decisions
- Custom Docker images pushed to Docker Hub (web-app, mysql)
- IAM role + EBS CSI driver configured in eksctl YAML
- initContainers used to:
     - Wait for RabbitMQ, MySQL, Memcached readiness
     - Clean lost+found/ in EBS volumes (MySQL PVC)
- Manual Ingress install via YAML (no Helm)
- YAML-first approach — every manifest written by hand

## What This Project Demonstrates
- Kubernetes orchestration of 4 production-style services   
- Secure credential injection via K8s Secrets
- Persistent storage via EBS (PVC-bound)
- Public ingress routing via AWS ELB
- Modular manifest structure (prod-aligned)
- Debugged real-world issues:
     - CrashLoopBackOff
     - IAM binding failures
     - PVC mount errors

## Attribution
The Java application used in this project was externally sourced. All containerization, orchestration, deployment strategy, and infrastructure setup were independently implemented. Although it being way out of scope, the application was modified to remove course branding and to be presented as a general-purpose internal platform. The main goal was to practice and demonstrate key DevOps skills, in this case, orchestration and deployment of multi-service web application on AWS EKS cloud infrastructure.