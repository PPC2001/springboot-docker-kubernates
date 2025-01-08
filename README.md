-----------------------------------
Deploying a Spring Boot application to Kubernetes using Docker and Google Cloud Platform (GCP).

-------------------------------------


1. Create a Spring Boot REST Endpoint

Step 1.1: Initialize a Spring Boot Project

   Use Spring Initializr to generate a Spring Boot project:
Dependencies: Spring Web, Spring Boot DevTools.
Packaging: Jar.
Java Version: 17 (or the version you are using).
Download and unzip the project.

Step 1.2: Add a REST Endpoint

![image](https://github.com/user-attachments/assets/b61d99f0-3f74-4a44-bce2-86b190a98475)

Step 1.3: Test the Application

mvn spring-boot:run : Access the endpoint at http://localhost:8080/hello to verify it returns "Hello, Kubernetes!".

------------------

2. Create a Docker Image

Step 2.1: Add a Dockerfile

Create a Dockerfile in the root of your project:

![image](https://github.com/user-attachments/assets/8fe72d56-d2bd-4862-a0da-834b79aa2f5a)


Step 2.2: Build the Spring Boot JAR

mvn clean package

Step 2.3: Build the Docker Image

docker build -t ppc2001/spring-boot-app:v1 .


Step 2.4: Test the Docker Image Locally

docker run -p 8080:8080 ppc2001/spring-boot-app:v1

![image](https://github.com/user-attachments/assets/85edc4ba-bbbb-4656-b311-65dc2b5aba9a)

Access http://localhost:8080/hello to verify the container works.

![image](https://github.com/user-attachments/assets/107c337e-6dfb-43cc-aa57-dd93b2c7e238)


----------

3. Push the Docker Image to Docker Hub

Step 3.1: Log in to Docker Hub
Log in to your Docker Hub account using below command:

docker login

Step 3.2: Push the Image

docker push ppc2001/spring-boot-app:v1

![image](https://github.com/user-attachments/assets/40d5be06-9fb8-4538-87e6-0a7154a5b7f6)


-----------------

4. Set Up GCP and Kubernetes

Step 4.1: Enable Required APIs
Enable the Kubernetes Engine API:

gcloud services enable container.googleapis.com

Step 4.2: set a default Zone 

gcloud config set compute/region us-east1

gcloud config set compute/zone us-east1-b

![image](https://github.com/user-attachments/assets/c795b0ee-8e45-431c-9353-56147c0353bb)


Step 4.3: Create a Kubernetes Cluster

Create a GKE cluster:

gcloud container clusters create spring-boot-cluster \
    --zone us-east1-b \
    --num-nodes 2

![image](https://github.com/user-attachments/assets/63878d73-f68b-4048-a032-8b2e19ba44e8)



Step 4.4: Configure kubectl

Fetch the cluster credentials:

gcloud container clusters get-credentials spring-boot-cluster --zone us-east1-b

![image](https://github.com/user-attachments/assets/4ded6006-28d0-4d69-b89d-9cf81b605a87)


---------------

5. Deploy the Application to Kubernetes

Step 5.1: Create Kubernetes Deployment

Create a spring-boot-deployment.yaml file:

![image](https://github.com/user-attachments/assets/4d89c63a-63f9-44f7-8b03-b1225b2b2ad1)



Step 5.2: Create Kubernetes Service

Create a spring-boot-service.yaml file:

![image](https://github.com/user-attachments/assets/2be2601b-2e4f-44f5-9fc2-328fbc95a1c2)


Step 5.3: Apply the YAML Files

Deploy the application and service:

kubectl apply -f spring-boot-deployment.yaml
kubectl apply -f spring-boot-service.yaml


------------------------

6. Verify the Deployment

Step 6.1: Check Pods

kubectl get pods

![image](https://github.com/user-attachments/assets/5ac90282-b5d1-4883-9d6f-afa34b31d99e)

Step 6.2: Get the External IP
Retrieve the external IP of the service:

kubectl get services

![image](https://github.com/user-attachments/assets/1c972c60-3515-4b5b-9235-d55a4d0bdbd8)


Step 6.3: Test the Application
Access the application at http://<EXTERNAL-IP>/hello to verify it works.

![image](https://github.com/user-attachments/assets/9d219ad3-a560-4fe7-ad25-29bd8bf9e454)


------------------

7. Clean Up Resources

Delete the Kubernetes cluster to avoid unnecessary charges:

gcloud container clusters delete spring-boot-cluster --zone us-west1-a

![image](https://github.com/user-attachments/assets/c2f8da41-e7f8-46f8-96fe-8a247af83bf7)













