---
date: 2025-12-02T20:27
title:  Golden 5 Kubernetes Commands (Know this by muscle memory)
tags: 
    - DevOps
---
<!-- 2025-12-02-2027 (December 02, 2025 08:27:14 PM) -->

# Golden 5 Kubernetes Commands (Know this by muscle memory)

**Because when your cluster's on fire, you won't Google, you will type.**

Engineers who knew this golden 5 as "reflexes" are the real heroes when things break.

1. The first responder. Use it to check events, probe failures, container restarts, and volume errors.
```bash
kubectl describe pod <name>
```

2. Easily catch memory leaks, CPU death spirals, and OOM killers in seconds. Shows top pods using the most resources.
```bash
kubectl top pod
```

3. Gives actual timeline. Useful when you're guessing what broke first.
```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

4. Helps catch cross-node drain chaos and helps debug DNS resolution issues (not just for IP and node mapping).
```bash
kubectl get pods -o wide
```

5. Shows the logs of crashed containers (normally, normal logs are gone once they crash). Lets you see what killed the pod.
```bash
kubectl logs -p <pod>
```

