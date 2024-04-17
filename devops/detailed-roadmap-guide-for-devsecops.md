---
tags:
  - How-To
  - Roadmap
  - DevOps
date: 2024-04-17T16:07
---
<!-- 2024-04-17-1607 (April 17, 2024 04:07:15 PM) -->

# Detailed Roadmap for Everything DevSecOps

## **THIS GUIDE in order (general roadmap):**

*I moved from Infrastructure/On Premise Systems Engineering to DevSecOps last year, based in Australia. I have a few overlooked tips, they helped me a lot.*
 
### 1. Learn Networks. (Architecture, Security): 
- NetSec is a bit of a hole in the dev skillset, and by gaining a deep understanding of how traffic runs through a businesses environment, you can capitalise heavily on presenting solutions to particular problems. 
  - Example Issue: “We need Egress Monitoring across all public facing gateways without affecting our hosts!” Solution: “Understand the Network, Implement a Firewall Appliance, Configure Alerts.”

### 2. Infrastructure-as-Code. 
- Very important to understand this, as chances are you’ll be specifically tasked with: 
  - managing/troubleshooting IAM Roles, 
  - Scanning Repos for Dependencies, and 
  - ensuring that Infrastructure is following whatever compliance standards your industry has in place.

### 3. Tools + Scripting! 
- Less about Containers/Orchestration, more about booting and working with little Security tools. 
- Expect to boot helpful little containers to do stuff like: 
  - Inspect SSL, 
  - Map VPCs and Subnets, and 
  - report on rogue Subdomains. 
- Also note that while many of these tools are specifically Python/Golang based, understanding how they’re working under the hood by understanding BASH and even PowerShell is quite important.

## Practical Sense and Application
**Since I had those skills, I think my formula was pretty efficient. I just looked at what defines DevSecOps and labbed as follows which landed me the role:**

### 1. `“Infrastructure as Code”:` 
- Create a Repo, 
- download Terraform, 
- sign up to AWS, open VS Code, and 
- write a file that creates: 
  1. 3x IAM Roles, 
  2. two Subnets, 
  3. two VPCs, 
  4. two EC2 Instances (Apache/PostgreSQL), 
  5. a Transient Gateway, and 
  6. two S3 Buckets. 
Check in.

### 2. `“Monitoring and Management”:`
- Create a Second Repo, and place files containing: 
  - a CloudWatch Configuration, 
  - GitHub Actions for Pull Requests, and 
  - Dependabot. 
- Now build an AWS Network Firewall, a Splunk Instance, and Enable CloudWatch.

### 3. `“Automation”:`
- Boot a container with: 
  - Snyk for SCA, 
  - a SAST tool of your choice, 
  - a DAST tool of your choice, and 
  - a Threat Mapping tool (of your choice). 
- Configure Splunk to ingest logs. Read them.

---
## Other things

*Interesting you mention egress and AWS Network Firewall. DevOps is to be able to implement it. DevSecOps would be to know how it works and therefore what threats it can (and cannot) mitigate. See [this](https://canglad.com/blog/2023/aws-network-firewall-egress-filtering-can-be-easily-bypassed/) recent article on how to bypass said firewall.*

- There's a `curl` based [test](https://chasersystems.com/discriminat/comparison/aws-network-firewall/#litmus-test) too you can run to test the effectiveness of this and other SNI-only egress solutions.

- Also, with [ECH now being enabled by default](https://groups.google.com/a/chromium.org/g/blink-dev/c/CmlXjQeNWDI/m/hx-_4lNBAQAJ?pli=1), the value present in the unencrypted SNI field cannot be trusted. (In theory. In practice, we aren't there yet.)
