---
title: All you need to learn/adopt DevOps
date: 2024-02-19-1336 (February 19, 2024 1:36 PM)
tags:
- DevOps
---
# All you need to learn/adopt DevOps
1. Everything should be centered around **automation** and **Git** (version control)
  - Understand Git *deeply*
2. Write everything you're making in **code** with Git for version control
  - use Ansible and IaC (Infrastructure as Code)
  - even simple scripts - its how to start to train yourself
3. Adopt a **developer workflow** (e.g. Agile, Scrum, Kanban, etc.)
4. Understand Software Architecture
5. Read documentation and write documentation
  - When solving a problem, your first instinct should be is:
    - **to google (search/ask ai)** 
    - **READ** the docs

# Common tools you need
- [ ] 1. **Kubernetes** (see [learning-kubernetes]())
  - **Managed** (EKS, AKS, GKE, OpenShift)
  - **Self-managed**
  - Essential tools for Kubernetes (e.g. Helm, Kustomize, ArgoCD or FluxCD, etc.)
- [ ] 2. **Docker (Containers)**
- [ ] 3. **Terraform (IaC)**
- [ ] 4. **Ansible (Automation & Configuration Management)**
- [ ] 5. **CI/CD tools (any tool mentioned below)**
  - GitHub Actions
  - GitLab CI
  - Azure DevOps
  - Jenkins
- [ ] 6. **Monitoring tools (e.g. Prometheus, Grafana, etc.)**
- [ ] 7. **Cloud provider (e.g. AWS, Azure, GCP, etc.)** - pick one
- [ ] 8. **Programming language (e.g. Python, Go, etc.)** - pick one

# Remember these 3 things
1. **Failure is optimal.**
  - Failure is accepted and built into the process. 
  - Practice accepting your own failures, quickly, and move on.
2. **The ability to learn and adapt is the biggest trait required for DevOps.**
  - Accept that Technology is ever-evolving and rapidly changing
3. Slow is steady, steady is fast. 
  - take your time and write good code. Its okay to write good code that fails. Ugly hard to read code that fails is significantly worse.

# Start with this:
Get a lab environment for yourself. A Dell Optiplex will do. Maybe 2.
- Put a hypervisor on it. Proxmox.
- Install Linux VMs. Ubuntu is a good start.
- Setup a kubernetes cluster
- Write a very simple hello world app, deploy to said cluster manually
- Automate the deployment process
- Setup splunk on another VM so you can get logs.
- Modify your simple app to send logs to splunk
- For Linux stuff. After installing ubuntu, look up a video on some basic commands. (Dont install desktop version, just use command line)
- cd, ls, pwd, cat, less, more, tail, passwd, dmidecode, ip, su and sudo.
- Learn what goes under /usr. /home, /root, /dev, /etc.
- Create a new user account, give it sudo access but lock it down to only specific commands it can sudo.
- Learn how to use vi or vim to modify files in the command line. Or try nano. I prefer vim, its what I learned on.

Just google all these things and learn how to do them. There's also a devops roadmap somewhere. Great pathways.

DevOps is a buzzword title to a job. One company's devops team will have different responsibilites than another. Its very subjective to the company's needs. Most will attribute to some of the common tooling such as Jenkins/terraform/ansible. Yet, Azure Devops and Github can do similar things with pipelines. My team is more ops than dev, but we are moving hastily into more dev and automation.

# Explore
- [Bootstrapping K3s with Cilium](https://blog.stonegarden.dev/articles/2024/02/bootstrapping-k3s-with-cilium/)
