---
date: 2025-03-26T21:51
title: DevOps History
tags: 
    - DevOps
---
<!-- 2025-03-26-2151 (March 26, 2025 09:51:14 PM) -->

# DevOps History

- ref: [What backend systems does MMO games use? - comment](https://www.reddit.com/r/gamedev/comments/qad9mn/comment/hh2m19g/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)
- Automated testing (unit, integration, performance).  

---
Historically, application backends were developed as one single block (a monolith). This was fine for a fixed number of users accessing it simultaneously. Sudden surges of users however, like on christmas, were always hard to satisfy. There are generally two ways to solve this: Horizontal scaling and vertical scaling. The first one means to add more servers to the resource pool and is generally though of to be the cheaper solution. Vertical scaling means beefing up existing machines in the cluster and can be very expensive.

Developers quickly realized that some parts of an application have different scaling demands than others. While you usually want no latency for user-facing things, it's perfectly fine to wait 5 minutes more for some analyses. Thus the monolith was broken up into (micro-)services, that have different teams developing them, have different release cycles and generally are loosely coupled (maybe a message queue or a RPC solution). Examples of microservices would be an identity service (like keycloak) for user sign-in and token generation, anti-cheating detection or the actual gameserver where the game state lives.

Containers are a way of packaging such a service and its dependencies. They allow you to package an app and all of its config files, dependencies, etc. up into a neat package that you can run on any container runtime (like Docker, cri-o, containerd or Kata Containers). Because most services are composed of multiple services, you want to be able to manage them together. You do this with Container Orchestration Software like Kubernetes, Apache Mesos or Docker Compose.

Containers can be built automatically by buildservers (like GitLab, Jenkins, TravisCI, Circle CI, ...). This is called continuous integration. To be precise, CI just means that changes to a code base (i.e. commits) are integrated often into the main branch and automatically tested.

Ops people are doing something similar: Instead of managing infrastructure by hand, they only describe how it's supposed to look like and let software run those tasks (think Terraform, Chef, Puppet, Ansible). This is called Infrastructure as Code and is closely related to GitOps, which means managing the files in git to have a nice history of changes to the infrastructure.

Kubernetes is kind of the missing puzzle piece: As a developer, you define the resource needs of you services and how they scale. You can define health checks and use various metrics to automate scaling up or down. You also gain a lot of insight into how your application behaves by being able to collect logs (e.g. with FluentD) at a central location (like the ELK Stack, Graylog, Splunk). Metrics of your services can be saved in a time series database (InfluxDb, Prometheus) and nicely visualized (Grafana). You can run A/B tests (i.e. 5% of your userbase use the new version of the service, the other 95% use the old service). You can also easily do things like running Red/Green Deployments. Lastly one can use service meshes (Istio).

What a lot of companies are doing now is to automate deployment into the cluster: The build server runs tests on the real cluster. Typically you have unit tests and integration tests, most companies also do things like load testing. If the tests are successful, the service is automatically deployed to the cluster (ArgoCD). If your metrics show that more errors are occuring, a rollback to the last version is performed. We call this continuous deployment.

One trend in the last few years is called DevOps. As outlined above, the roles of a developer and an Ops Person have kind of merged together. Nowadays you can expect your developers to manager their own k8s deployments to be able to iterate rapidly. There are some people who only do ops (like the ones setting up the cluster in the first place), but it makes a lot of sense to learn K8s in any way.

I hope I could outline why companies chose to use K8s. I assume most GameDev companies have switched or are going to switch at some point, because it just makes a lot of sense.

So what should you do if you make an MMO by yourself? Somewhat counterintuitively I would suggest not going with K8s but instead to just build a monolith first. Chances are that as an indie developer the overhead of administrating such a cluster outweigh the benefits by far. Sure, a monolith might not be as efficient, but it's easier to keep track of what it does. If you ever feel the need you can still refactor it later. If you really think you need K8s at the beginning think about running you containers in a managed cluster (Amazon's EKS is a good option, I'm sure Google Cloud Platform or Azure have the same thing).

I know most of my reply is not very detailed, so if you have further questions feel free to ask :)
