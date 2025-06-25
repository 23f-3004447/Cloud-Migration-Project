# Kimai Cloud Migration Project

## 📦 Project Overview

This project focuses on migrating the **Kimai** time-tracking application to a cloud-native architecture using **AWS**, **Terraform**, **Docker**, and **Jenkins**, with a robust CI/CD pipeline, secure access, and full observability.

---

## 🔧 Tech Stack

| Component        | Tech                            |
| ---------------- | ------------------------------- |
| Backend          | PHP (Symfony Framework)         |
| Frontend         | HTML/CSS/JS (Bundled)           |
| DB               | MariaDB / MySQL                 |
| Server           | Nginx                           |
| Runtime          | PHP-FPM                         |
| OS               | Amazon Linux 2 (EC2)            |
| IaC              | Terraform                       |
| Containerization | Docker                          |
| CI/CD            | Jenkins                         |
| Monitoring       | CloudWatch, Prometheus, Grafana |
| Logging          | CloudWatch Logs                 |
| Access Control   | AWS IAM, Bastion Host           |

---

## 🚀 Deployment Architecture

### Textual Architecture Diagram

```
[ User ]
   |
[ Load Balancer (Public) + WAF ]
   |
[ Bastion Host (Public Subnet) ]
   |
[ VPC ]
 ├── EC2 (Kimai + Docker) [Private Subnet]
 └── RDS (MySQL DB)       [Private Subnet]
```

### Security Highlights:

* Bastion host for secure SSH
* IAM with least privilege
* Security Groups with only required ports open
* ALB protected by AWS WAF

---

## 📁 Repository Structure

```
Cloud-Migration-Project/
├─ terraform/
│   ├─ main.tf
│   ├─ variables.tf
│   └─ ec2_module/...
├─ Jenkinsfile
├─ Dockerfile
├─ docker-compose.yml (for local dev)
├─ docs/
│   ├─ high-level-design.md
│   └─ low-level-design.md
└─ README.md
```

---

## ⚙️ Terraform Setup

* Modular setup for reusability
* Remote state managed in **S3**
* Uses **t3.large** for Kimai EC2 and **t2.micro** for Bastion Host
* Outputs public/private IPs, instance IDs

To deploy:

```bash
cd terraform
terraform init
terraform apply -auto-approve
```

---

## 🐳 Docker Setup

Kimai is containerized with a multi-stage Dockerfile for optimized builds. The container is:

* Non-root
* Has healthcheck
* Ready for production

To build locally:

```bash
docker build -t kimai-app .
docker run -p 80:8001 kimai-app
```

---

## 🔁 CI/CD Pipeline (Jenkins)

Trigger: Push to `main` branch

Stages:

* Checkout
* Build Docker Image
* Run Tests
* Push to DockerHub
* Deploy via SSH to EC2

Pipeline is fully automated using a `Jenkinsfile`.

---

## 🔒 Security

* **WAF** filters traffic on Load Balancer
* **Bastion Host** enables safe key-based access to private instances
* **IAM Role** attached to Kimai EC2 for CloudWatch agent and S3 access

---

## 📊 Monitoring & Logging

* **CloudWatch Agent** installed and configured on EC2
* Monitors:

  * CPU usage
  * Memory and Disk space
  * Application logs
* Alerts configured for:

  * High CPU (> 80%)
  * Low Disk Space (< 20% free)
  * Health Check failures

---

## 💵 Cost Estimation (Monthly)

| Resource                | Cost                |
| ----------------------- | ------------------- |
| EC2 (t3.large)          | \~\$33.28           |
| Bastion Host (t2.micro) | \~\$7.62            |
| EBS (28GB total)        | \~\$2.80            |
| S3 (backups + state)    | \~\$0.12            |
| CloudWatch              | \~\$2.00            |
| **Total**               | **\~\$45–48/month** |

---

## 📄 Setup Guide

For full Kimai EC2 setup with PHP, Nginx, MariaDB, permissions, .env, database, admin user, Nginx config, cache clear, firewall — refer to the [Setup Guide](docs/low-level-design.md)

---

## 📌 Notes for Teams

* Use Git to always pull latest changes
* All setup scripts are idempotent
* Logs and backups go to AWS
* Easily replicable using Terraform anywhere

---

## 📑 Final Checklist

* ✅ High-Level & Low-Level Design in `/docs`
* ✅ Terraform infra with S3 remote state
* ✅ CI/CD with Jenkins
* ✅ Dockerized Kimai app
* ✅ Secure via Bastion + IAM + WAF
* ✅ Monitoring and alerts with CloudWatch
* ✅ Cost Estimate and Final Report generated

---

Made with ☁️ by Angeline & Team
