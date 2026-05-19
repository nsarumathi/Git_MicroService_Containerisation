# Git_MicroService_Containerisation

Overview:
---------------------------------------
This document provides details on setup & testing various services after running the docker-compose file. These services include User, Product, Order, and Gateway Services. Each service has its own endpoints for testing purposes.

---
# Sections:

A. Local Containerization Using Docker Compose (Without Jenkins)
B.  Containerization with Jenkins CI/CD & Kubernetes Deployment
---
## Compartment 1: Local Containerization Using Docker Compose (Without Jenkins)

This section covers manual setup, Dockerfile creation, and multi-container orchestration using Docker Compose on an AWS EC2 instance.

Services and Endpoints:
----------------------------------------

<img width="1166" height="97" alt="image" src="https://github.com/user-attachments/assets/847a2010-630d-4d23-b02b-44d4a90b0d1c" />

**Gateway Service:**

<img width="429" height="97" alt="image" src="https://github.com/user-attachments/assets/63bf976d-3154-459d-92f4-9b4c3280d0fa" />

Environmental Setup:
---------------------------
1.Launch EC2 instacne with Ubuntu 22.04 version

<img width="1918" height="843" alt="image" src="https://github.com/user-attachments/assets/ef6c7271-6443-4c70-9449-5752ebd4a197" />

2.Installed Docker Engine with Ubuntu OS

3.Clone Git Repository ** https://github.com/mohanDevOps-arch/Microservices-Task.git**

4.Navigate to each services folder 
    cd MicroServices-Task/Microservices for 4 services  ( User-Service,Product-service,Gateway-Service,Orders-Service)
    
5.Create Docker file for 4 services with respective port ( User-Service :3000 ,Product-service:3001 ,Gateway-Service:3003,Orders-Service:3002)

      FROM node:20-slim
      WORKDIR /app
      COPY package*.json ./
      RUN npm install --production
      COPY . .
      EXPOSE {{port_no}}
      CMD [ "node", "app.js" ]
      
6.Create docker-compose.yml inside Microservice folder  (cd MicroServices-Task/Microservices)

       version: "3.9"
       services:
          user-service:
            build:
              context: ./user-service
            container_name: user_service
            ports:
              - "3000:3000"
            networks:
              - app-network
        
          product-service:
            build:
              context: ./product-service
            container_name: product_service
            ports:
              - "3001:3001"
            networks:
              - app-network
        
          order-service:
            build:
              context: ./order-service
            container_name: order_service
            ports:
              - "3002:3002"
            networks:
              - app-network
        
          gateway-service:
            build:
              context: ./gateway-service
            container_name: gateway_service
            ports:
              - "3003:3003"
            depends_on:
              - user-service
              - product-service
              - order-service
            networks:
              - app-network
        
        networks:
          app-network:
            driver: bridge
6.Run docker-compose up --build

   <img width="1622" height="853" alt="Docker-cmpose-run" src="https://github.com/user-attachments/assets/3c790bbf-c434-4eaf-8737-2247f9cc9ad1" />'
   
7.Containers List & Docker Bridge Network

<img width="1620" height="497" alt="Docker_images" src="https://github.com/user-attachments/assets/8a72d454-2871-4ed5-91ec-1e4f62fa07ed" />

**Network:** microservices_app-network

<img width="1623" height="883" alt="Dockernetwork1" src="https://github.com/user-attachments/assets/ad369ac4-f5e2-49df-9ba6-9e5d8c4966e6" />

<img width="1617" height="873" alt="Dockernetwork2" src="https://github.com/user-attachments/assets/739bc974-501d-448d-ba0b-92314bdf59e0" />

8.EndPoints Testing

Gateway Service:
-----------------------------------
**Gateway health :**

<img width="1167" height="251" alt="gateway_health" src="https://github.com/user-attachments/assets/08ee962e-783d-4003-aaab-00b8578c7ae2" />

**Gateway users:**

<img width="710" height="242" alt="gateway_users" src="https://github.com/user-attachments/assets/e0bcd386-b3f5-4e4c-a59f-3765942594bf" />

**Gateway products:**

<img width="828" height="307" alt="gateway_products" src="https://github.com/user-attachments/assets/2d74cde6-cfc0-4bc8-a96f-9d135ce6d4d0" />

**Gateway orders:**

<img width="693" height="268" alt="gateway_orders" src="https://github.com/user-attachments/assets/a258df90-c764-4d4f-99e9-74e42c51264a" />

User Service:
---------------------------------------
<img width="817" height="347" alt="Users_List_EC2" src="https://github.com/user-attachments/assets/ef8e7ad0-de80-4ca1-9ee9-96937ce8d1a6" />

