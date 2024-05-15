---
tags:
  - Wizardry
date: 2024-05-10T13:48
---
<!-- 2024-05-10-1348 (May 10, 2024 01:48:03 PM) -->

# Kubernetes Wizardry
- https://glasskube.dev/guides/kubectl/ 

## Exploring New Clusters - `kubectl` guide
- [ref](https://www.reddit.com/r/kubernetes/s/5MGCqP1QTj)

For exploring the cluster, k9s. If you see helm metadata on a resource then `helm ls -a -A` to see the release. Copy the release name and `helm show all <release name> -n <namespace>`

A few commands that you might try in case you know nothing about the cluster. These may not work, as they depend on certain third party tools to be running in the cluster.

- `kubectl get managed` - list anything that has been deployed by [Crossplane](crossplane.io)
- `kubectl get certificates` - list any cert manager certificates
- `kubectl get ingressroutes` - list any Traefik ingress routes
- `kubectl get prometheuses` - list any Prometheus instances controlled by Prometheus operator
- `kubectl get ServiceMonitors` - list any service monitoring rules for prometheus
- `kubectl get constraints` - list admission control policies for Gatekeeper

Other:
- `kubectl get crd` - To find out what custom resources are available 
- `kubectl cluster-info` - some details about the cluster
- `kubectl get nodes -o wide` - extra details about nodes
- `kubectl api-resources` - lists all *published* API kinds available (many which are built-in including a bunch which your cloud provider may manage and you'd never actually touch, as well as kinds made available by custom resource definitions.)

Oftentimes I find this command to be really useful because a CRD can be named anything but what matters is the API version and kind that the CRD makes available.

Event related:
- `kubectl events` - lists events that have happened cluster wide
- `kubectl get events` - list events from a specific namespace
- `kubectl get events -o yaml` - list events in yaml format. This is useful if you want to clear events from a namespace while debugging. The normal get events command above doesn't give you details to remove the events so you can get the actual name of the events from the yaml and then do `kubectl delete event <event names>`

Understanding the API of a resource: 
- `kubectl explain <Kind>` - for example `kubectl explain pod` will provide you details of the API for the pod resource so you could build out your manifests.
- `kubectl --api-version=example.com/v1alpha1 get <some kind>` - in case you have some custom resources installed using one CRD version then upgrade the CRD version, and it gives you a new API version, then your normal kubectl get commands will only report the details for resources deployed with the new API version. You would use this to list the old ones which would need to be migrated to the new API version.

There's not much I can say for exploring Flux resources because the commands you would use with the Flux cli are very similar to those for kubectl so I tend to use the kubectl commands except for when I need to force a reconcile. 

Hope this helps

Edit: Some of the custom resources are namespaced, so you may try adding `-A` to any of them to list those kinds of resources from all namespaces.

## Kubernetes Networking


