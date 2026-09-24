# 🚀 End-to-End CI/CD Pipeline with GitHub Actions, SonarQube, Docker & Amazon EKS

## 📌 Project Overview

This project demonstrates an **end-to-end CI/CD pipeline** built using **GitHub Actions** to automate application build, code-quality analysis, containerization, security scanning, and deployment to **Kubernetes on Amazon EKS**.

The project also demonstrates how to configure an **AWS EC2 virtual machine as a self-hosted/private GitHub Actions runner**, allowing CI/CD workloads to run on infrastructure controlled by the organization.

### 🔄 CI/CD Workflow

```text
Developer
    │
    │ git push
    ▼
GitHub Repository
    │
    ▼
GitHub Actions
    │
    ├── Checkout Source Code
    │
    ├── Build Application
    │
    ├── SonarQube
    │      └── Code Quality Analysis
    │
    ├── Trivy
    │      └── Security / Vulnerability Scan
    │
    ├── Docker
    │      └── Build Container Image
    │
    └── Kubernetes
           │
           ▼
        Amazon EKS
           │
           ▼
     Application Running
```

---

## 🎯 Project Objectives

The main objectives of this project are:

* Build a CI/CD pipeline using **GitHub Actions**
* Understand the difference between **GitHub-hosted and self-hosted runners**
* Configure an **EC2 VM as a private/self-hosted runner**
* Integrate **SonarQube** for static code analysis
* Integrate **Trivy** for vulnerability scanning
* Build a **Docker container image**
* Deploy the application to **Kubernetes**
* Create and configure an **Amazon EKS cluster**
* Automate Kubernetes deployment through GitHub Actions

---

# 🛠️ Technologies Used

| Technology     | Purpose                           |
| -------------- | --------------------------------- |
| Git & GitHub   | Source Code Management            |
| GitHub Actions | CI/CD Automation                  |
| AWS EC2        | Self-hosted GitHub Actions Runner |
| SonarQube      | Static Code Analysis              |
| Trivy          | Security / Vulnerability Scanning |
| Docker         | Containerization                  |
| Kubernetes     | Container Orchestration           |
| Amazon EKS     | Managed Kubernetes Cluster        |
| kubectl        | Kubernetes CLI                    |
| AWS CLI        | AWS Resource Management           |

---

# 🏗️ Architecture

```text
                    ┌─────────────────────┐
                    │      Developer      │
                    └──────────┬──────────┘
                               │
                               │ git push
                               ▼
                    ┌─────────────────────┐
                    │   GitHub Repository │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │   GitHub Actions    │
                    │      Workflow       │
                    └──────────┬──────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
              ▼                ▼                ▼
        ┌───────────┐    ┌───────────┐    ┌───────────┐
        │ SonarQube │    │   Trivy   │    │  Docker   │
        │  Analysis │    │   Scan    │    │   Build   │
        └───────────┘    └───────────┘    └─────┬─────┘
                                                │
                                                ▼
                                      ┌──────────────────┐
                                      │  Docker Registry  │
                                      └────────┬─────────┘
                                               │
                                               ▼
                                      ┌──────────────────┐
                                      │   Amazon EKS     │
                                      │    Kubernetes    │
                                      └────────┬─────────┘
                                               │
                                               ▼
                                      ┌──────────────────┐
                                      │   Application    │
                                      │      Pods        │
                                      └──────────────────┘
```

---

# 🔹 1. GitHub Repository Setup

The application source code is maintained in a GitHub repository.

The repository contains:

```text
project/
│
├── application-source-code
├── Dockerfile
├── kubernetes/
│   ├── deployment.yaml
│   └── service.yaml
│
└── .github/
    └── workflows/
        └── ci-cd.yml
```

The GitHub Actions workflow is triggered when changes are pushed to the repository.

---

# 🔹 2. GitHub Actions Runner

GitHub Actions provides two major runner options:

### GitHub-hosted Runner

GitHub provides and manages the runner infrastructure.

```text
GitHub
   │
   ▼
GitHub-hosted Runner
   │
   ▼
Pipeline Jobs
```

### Self-hosted Runner

In this project, an AWS EC2 instance is configured as a **self-hosted GitHub Actions runner**.

```text
AWS EC2
   │
   └── GitHub Actions Runner
             │
             ├── Build
             ├── SonarQube
             ├── Trivy
             ├── Docker
             └── kubectl
```

This provides more control over the execution environment and allows the pipeline to use tools and infrastructure configured on the EC2 instance.

---

# 🔹 3. SonarQube Integration

**SonarQube** is integrated into the CI pipeline to perform static code analysis.

The pipeline analyzes the application source code for issues such as:

* Bugs
* Code smells
* Security issues
* Maintainability issues
* Code quality problems

### Pipeline Flow

```text
Source Code
     │
     ▼
Build
     │
     ▼
SonarQube Analysis
     │
     ▼
Quality Evaluation
     │
     ▼
Continue Pipeline
```

This helps identify code-quality issues before the application is deployed.

---

# 🔹 4. Trivy Security Scan

**Trivy** is used to scan container images for known vulnerabilities.

Example workflow:

```text
Application Source
       │
       ▼
Docker Build
       │
       ▼
Docker Image
       │
       ▼
Trivy Scan
       │
       ▼
Security Check
```

