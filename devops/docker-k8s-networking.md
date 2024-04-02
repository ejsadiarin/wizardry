---
id: docker-k8s-networking
aliases: []
tags:
  - Docker
  - Kubernetes
  - Networking
date: 2024-03-24-1125 (March 24, 2024 11:25 AM)
title: How Docker and Kubernetes differ from Traditional Networking
---

# How Docker and Kubernetes differ from Traditional Networking

## Docker
- Docker networking does differ from traditional networking in several key ways:

1. **Isolation**: Docker containers are isolated from each other and from the host system by default. This means that each container has its own network namespace, which includes its own network stack, IP address, routing table, and firewall rules. This isolation is crucial for security and allows containers to run in a secure and isolated environment.
2. **Bridge Networking**: By default, Docker uses a bridge network for containers. This bridge network acts as a virtual switch, allowing containers on the same bridge network to communicate with each other, while isolating them from the host system and other networks. This is different from traditional networking, where devices on the same network can communicate with each other without the need for a virtual switch.
3. **NAT (Network Address Translation)**: Docker uses NAT to allow containers to access the external network. This is different from traditional networking, where devices typically have their own public IP addresses or are behind a router that handles NAT.
4. **Port Mapping**: Docker allows you to map ports from the container to the host system, making it possible to access services running inside a container from outside the host. This is a feature that is not typically available in traditional networking without additional configuration .
5. **Service Discovery**: Docker provides built-in service discovery for containers running on the same network. This allows containers to communicate with each other using service names instead of IP addresses, which is not a standard feature in traditional networking.
6. **Overlay Networking**: Docker supports overlay networks, which allow containers running on different hosts to communicate with each other as if they were on the same network. This is particularly useful in Docker Swarm and Kubernetes environments, where containers might be distributed across multiple hosts. 

In summary, Docker networking is designed to be flexible, secure, and isolated, making it suitable for containerized applications. It differs from traditional networking in terms of isolation, networking modes (bridge, overlay), NAT, port mapping, service discovery, and the ability to run containers across multiple hosts

## Kubernetes
- Kubernetes networking also differs significantly from traditional networking, especially in the context of container orchestration and micro
services architecture. 

1. **Pods and Services**: In Kubernetes, the smallest deployable units are Pods, which can contain one or more containers. Kubernetes uses Services to expose these Pods to the network. Services provide a stable IP address and DNS name, allowing other Pods or external clients to access the Pods. This is a fundamental difference from traditional networking, where services are not a built-in concept.
2. **Network Policies**: Kubernetes supports Network Policies, which allow administrators to control the flow of traffic between Pods and Services. This is a powerful feature for security and isolation, enabling fine-grained control over network access. Traditional networking typically does not offer this level of granularity without additional tools or configurations.
3. **Ingress and Egress**: Kubernetes introduces the concepts of Ingress and Egress, which manage external access to the services in a cluster. Ingress allows external traffic to be routed to services within the cluster, while Egress controls outbound traffic from the cluster. This is a feature not commonly found in traditional networking, where traffic routing is often managed by routers or firewalls.
4. **Service Mesh**: Kubernetes can be integrated with a Service Mesh, such as Istio or Linkerd, to provide advanced networking features like traffic management, security, and observability. These features are not typically available in traditional networking without additional tools or configurations.
5. **Multi-Tenancy**: Kubernetes supports multi-tenancy, allowing multiple users or teams to use the same cluster resources. This is achieved through namespaces, which provide a scope for names and can be used to implement network policies and resource quotas. Traditional networking does not inherently support multi-tenancy in the same way.
6. **Load Balancing and Scaling**: Kubernetes provides built-in load balancing and scaling capabilities for services. This is different from traditional networking, where load balancing and scaling might require additional hardware or software solutions.
7. **Cross-Cluster Communication**: Kubernetes supports cross-cluster communication through federation and service meshes, allowing services in one cluster to communicate with services in another cluster. This is a feature not commonly found in traditional networking without significant customization.

In summary, Kubernetes networking is designed to support the dynamic, scalable, and secure deployment of containerized applications. It introdu ces concepts like Pods, Services, Network Policies, Ingress, Egress, and Service Meshes, which are not typically found in traditional networking. These features enable Kubernetes to provide advanced networking capabilities tailored to the needs of containerized applications and micro services architectures.