Products Service:
---------------------------------------
<img width="900" height="256" alt="products_list" src="https://github.com/user-attachments/assets/5543fb83-93d9-43ed-8ad1-e016ef0c8a0d" />

Orders Service:
---------------------------------------
<img width="980" height="310" alt="orders_list" src="https://github.com/user-attachments/assets/b8dd9498-1ae4-4262-86ef-0163123f657b" />

---
## Compartment 2: Containerization with Jenkins CI/CD & Kubernetes Deployment

This section details the automated CI/CD pipeline implementation using Jenkins, pushing images to a container registry, and orchestrating the microservices using Kubernetes (K8s).

Services:
----------
<img width="999" height="73" alt="image" src="https://github.com/user-attachments/assets/90eb92e7-4cc6-406b-b334-1b7d800048ed" />

<img width="525" height="97" alt="image" src="https://github.com/user-attachments/assets/5d36003a-66a5-41d2-bfa1-fe492ea3cc57" />


Environmental Setup:
---------------------------
1.Docker & Jenkins Server Setup

  # Install Docker
  # Install Jenkins
  # Enable Jenkins
  <img width="1700" height="880" alt="Jenkins" src="https://github.com/user-attachments/assets/4ab6ea28-e005-4471-9369-b6583b360e5f" />


2.CI/CD Pipeline Job 

   Configure jenkins Job (to push images to ECR using jenkinsfile)
   <img width="1702" height="960" alt="JobConfig" src="https://github.com/user-attachments/assets/6cd61d99-8655-46c1-ba0b-5ef8324b6f7b" />
   <img width="1918" height="877" alt="Job_success" src="https://github.com/user-attachments/assets/bc4084c9-dadc-4f53-b668-affa503eca97" />

   Add Github WEbhook

3.ECR Setup:
  Create private ecr repo for 4 services
  <img width="1918" height="602" alt="ECR_AfterDocker" src="https://github.com/user-attachments/assets/f451551b-49ba-4a80-9a8f-e6e3274ab426" />

  once build is successful:
  <img width="1918" height="467" alt="ECR_Bfr" src="https://github.com/user-attachments/assets/d4f39938-9168-46d2-b040-591a3e521016" />


4.k8s & Minikube setup

# Install k8s
    curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.29.0/2024-01-04/bin/linux/amd64/kubectl
    chmod +x ./kubectl
    sudo mv ./kubectl /usr/local/bin
    kubectl version --client
# Install Minikube
    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
    sudo install minikube-linux-amd64 /usr/local/bin/minikube
    rm minikube-linux-amd64
    minikube version
# Start Minikube by providing docker daemon access to ubuntu user
    sudo usermod -aG docker $USER
    newgrp docker
    minikube start
# Add ingress on cluster 
    minikube addons enable ingress
    <img width="1918" height="782" alt="K8S_MinikubeInstall" src="https://github.com/user-attachments/assets/677d000f-f104-43f0-b444-e50f68292008" />
<img width="1918" height="782" alt="K8S_MinikubeInstall" src="https://github.com/user-attachments/assets/fa4d4ceb-2f7c-47af-85f3-94a3428a989e" />


5.Create kubernetes manifest as per submission folder & apply it
    **
    submission/
    ├── deployments/
    │   ├── user-service.yaml
    │   ├── product-service.yaml
    │   ├── order-service.yaml
    │   └── gateway-service.yaml
    ├── services/
    │   ├── user-service.yaml
    │   ├── product-service.yaml
    │   ├── order-service.yaml
    │   └── gateway-service.yaml
    ├── ingress.yaml  
    **

# To access private ECR
        kubectl create secret docker-registry ecr-secret \
          --docker-server=944765969321.dkr.ecr.ap-south-2.amazonaws.com \
          --docker-username=AWS \
          --docker-password=$(aws ecr get-login-password --region ap-south-2)
        
        kubectl get secret ecr-secret
   <img width="1087" height="786" alt="Deploy_files" src="https://github.com/user-attachments/assets/e9f36075-3b09-426a-8fbd-581ca3fa1024" />

# Deploy application

    kubectl apply -f deployments/
    kubectl apply -f service/
    kubectl apply -f ingress.yaml
    kubectl get all
    
    <img width="771" height="813" alt="deploy_verify" src="https://github.com/user-attachments/assets/a8673525-b95f-4b74-baa3-234047485846" />

6.Cluster-Internal testing

    kubectl run alpine-test --rm -it --image=alpine -- sh  
    # 1. Update packages and install curl
            apk add --no-cache curl   
    # 2. Test your internal services & gateway services inside cluster
    
    <img width="1071" height="796" alt="Testing_Internal" src="https://github.com/user-attachments/assets/d07f8d3d-e63b-427a-b95d-f55181350234" />










