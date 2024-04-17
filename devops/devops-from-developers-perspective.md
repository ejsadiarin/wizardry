---
tags:
  - DevOps
date: 2024-04-17T18:15
---
<!-- 2024-04-17-1815 (April 17, 2024 06:15:27 PM) -->

# DevOps from a Developer's Perspective

### As a java devops who is responsible for teaching junior devs I can probably apply:
1. Web security, OWASP top 10
2. Network protocols. - ip, tcp, dns, http - important headers (etag, if, cache-control, cookie..) and response codes classes, tls (+ crypto), websockets.
3. Docker - dockerfiles, repositories, cli, compose. Dockerize petstore app and run locally in docker.
4. Set up nginx in a docker as a reverse proxy in front of a petstore.
5. REST and principles: HATEOAS, idempotence, cacheability.
6. SQL databases - how to deploy locally with docker, analyze and optimize queries (explain), indices, data types. Deploy database locally in docker.
7. Monitoring - Prometeus, Graphana. OTEL. Metrics. Visibility. Configure monitoring stack locally in docker and set up monitoring of a petstore
8. JVM optimization for docker. jlink/jpackage. Heap size and vcpu options. Graalvm native build. Distroless containers. Optimize java app docker image.
9. Oauth2. Oidc. Authorization. Deploy dockerized Keycloak locally and use it for auth.
10. Clouds. IAM. S3. RDS. Move and deploy dockerized app in the cloud (apprunner/beanstalk/ecs)
11. Infrastructure as code. Cloudformation/Terraform/CDK. Write a stack to run docker image locally (terraform/cdktf) or in the cloud.
12. CI/CD. Github actions. Pipeline to build and deploy docker image to the repository.
13. Optionally - serverless, lambdas, dynamodb, api gateway, stepfunctions, sns/sqs/eventbridge.

### For Developers? SWE (TO BE LEARNED FOR THE FIRST 3 WEEKS):
- How to containerize an app. DockerFile, docker compose and k8s deployment charts
- Ingress / Proxy. How to route traffic to various microservices
- Git branching. How it affects Deployment
- Git hooks/Jenkins Build
- Deployment/Orchestration via Getwebhookups or Jenkins - Monitoring/Observability
- General design for micro-service architecture - Routing, Communication, Messaging
Advance: From the above
- Composability. Making workflow extendable to handle scaling. 
  - Using environment variables. How to deploy 10,000 microservices in production at scale. Design patterns for this.
- Environmental Configuration. Like above but for multiple deployments
- API gateways
- Scaling. Disaster Recover, Failover
- K8s best practices
- Platform engineering, DevX (Developer Experience)
Security:
- Code Scanning . Image and Repository Scanning
- OWASP
- Basic Best Practices: non-root, ingress configurations for XSS, Cross-site, signature,etc
- Key Servers. Key rotation. How to build those hooks into their apps
- Service Mesh
**We teach all this new hires. They learn this in their ==first 3 weeks==.**

# References
- https://www.reddit.com/r/devops/comments/1ax67ey/if_you_were_to_create_a_devops_course_for_your/?share_id=9PZ4ofz-oDH8X4h01oJa0
