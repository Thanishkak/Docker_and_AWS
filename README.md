# Docker_and_AWS

# 🚀 Spring Boot Microservice on AWS EKS with Docker, Lambda, SDK & CI/CD
📸 Project Overview
```
A backend project built with Spring Boot and deployed using Docker, Kubernetes, and AWS EKS, with integrated AWS Lambda for serverless functions, the AWS SDK for cloud service interaction, and an automated CI/CD pipeline for seamless deployments.

This microservice demonstrates a cloud-native architecture utilizing:
- Spring Boot for backend service logic
- Docker for containerization
- Kubernetes on AWS EKS for orchestration and scaling
- AWS Lambda for event-driven serverless tasks
- AWS SDK for integrating with AWS service S3.
- CI/CD pipeline with AWS CodePipeline & CodeBuild

```
## 🧰 Tech Stack
```
| Technology      | Purpose                                         |
|-----------------|-------------------------------------------------|
| Spring Boot     | RESTful API backend                             |
| Docker          | Containerize the application                    |
| Kubernetes (EKS)| Deployment and orchestration on AWS             |
| AWS Lambda      | Serverless functions for async tasks            |
| AWS SDK         | Access AWS services from Java code              |
| CI/CD Pipeline  | Automate build & deploy via AWS CodePipeline    |

```
🔨 Build & Run
```
🐳 Step 1: Dockerize the Application

```bash
docker build -t plantopia .
```bash
docker run -p plantopia

The application will be accessible at http://localhost:8080.
```

### ⚙️ Step 2: Push Docker Image to ECR

```bash
docker tag springboot-eks-app <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/springboot-eks-app
docker push <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/springboot-eks-app
```


## ☁️ Kubernetes Deployment (AWS EKS)

### 🛠 Step 1: Create EKS Cluster

Created using `eksctl` or AWS Console:
```bash
eksctl create cluster --name my-cluster --region us-east-1 --nodes 2 --node-type t3.medium
```

### ⚙️ Step 2: Apply Kubernetes Manifests

```bash
kubectl apply -f deployment/deployment.yaml
kubectl apply -f deployment/service.yaml
```

> ✅ Your Spring Boot app is now running on AWS EKS!

---

## 🖼 Screenshots

### 🧱 Docker Container Running
![Docker](screenshots/docker.png)

### ☸️ EKS Deployment
![EKS Pods](screenshots/eks-pods.png)

### 📦 Lambda Function Setup
![Lambda](screenshots/lambda.png)

### 📂 CI/CD Pipeline in Action
![CI/CD](screenshots/cicd-pipeline.png)

---

## 📦 AWS Lambda Integration

Lambda 

## 🔌 AWS SDK Integration

Used AWS SDK (v2) to connect with AWS services directly from the app.

```java
S3Client s3 = S3Client.builder().region(Region.US_EAST_1).build();
s3.putObject(PutObjectRequest.builder().bucket("my-bucket").key("file.txt").build(), Paths.get("file.txt"));
```

---

## 🚀 CI/CD Pipeline (AWS CodePipeline + CodeBuild)
```
1. Code pushed to GitHub/CodeCommit.
2. Build triggered in **AWS CodeBuild** using `buildspec.yml`.
3. Docker image built and pushed to ECR.
4. Kubernetes manifest applied using `kubectl` inside CodeBuild.
5. Deployed live to AWS EKS 🚀
```

## 🧠 Learnings
```
✅ How to deploy and scale microservices using AWS EKS
✅ CI/CD automation with AWS native tools
✅ Best practices in cloud-native application design
```

## 🙌 Special Thanks
```
To AWS documentation, the Spring community, and all open-source contributors whose tools made this project possible.
```
