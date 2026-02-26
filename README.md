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


===============================================
kubernetes-ingress-deployment – ALL COMMANDS
===============================================

------------------------------
1. CONNECT TO EC2
------------------------------

ssh -i key.pem ec2-user@<EC2_PUBLIC_IP>

------------------------------
2. UPDATE SYSTEM
------------------------------

sudo yum update -y

------------------------------
3. DOCKER INSTALLATION
------------------------------

sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ec2-user
newgrp docker
docker --version

------------------------------
4. GIT INSTALL & CLONE REPO
------------------------------

sudo yum install git -y
git --version
git clone <your-github-repo-url>
cd <project-folder>

------------------------------
5. KUBECTL INSTALL
------------------------------

curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.27.1/2023-04-19/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client

------------------------------
6. AWS CLI CONFIGURE
------------------------------

aws configure

------------------------------
7. CONNECT KUBECTL WITH EKS
------------------------------

aws eks update-kubeconfig --region <region> --name <cluster-name>
kubectl get nodes

------------------------------
8. CHECK NGINX INGRESS
------------------------------

kubectl get pods -n ingress-nginx

------------------------------
9. INSTALL NGINX INGRESS (IF NOT PRESENT)
------------------------------

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/aws/deploy.yaml

kubectl get svc -n ingress-nginx

NOTE:
If ingress-nginx is not installed,
AWS LoadBalancer will NOT be created.

------------------------------
10. CREATE AWS ECR REPOSITORIES
------------------------------

aws ecr create-repository --repository-name app-aws
aws ecr create-repository --repository-name app-azure
aws ecr create-repository --repository-name app-gcp
aws ecr create-repository --repository-name app-docker

------------------------------
11. LOGIN TO AWS ECR
------------------------------

aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com

------------------------------
12. DOCKER IMAGE BUILD
------------------------------

docker build -t app-image .

------------------------------
13. TAG & PUSH IMAGE TO ECR
------------------------------

docker tag app-image:latest <ECR_URI>:latest
docker push <ECR_URI>:latest

(Repeat build, tag, push for all 4 ECR repos)

------------------------------
14. UPDATE KUBERNETES FILES
------------------------------

cd k8s-files

(Change image name in all deployment.yaml files)
image: <account-id>.dkr.ecr.<region>.amazonaws.com/app-aws:latest

------------------------------
15. DEPLOY TO KUBERNETES
------------------------------

kubectl apply -f .

------------------------------
16. VERIFY DEPLOYMENT
------------------------------

kubectl get pods
kubectl get svc
kubectl get ingress

------------------------------
17. ROUTE 53 DOMAIN SETUP
------------------------------

Create Hosted Zone
Create Record
Point domain to LoadBalancer DNS

------------------------------
18. UPDATE INGRESS HOST
------------------------------

host: app.example.com

kubectl apply -f ingress.yaml

------------------------------
19. TROUBLESHOOTING
------------------------------

kubectl describe ingress
kubectl logs -n ingress-nginx -l app.kubernetes.io/name=ingress-nginx

------------------------------
FINAL RESULT
------------------------------

LoadBalancer DNS -> 404 ERROR
Route53 Domain   -> APPLICATION RUNNING
