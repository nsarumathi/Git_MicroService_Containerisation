# Git_MicroService_Containerisation

Overview:
---------------------------------------
This document provides details on setup & testing various services after running the docker-compose file. These services include User, Product, Order, and Gateway Services. Each service has its own endpoints for testing purposes.

Services and Endpoints:
----------------------------------------

<img width="1166" height="97" alt="image" src="https://github.com/user-attachments/assets/847a2010-630d-4d23-b02b-44d4a90b0d1c" />

**Gateway Service:**

<img width="429" height="97" alt="image" src="https://github.com/user-attachments/assets/63bf976d-3154-459d-92f4-9b4c3280d0fa" />

Setup:
---------------------------
1.Launch EC2 instacne with Ubuntu 22.04 version

2.Installed Docker Engine with Ubuntu OS

3.Clone Git Repository https://github.com/mohanDevOps-arch/Microservices-Task.git

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
Gateway health :
<img width="1167" height="251" alt="gateway_health" src="https://github.com/user-attachments/assets/08ee962e-783d-4003-aaab-00b8578c7ae2" />

Gateway users:
<img width="710" height="242" alt="gateway_users" src="https://github.com/user-attachments/assets/e0bcd386-b3f5-4e4c-a59f-3765942594bf" />

Gateway products:
<img width="828" height="307" alt="gateway_products" src="https://github.com/user-attachments/assets/2d74cde6-cfc0-4bc8-a96f-9d135ce6d4d0" />

Gateway orders:
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












