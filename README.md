# RicardoFood DevOps Repository

Welcome to the RicardoFood DevOps repository! This repository is dedicated to storing all DevOps-related configurations and scripts for the RicardoFood project.

## Overview

This repository contains the following:

- **Jenkins Pipelines**: All Jenkins pipeline scripts used to automate various aspects of the RicardoFood project, including build, test, and deployment processes.
- **Kubernetes YAML Files**: Configuration files for deploying and managing the RicardoFood project in a Kubernetes environment.
- **Docker Compose**: A `docker-compose.yaml` file for setting up Jenkins, SonarQube, and Nexus in a local development environment.

## Structure

- `pipelines/`: This directory contains all the Jenkins pipeline scripts (`Jenkinsfile`) that automate the CI/CD process.
- `kubernetes/`: This directory holds all Kubernetes deployment, service, and configuration files (`.yaml`) used to deploy the RicardoFood project components.
- `docker-compose.yaml`: A Docker Compose file that sets up Jenkins, SonarQube, and Nexus for local development and testing.

## Testing in a Minikube Environment

To test the RicardoFood project in a Minikube environment, follow these steps:

1. **Create the Namespace:**

   Apply the `namespace.yaml` file to create the necessary namespace:

   ```bash
   kubectl apply -f namespace.yaml
   ```

2. **Create the Admin Service Account:**

   Apply the `service-account.yaml` file to create the admin user for this namespace:

   ```bash
   kubectl apply -f service-account.yaml
   ```

3. **Deploy the Database:**

   Apply the following commands to deploy the MySQL database:

   ```bash
   kubectl apply -f db/mysql-configmap.yaml -f db/mysql-pvc.yaml -f db/mysql-statefulset.yaml
   ```

4. **Deploy RabbitMQ:**

   To deploy a RabbitMQ cluster, follow these steps:

   - **Install the Kubernetes plugin for RabbitMQ:**

     First, apply the following command to install the RabbitMQ cluster operator:

     ```bash
     kubectl apply -f "https://github.com/rabbitmq/cluster-operator/releases/latest/download/cluster-operator.yml"
     ```

   - **Create the RabbitMQ cluster:**

     After installing the operator, apply the following command to create the RabbitMQ cluster:

     ```bash
     kubectl apply -f rabbit/rabbitmq-cluster.yaml
     ```

   - **Retrieve the RabbitMQ username and password:**

     After the cluster is created, you will need to retrieve the automatically generated username and password:

     - To get the username:

       ```bash
       kubectl get secret rabbitmq-cluster-default-user -n ricardofood -o jsonpath='{.data.username}' | base64 --decode
       ```

     - To get the password:

       ```bash
       kubectl get secret rabbitmq-cluster-default-user -n ricardofood -o jsonpath='{.data.password}' | base64 --decode
       ```

   - **Update the RabbitMQ ConfigMap:**

     Edit the `rabbit/rabbitmq-configmap.yaml` file and update the `RABBITMQ_USER` and `RABBITMQ_PASSWORD` keys with the username and password obtained in the previous steps.

   - **Apply the updated ConfigMap:**

     After editing the `rabbitmq-configmap.yaml`, apply it using the command:

     ```bash
     kubectl apply -f rabbit/rabbitmq-configmap.yaml
     ```

5. **Retrieve the Service Account Token:**

   Use the following command to get the token for the `ricardofood-sa` service account, which will be used in Jenkins:

   ```bash
   kubectl get secret ricardofood-sa-token -n ricardofood -o jsonpath="{.data.token}" | base64 --decode
   ```

   This token can then be added to Jenkins for authentication and access to the Kubernetes cluster.

6. **Obtain the Kubernetes `SERVER_URL`:**

   To get the `SERVER_URL` needed for Kubernetes configurations in Jenkins, execute the following command:

   ```bash
   kubectl cluster-info
   ```

   The output will include the URL of the Kubernetes control plane, which you can use as the `SERVER_URL` in your Jenkins configuration.

7. **Connect Minikube Network to Jenkins Container (if Minikube is running in Docker):**

   If you are running Minikube inside Docker, you need to connect the Minikube network to the Jenkins container to allow Jenkins to interact with the Kubernetes cluster:

   ```bash
   docker network connect minikube jenkins
   ```

---

This setup will allow you to deploy and manage the RicardoFood project components within a Minikube environment, using the provided configurations and scripts.