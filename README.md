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

3. **Retrieve the Service Account Token:**

   Use the following command to get the token for the `ricardofood-sa` service account, which will be used in Jenkins:

   ```bash
   kubectl get secret ricardofood-sa-token -n ricardofood -o jsonpath="{.data.token}" | base64 --decode
   ```

   This token can then be added to Jenkins for authentication and access to the Kubernetes cluster.

---

This setup will allow you to deploy and manage the RicardoFood project components within a Minikube environment, using the provided configurations and scripts.

