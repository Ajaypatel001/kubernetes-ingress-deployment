# 🚀 Multi-Cloud Docker & Kubernetes Deployment using AWS EKS

This project demonstrates a complete **end-to-end DevOps deployment** using **Docker, Kubernetes (EKS), AWS ECR, Nginx Ingress, and Route 53**.  
The application is containerized, pushed to ECR, deployed on Kubernetes, and exposed to the internet using an AWS Load Balancer and custom domain.

---

## 📌 Project Overview

In this project, I deployed an application on **AWS EKS** by:
- Creating and configuring an EC2 instance
- Setting up IAM roles and security groups
- Installing Docker, Git, and Kubernetes tools
- Using AWS ECR for Docker images
- Deploying the application using Kubernetes
- Exposing it with Nginx Ingress and AWS Load Balancer
- Connecting a custom domain using Route 53

---

## 🛠️ Tools & Technologies Used

- AWS EC2  
- AWS EKS  
- AWS ECR  
- IAM Roles  
- Security Groups  
- Docker  
- Kubernetes (kubectl)  
- Nginx Ingress Controller  
- Route 53  
- Git & GitHub  

---

## 🧱 Architecture Flow

1. EC2 instance creation and configuration  
2. EKS cluster setup  
3. Docker image build  
4. Push image to AWS ECR  
5. Kubernetes deployment using ECR images  
6. Ingress configuration with Load Balancer  
7. Domain mapping using Route 53  

---

## ⚙️ Detailed Setup & Deployment Steps

---

## 1️⃣ EC2 Instance Setup

- Created an **EC2 instance**
- Attached **IAM Role** with EKS, ECR, and EC2 permissions
- Configured **Security Group** (ports 22, 80, 443)
- Connected to EC2 using SSH

```bash
ssh -i key.pem ec2-user@<EC2_PUBLIC_IP>
