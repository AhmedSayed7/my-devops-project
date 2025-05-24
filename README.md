# DevOps Project: Flask App with Docker, Kubernetes, Helm, and Jenkins

This project demonstrates a complete DevOps workflow for deploying a simple Flask web application using Docker for containerization, Jenkins for CI/CD, Kubernetes for orchestration, and Helm for deployment management. The pipeline automates building, testing, and deploying the application to a Kubernetes cluster.

## Features
- **Containerization**: Dockerizes a Flask application for portability.
- **CI/CD Pipeline**: Automates build, test, and deployment using Jenkins.
- **Kubernetes Deployment**: Deploys the app to a Kubernetes cluster (e.g., Minikube for local development).
- **Helm Chart**: Manages Kubernetes resources with a reusable Helm chart.
- **GitHub Integration**: Triggers Jenkins builds on code pushes via webhooks.

## Prerequisites
To run this project, ensure you have the following installed:
- [Docker](https://docs.docker.com/get-docker/) (for building container images)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/) (for a local Kubernetes cluster)
- [Helm](https://helm.sh/docs/intro/install/) (for Kubernetes package management)
- [Jenkins](https://www.jenkins.io/doc/book/installing/) (for CI/CD pipeline)
- [Git](https://git-scm.com/downloads) (for version control)
- A [Docker Hub](https://hub.docker.com/) account for pushing images
- Python 3.9+ (optional, for local testing)

## Installation and Setup

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/my-devops-project.git
cd my-devops-project
```

### 2. Set Up Minikube
Start a local Kubernetes cluster:
```bash
minikube start
```

### 3. Install Helm
Follow the official Helm installation guide: [Helm Installation](https://helm.sh/docs/intro/install/). Verify with:
```bash
helm version
```

### 4. Set Up Jenkins
Install Jenkins on a server or locally:
- For a server (e.g., Red Hat 9 on AWS EC2):
  ```bash
  wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
  sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
  sudo yum upgrade
  sudo yum install java-17-openjdk
  sudo yum install jenkins
  systemctl daemon-reload
  systemctl enable jenkins --now
  ```
- Access Jenkins at `http://<server-ip>:8080` and complete the setup using the admin password from `/var/lib/jenkins/secrets/initialAdminPassword`.

Install the following Jenkins plugins:
- Docker Pipeline
- Kubernetes
- GitHub Plugin
- Docker, Docker API, Docker Build Step
- Pipeline, Pipeline: Stage View

### 5. Configure Jenkins
- **Add Credentials**:
  - Docker Hub: Add username and password (ID: `dockerhubCred`).
  - Kubernetes: Add kubeconfig (ID: `k8smaster`).
  - GitHub: Add credentials for repository access.
- **Configure Tools**: In `Manage Jenkins > Global Tool Configuration`, add Maven or other tools if needed.
- **Set Up Webhook**: In your GitHub repository, go to `Settings > Webhooks > Add Webhook`. Set the payload URL to `http://<jenkins-url>/github-webhook/` with `application/json` content type.

### 6. Create Jenkins Pipeline
- In Jenkins, create a new pipeline job.
- Point it to your GitHub repository and specify the `Jenkinsfile`.

## Project Structure
```
my-devops-project/
├── README.md               # Project documentation
├── LICENSE                 # License file (MIT)
├── .gitignore              # Ignored files (e.g., build artifacts)
├── src/                    # Application source code
│   ├── main.py             # Flask application
│   └── requirements.txt    # Python dependencies
├── Dockerfile              # Docker image configuration
├── Jenkinsfile             # CI/CD pipeline script
├── charts/                 # Helm chart for Kubernetes
│   ├── Chart.yaml          # Helm chart metadata
│   ├── values.yaml         # Helm chart configuration
│   └── templates/          # Kubernetes manifests
│       ├── deployment.yaml # Deployment configuration
│       ├── service.yaml    # Service configuration
```

## Usage
1. **Push Code to GitHub**:
   - Commit and push changes to your repository:
     ```bash
     git add .
     git commit -m "Initial commit"
     git push origin main
     ```
   - This triggers the Jenkins pipeline via the webhook.

2. **Run the Pipeline**:
   - The pipeline checks out the code, builds a Docker image, pushes it to Docker Hub, and deploys it to Kubernetes using Helm.
   - Monitor the pipeline in Jenkins at `http://<jenkins-url>`.

3. **Access the Application**:
   - Verify the deployment:
     ```bash
     helm list
     ```
   - Access the app via Minikube:
     ```bash
     minikube service flask-app-service
     ```
   - The app will be available at the provided URL, displaying "Hello, DevOps World!".

## Contributing
Contributions are welcome! Please fork the repository, create a feature branch, and submit a pull request with your changes.

## Acknowledgments
- [How to Set Up Jenkins CI/CD on Kubernetes Cluster Using Helm](https://www.red-gate.com/simple-talk/devops/ci-cd/how-to-set-up-jenkins-ci-cd-on-kubernetes-cluster-using-helm/)
- [DevOps Excellence: Dockerized Multi-Branch Pipeline with Jenkins, Kubernetes, Helm on AWS](https://medium.com/@mlynreddy/devops-excellence-dockerized-multi-branch-pipeline-with-jenkins-kubernetes-helm-onaws-43e9c9e6aef3)
- [Official Helm Documentation](https://helm.sh/docs/)
- [Docker Documentation](https://docs.docker.com/)
- [Jenkins Documentation](https://www.jenkins.io/doc/)