This introduces security scanning into the CI/CD process instead of waiting until deployment.

---

# 🔹 5. Docker Containerization

The application is packaged into a Docker image using a `Dockerfile`.

Example:

```bash
docker build -t myapp:latest .
```

The image can then be tested and scanned before deployment.

```text
Application
     │
     ▼
 Dockerfile
     │
     ▼
Docker Image
     │
     ├── Trivy Scan
     │
     └── Push to Registry
```

---

# 🔹 6. Amazon EKS Cluster

An **Amazon EKS** cluster is created to provide the Kubernetes environment for application deployment.

Basic architecture:

```text
                    Amazon EKS
                       │
             ┌─────────┴─────────┐
             │                   │
       Control Plane          Worker Nodes
                                 │
                    ┌────────────┼────────────┐
                    │            │            │
                   Pod          Pod          Pod
```

The pipeline uses `kubectl` to communicate with the Kubernetes cluster.

---

# 🔹 7. Kubernetes Deployment

After the Docker image is created and validated, the GitHub Actions pipeline deploys it to Kubernetes.

Typical deployment flow:

```text
Git Push
   │
   ▼
GitHub Actions
   │
   ▼
Build Application
   │
   ▼
SonarQube
   │
   ▼
Trivy
   │
   ▼
Docker Build
   │
   ▼
Docker Registry
   │
   ▼
Amazon EKS
   │
   ▼
Kubernetes Deployment
   │
   ▼
Application Pods
```

Kubernetes manifests define the application deployment and service.

Example:

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: myapp

spec:
  replicas: 2

  selector:
    matchLabels:
      app: myapp

  template:
    metadata:
      labels:
        app: myapp

    spec:
      containers:
        - name: myapp
          image: myapp:latest
          ports:
            - containerPort: 8080
```

---

# 🔹 8. GitHub Actions Pipeline

The complete pipeline can be represented as:

```text
                    Git Push
                       │
                       ▼
              ┌─────────────────┐
              │ GitHub Actions   │
              └────────┬────────┘
                       │
                       ▼
                 Checkout Code
                       │
                       ▼
                  Build App
                       │
                       ▼
                 SonarQube
                       │
                       ▼
                   Trivy
                       │
                       ▼
                Docker Build
                       │
                       ▼
                Docker Registry
                       │
                       ▼
                  AWS EKS
                       │
                       ▼
                kubectl apply
                       │
                       ▼
               Kubernetes Pods
```

---

# 🔐 Security Considerations

The project demonstrates several security practices:

* Source-code quality analysis with SonarQube
* Container vulnerability scanning with Trivy
* Kubernetes deployment using controlled CI/CD
* AWS IAM-based access
* Secrets should be stored using **GitHub Actions Secrets/Variables** rather than hard-coded in workflow files

Example:

```yaml
env:
  AWS_REGION: ${{ secrets.AWS_REGION }}
```

Sensitive credentials should never be committed to GitHub.

---

# 📂 Project Structure

```text
.
├── .github/
│   └── workflows/
│       └── ci-cd.yml
│
├── kubernetes/
│   ├── deployment.yaml
│   └── service.yaml
│
├── Dockerfile
│
├── application/
│
└── README.md
```

---

# 🚀 Key DevOps Concepts Practiced

This project provided hands-on practice with:

### CI/CD

* Continuous Integration
* Continuous Delivery
* Pipeline automation
* GitHub Actions
* Pipeline jobs and steps
* Runner configuration

### Containerization

* Dockerfile
* Docker image creation
* Containerization
* Image scanning

### Security

* SonarQube
* Static code analysis
* Trivy
* Container vulnerability scanning
* Secrets management

### Kubernetes

* Kubernetes Deployments
* Kubernetes Services
* Pods
* `kubectl`
* EKS
* Automated deployments

### AWS

* EC2
* IAM
* Amazon EKS
* AWS CLI

---

# 📚 What I Learned

Through this project, I gained practical understanding of how different DevOps tools work together in a real CI/CD workflow.

The major learning was understanding that CI/CD is not simply about running a build.

A production-style pipeline can perform multiple stages:

```text
Code
 ↓
Build
 ↓
Code Quality
 ↓
Security Scan
 ↓
Container Build
 ↓
Container Scan
 ↓
Registry
 ↓
Kubernetes Deployment
```

This project helped me understand the relationship between **GitHub Actions, Docker, SonarQube, Trivy, AWS and Kubernetes** as components of a complete DevOps workflow.

---

# 🔮 Future Improvements

Possible improvements to this project include:

* Add Docker image tagging using Git commit SHA
* Push images to Amazon ECR
* Add automated rollback
* Implement Kubernetes health checks
* Add Helm charts
* Add Prometheus and Grafana monitoring
* Add Argo CD for GitOps-based deployment
* Add Terraform for infrastructure provisioning
* Implement separate Dev/Staging/Production environments

---

# 👨‍💻 Author

**Achyut Prasad**

Cloud / DevOps Engineer

### Skills

`AWS` `Linux` `Git` `GitHub` `GitHub Actions` `Docker` `Kubernetes` `Amazon EKS` `Terraform` `Ansible` `Jenkins` `SonarQube` `Trivy` `Prometheus` `Grafana`

---

⭐ If you find this project useful, feel free to explore the repository and follow the implementation.
