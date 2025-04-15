# Docker_and_AWS

# 🚀 AWS EKS, Docker, Lambda, SDK & CI/CD in Spring Boot Microservice
# 📸 Project Overview
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
## 🔨 Build & Run Docker 
```🐳 Dockerfile
FROM tomcat:10.1-jdk17
WORKDIR /user/local/tomcat/webapps/
COPY target/plantopia-0.0.1-SNAPSHOT.war plantopia.war
EXPOSE 8080
CMD ["catalina.sh","run"]

```bash
docker build -t plantopia .
```bash
docker run -p 8080:8080 plantopia
```
🧱 Docker Container Running images

## 🌱 Minikube & Kubernetes
### ⚙️ 1. Start Minikube
```bash
minikube start
minikube status
```
### 🚀 2. Load Your Spring Boot Docker Image into Minikube
Make sure you've built your Docker image:
```bash
# Build your JAR file
./mvnw clean package
# Build Docker image (from project root where Dockerfile is)
docker build -t plantopia-app .
```
Then load it into Minikube:
```bash
minikube image load plantopia-app
```
### ⚙️ 3. Set `kubectl` Context to Minikube
```bash
kubectl config use-context minikube
```
### 📝 4. Kubernetes YAML Files 
#### `deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: plantopia
spec:
  replicas: 2
  selector:
    matchLabels:
      app: plantopia
  template:
    metadata:
      labels:
        app: plantopia
    spec:
      containers:
      - name: plantopia
        image: plantopia:latest
        imagePullPolicy: Always
        ports:
        - containerPort: 8080
```
---
#### `service.yaml`
```yaml
apiVersion: v1
kind: Service
metadata:
  name: spring-boot-app-service
spec:
  type: NodePort
  selector:          
    app: plantopia
  ports:
    - port: 8080
      targetPort: 8080
      nodePort: 30007
```
```
### ☁️ 5. Apply Kubernetes Deployment and Service
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```
### ✅ 6. Check Pods
```bash
kubectl get pods
```
### 🖼 7. Get App URL (access from browser)
```bash
minikube service plantopia-service --url
```
### 📝 8. View Logs (for debugging)
```bash
kubectl logs <pod-name>
```
Get the pod name using:
```bash
kubectl get pods
```
---

## ☸️ AWS EKS & IAM
### ☁️ Step 1: Push Docker Image to Amazon ECR
1. **🛠Create ECR Repository**: Go to AWS Console > ECR > Create Repository.
2. **Authenticate Docker:**
```bash
aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
```
3. **Tag & Push Image**:
```bash
docker tag plantopia-app:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/plantopia-app:latest
docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/plantopia-app:latest
```
### 🌟 Step 2: Create Your EKS Cluster
**Using AWS Console**
- Go to **EKS > Clusters > Create Cluster**.
- Choose name (e.g., `plantopia-cluster`), IAM roles, VPC, subnets, etc.
- Create Node Group.
Create the cluster:
```bash
eksctl create cluster -f cluster-config.yaml
```
### 🔧 **Step 3: Configure kubectl**
```bash
aws eks --region us-east-1 update-kubeconfig --name plantopia-cluster
kubectl get nodes
```
### 🚢 **Step 4: Deploy Your Spring Boot App on EKS**
### 🌍 **Step 5: Access Your Spring Boot Application**
```bash
kubectl get svc
```
```
http://<external-ip>
```
### 🛡️ **Step 6: IAM Roles & Permissions**
1. **Create IAM Role for EKS**:
   - Attach: **AmazonEKSClusterPolicy**, **AmazonEKSWorkerNodePolicy**, **AmazonEC2ContainerRegistryReadOnly**.
2. **Configure EKS Node Role**:
   - Ensure EKS worker nodes have permission to pull images from ECR.
---

## 🌟 Lambda Function 
![Screenshot 2025-04-14 173418](https://github.com/user-attachments/assets/f873cf4f-030a-4645-bb6e-70538950d9f0)
### 📦 Step 1: Package Spring Boot Application 
1. **Create the Spring Boot JAR**:
   ```bash
   ./mvnw clean package
   ```
2. **Prepare for Lambda Deployment**:
   ```xml
   <dependencies>
       <dependency>
           <groupId>com.amazonaws.serverless</groupId>
           <artifactId>aws-serverless-java-container-springboot2</artifactId>
           <version>1.7.1</version>
       </dependency>
   </dependencies>
   ```
### 💻 Step 2: Zip Your Lambda Code 
1. **Create a Zip File** for your Lambda function code:
   ```bash
   # Package your Spring Boot app into a JAR
   ./mvnw clean package
   # Zip the JAR file for Lambda deployment
   zip -r function.zip target/plantopia-app.jar
   ```
### 🧪 Step 3: Test the Lambda Function 🧪
```bash
aws lambda invoke --function-name MyLambdaFunction output.txt
```
### 🛠 Step 4: IAM Roles & Permissions 
- `AWSLambdaBasicExecutionRole`
- `AWSLambdaVPCAccessExecutionRole`
### 🧹 Step 5: Clean Up Resources 
```bash
eksctl delete cluster --name plantopia-cluster
```
---

## 🌟 AWS SDK Integration in Spring Boot 

### 🛠️ Step 1: Add AWS SDK Dependencies in `pom.xml` 📝
### 🌍 Step 2: Configure AWS Credentials 🔑
   ```bash
   aws configure
   ```
Provide your **AWS Access Key ID**, **AWS Secret Access Key**, **region**, and **output format**.
### 🚀 Step 3: Create a Service Class to Use AWS SDK 🧑‍💻
Create a service class that interacts with **Amazon S3** and **DynamoDB**.
### ⚙️ Step 4: Set Up AWS SDK Configuration 🛠️
Create a configuration class to set up the **Amazon S3** and **DynamoDB** clients.
### 🧪 Step 5: Using AWS SDK in Your Controller 📱
Create a **REST controller** to test the AWS SDK integration.
### 🔐 Step 7: IAM Roles & Permissions 🔑
- **S3 Permissions:** `AmazonS3FullAccess`
- **DynamoDB Permissions:** `AmazonDynamoDBFullAccess`


## 🚀 CI/CD Pipeline (AWS CodePipeline + CodeBuild)
```
1. Code pushed to GitHub/CodeCommit.
2. Build triggered in **AWS CodeBuild** using `buildspec.yml`.
3. Docker image built and pushed to ECR.
4. Kubernetes manifest applied using `kubectl` inside CodeBuild.
5. Deployed live to AWS EKS 🚀
```
