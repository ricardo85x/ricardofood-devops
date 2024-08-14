FROM jenkins/jenkins:latest

# Copia o arquivo plugins.txt para o container
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt

# Instala os plugins listados em plugins.txt
RUN jenkins-plugin-cli --plugin-file /usr/share/jenkins/ref/plugins.txt

# Switch to root to install Docker and modify groups
USER root

# Install Docker CLI if not already installed
RUN apt-get update && apt-get install -y docker.io


# Install Trivy for analyzing Docker images
RUN apt-get install -y wget apt-transport-https gnupg lsb-release

RUN wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor -o /usr/share/keyrings/trivy.gpg \
    && echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb generic main" | tee /etc/apt/sources.list.d/trivy.list \
    && apt-get update \
    && apt-get install -y trivy

# Install kubectl
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" \
    && chmod +x kubectl \
    && mv kubectl /usr/local/bin/kubectl

# Add the Jenkins user to the Docker group
RUN usermod -aG docker jenkins

# Switch back to the Jenkins user
USER jenkins