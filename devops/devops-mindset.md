---
date: 2025-03-26T16:34
title: DevOps Mindset
tags: 
    - DevOps
---
<!-- 2025-03-26-1634 (March 26, 2025 04:34:01 PM) -->

# DevOps Mindset

Think in project stages (SDLC):

### 1. Planning 
- Project Management --> Jira, Trello, GitHub Projects
- Documentation --> Confluence, GitBook
- Collaboration --> Slack, Microsoft Teams
- Diagramming --> Draw.io, LucidChart

### 2. Creating code
- IDEs --> VS Code, IntelliJ IDEA, Eclipse
- Version Control --> Git
- Code Repositories --> GitHub, GitLab, Bitbucket
- Code Quality --> SonarLint, ESLint, and other linters

### 3. Building code
- Build Tools --> Maven, Gradle, npm
- CI/CD Platforms --> Jenkins, GitLab CI, GitHub Actions
- Artifact Management --> JFrog Artifactory, Nexus

### 4. Testing code
- Unit Testing --> JUnit, pytest, Jest
- Integration Testing --> Postman, REST Assured
- Load Testing --> JMeter, k6
- UI Testing --> Selenium, Cypress
- Test Management --> TestRail, qTest

### 5. Release
- Building image
- Containerization --> Docker, Podman
- Container Orchestration --> Kubernetes
- Configuration Management --> Ansible, Puppet
- Infrastructure as Code --> Terraform, CloudFormation

### 6. Security scanning image
- SAST --> SonarQube, Checkmarx
- DAST --> OWASP ZAP, Burp Suite
- Container Scanning --> Trivy, Clair
- Dependency Scanning --> Snyk, Dependabot

### 7. Pushing image to registry
- Container Registry --> Docker Hub, AWS ECR, GitHub Container Registry
- Cloud Platforms --> AWS, Azure, GCP

### 8. Deployment - running image
- Incident Management --> PagerDuty, OpsGenie
- Secret Management --> HashiCorp Vault, AWS Secrets Manager
- Deployment Strategies --> Blue/Green, Canary

### 9. Monitoring and Logging
- Monitoring: Prometheus, Grafana
- Logging: ELK Stack, Splunk
- APM --> New Relic, Datadog


---

### **1. Code**  
**Processes**:  
- Version control, peer code reviews, collaborative development.  
**Tools**:  
- **Git** (version control), **GitHub/GitLab/Bitbucket** (repositories), **VS Code/IntelliJ** (IDEs).

---

### **2. Build**  
**Processes**:  
- Compiling code, creating binaries, containerization.  
**Tools**:  
- **Maven/Gradle** (Java), **npm/pip** (package managers), **Docker** (containerization), **Make** (build automation).

---

### **3. Dev**  
**Processes**:  
- Local development, debugging, environment setup.  
**Tools**:  
- **Docker** (local environments), **Vagrant** (VM provisioning), **Postman** (API testing), **PyCharm/VS Code** (IDEs).

---

### **4. Test and Security Scanning** 

**Processes**:  
- Automated testing (unit, integration, performance).  
- Static code analysis, security scanning images, dependencies, etc.

**Testing Tools**:  
- `JUnit/pytest` (unit testing),
- `Selenium/Cypress` (UI testing),
- `JMeter` (load testing),
- `SonarQube` (code quality).

**Security Scanning Tools**:
- `SonarQube`, `Checkmarx` (SAST)
- `OWASP ZAP`, `Burp Suite` (DAST)
- `Trivy`, `Clair` (Container Scanning)
- `Snyk`, `Dependabot` (Dependency Scanning)

---

### **5. Release**  

**Processes**:  
- Versioning artifacts, managing releases, artifact storage.  

**Tools**:  
- `Nexus/Artifactory` (artifact repositories),
- `Docker Hub` (container registry),
- `SemVer` (versioning).

---

### **6. Deploy**  

**Processes**:  
- Infrastructure provisioning, configuration management, secrets management, orchestration.  

**Tools**:  
- `Kubernetes` (container orchestration),
- `Ansible/Chef/Puppet` (configuration),
- `HashiCorp Vault`, `AWS Secrets Manager` (secrets management),
- `Terraform` (IaC),
- `Helm` (K8s package manager).

**Fully automated deployment to production after CI/CD pipeline success**:
- `ArgoCD` - GitOps for Kubernetes deployments
- `Spinnaker`
- `AWS CodeDeploy`
- `FluxCD`

---

### **9. Monitor**  

**Processes**:  
- Real-time system health tracking, log analysis, alerting.  

**Tools**:  
- `Prometheus/Grafana` (metrics)
- `ELK Stack` (logs)
- `New Relic/Datadog` (APM)
- `Sentry` (error tracking)

- `LGTM` stack (monitoring and logging)

---

# Now each stage can be done with CI/CD tools

### CI/CD tools

- Automatically build, test code, and release deployments on every commit, push, pull request, etc.

**CI/CD Tools**:  
- `GitHub Actions`
- `GitLab CI/CD`
- `Gitea Actions`
- `Jenkins`
- `CircleCI`
- `Travis CI`

