---
tags:
  - DevOps
date: 2024-04-17T18:09
---
<!-- 2024-04-17-1809 (April 17, 2024 06:09:51 PM) -->

# WHAT IS DevSecOps?

**Welcome to DevSecOps.**
--> Dev to build the thing and test it 
--> Sec to ensure things are safe 
--> Ops to ensure they keep working and can be deployed

  - When creating anything on AWS or any cloud or PasS, you either need to document every single step and decision made, or to deploy and manage it using Infrastructure as Code. That way if something goes wrong or it needs to be duplicated it can be.
  - When writing code, use defensive programming and ensure the inputs are valid before doing anything else with those inputs. Write tests for your code, make sure you test the unhappy path for the error cases
  - Security is an aspect that needs to be implemented from the ground up as part of your code.
  - Use a least privilege approach so that things only have the minimum permission needed for them to function. This will involve writing IAM policies and assigning them to various custom roles.
  - Encryption in flight. Serve all your content via a TLS connection, if you are calling other APIs connect to them via a TLS connection such as HTTPS.
  - Encryption at rest. Store the content in your S3 bucket encrypted using either the default encryption key or a CMK via KMS
  - Along with least privilege comes the concept of minimizing the blast radius, where if someone gets access to something they shouldn't, they can't go anywhere else.
  - Apply industry best practice and keep them up to date as things change over time.
  - Use versioning of objects in your S3 buckets, that way you can see if anyone has screwed with a file in the bucket.
  - Configure auditing and logging of access, you want to see who accessed what and when and what they did.
  - Use a WAF in front of your exposed interfaces

### The above covers you for roughly all the things that are internal to AWS
- When exposing your API, you will need some system of authentication to ensure only authorized users or systems can access them. We don't know enough about your clients/users to answer here. But in general you want to use some form of OAuth2, SAML, or API key driven approach probably using JWTs for authentication.
- Finally you want observability into things, metrics, logs, and traces. See where errors are there are, how long things take, who and what is happening. For this the four golden signals (as per the Google SRE book) probably utilizing either or both the RED or USE methods for metrics.

### And all of that still doesn't cover the CI/CD aspect of your application
- Being a developer is so much easier than having to deal with the ever evolving world of Cloud tech, Ops, and Security, that is part of the life of the DevSecOps people.
- Tutorials don't cover half of this stuff, some of it is just learning and playing around, and things that work now might not work tomorrow. I am revisiting some IaC for an S3 bucket that was created 7+ months ago, that same IaC doesn't work anymore. APIs and things have changed.

Good luck.

## More on DevSecOps
- [Detailed Roadmap Guide](wizardry/devops/detailed-roadmap-guide-for-devsecops.md)
