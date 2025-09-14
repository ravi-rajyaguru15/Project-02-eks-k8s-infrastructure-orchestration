# Multi-Service Java Web Application Deployment with Docker Compose on AWS EC2

## About the 3-Project DevOps System
This project is the foundational stage of a self-designed 3-part DevOps portfolio projects designed to mirror the progression of infrastructure maturity in real-world engineering environments — from containerization, to orchestration, to full automation and observability.

- **Project 1 (this)**: Multi-service containerization and deployment using Docker Compose on AWS EC2  
- **Project 2**: Kubernetes orchestration of the same application stack on AWS EKS  
- **Project 3**: Infrastructure-as-Code with Terraform, CI/CD via GitHub Actions, and monitoring using Prometheus and Grafana

The Java web application used in this project was externally sourced. However, to ensure the system appeared more production-ready and portfolio-appropriate, all course-specific branding was removed. UI elements and presentation were modified to reflect a generic, open-source-style internal platform. This allowed full attention to be directed toward infrastructure, deployment, and DevOps process engineering.

## Project Overview
This project demonstrates the deployment of a multi-tier Java web application using Docker Compose on a single EC2 instance in AWS. It includes five interdependent containers: Tomcat (web app), Maven (build), Nginx (reverse proxy), MySQL (database), RabbitMQ (message queue), and Memcached (cache).

The objective was to simulate a real-world, multi-container deployment in a controlled, reproducible environment. This is the foundational project in a 3-stage portfolio demonstrating a full DevOps transformation.

## Architecture Overview
![Project 2 architecture](images/architecture/project-2-architecture.png)

## Technologies Used
- AWS EC2 (Ubuntu server)  
- Docker  
- Docker Compose  
- Nginx (reverse proxy)  
- Tomcat (Java application server)  
- Maven (build container)  
- MySQL (database)  
- RabbitMQ (message queue)  
- Memcached (in-memory caching)

## Repository Structure
```text
Project_01/
├── app/
│   ├── Dockerfile                              # Multi-stage Dockerfile (Maven -> Tomcat)
│   ├── src/                                    # Java web app source files
│   └── pom.xml                                 # Maven project file
├── mysql/
│   ├── Dockerfile                              # MySQL Dockerfile
│   └── db_backup.sql                           # .sql file to seed the MySQL database
├── nginx/
│   └── default.conf                            # Reverse proxy configuration
├── images/
|   ├── screenshots/                            # Screenshots of running services, EC2 instance, web app on EC2 public IP, etc.  
│   └── architecture/
|         └── project-1-architecture.png        # Architecture overview diagram of Project 1
├── .env                                        # Environment file to store credentials for MySQL and RabbitMQ
├── docker-compose.yaml                         # Docker compose file that defines 5 services
└── README.md                                   # Project README (You are here) (Inception!)
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
- Multi-container orchestration with Docker Compose  
- Containerized build process using Maven  
- Reverse proxy routing with Nginx  
- Dynamic runtime configuration using environment variables via `.env`  
- Inter-container communication using a shared Docker network  
- Production-style deployment on AWS EC2

## Engineering Decisions
- Chose Docker Compose to manage service dependencies and simplify orchestration of multiple containers on a single host  
- Used Maven as a standalone build container to ensure a reproducible `.war` file generation during deployment  
- Applied Nginx as a reverse proxy for abstraction and better request routing to the Tomcat container  
- Migrated hardcoded credentials (e.g., MySQL, RabbitMQ) into environment variables via a `.env` file to support better configuration management  
- Explicitly declared Docker networks and service dependencies in `docker-compose.yml` to control container startup order and network resolution  
- Isolated persistent data volumes where applicable for database containers
- Used real AWS infrastructure to simulate real-world cloud provisioning — this was not done locally to maximize cloud readiness and practice debugging on real infrastructure.

## What This Project Demonstrates
- Foundational understanding of container-based application architecture  
- Practical use of Docker Compose for service orchestration  
- AWS EC2 provisioning and deployment flow  
- Secure handling of environment-specific configurations using `.env`  
- Hands-on deployment of a stateful, multi-tier web app  
- Reproducible build pipeline within a cloud-hosted Linux environment

## Attribution
The Java application used in this project was externally sourced. All containerization, orchestration, deployment strategy, and infrastructure setup were independently implemented. Although it being way out of scope, the application was modified to remove course branding and to be presented as a general-purpose internal platform. The main goal was to practice and demonstrate key DevOps skills, in this case, containerization of multi-service web application and deplyment on cloud infrastructure.