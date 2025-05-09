---
date: 2025-05-07T19:29
title: K3s Multiple Clusters/Contexts
tags: 
    - Kubernetes
    - DevOps
---
<!-- 2025-05-07-1929 (May 07, 2025 07:29:41 PM) -->

# K3s Multiple Clusters/Contexts

To manage multiple Kubernetes clusters (e.g., your local K3s cluster and a separate remote cluster) and switch between them seamlessly, you can use **kubeconfig contexts**. Here's how to set this up:

---

### **Step 1: Gather Kubeconfig Files**
1. **Local K3s Cluster** (on your laptop):
   - K3s automatically generates a kubeconfig file at `/etc/rancher/k3s/k3s.yaml`.
   - Copy it to your user directory:
     ```bash
     mkdir -p ~/.kube
     sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config-local
     sudo chown $USER ~/.kube/config-local
     ```

2. **Remote Cluster**:
   - Obtain the kubeconfig file for your remote cluster (e.g., `config-remote`).
   - For K3s clusters, this is typically at `/etc/rancher/k3s/k3s.yaml` on the remote server.

---

### **Step 2: Merge Kubeconfigs**
Merge both configurations into a single `~/.kube/config` file or manage them dynamically.

#### **Option 1: Merge Manually**
```bash
# Merge local and remote configs
KUBECONFIG=~/.kube/config-local:~/.kube/config-remote kubectl config view --flatten > ~/.kube/config
```

#### **Option 2: Use `KUBECONFIG` Environment Variable**
```bash
# Dynamically load multiple configs
export KUBECONFIG=~/.kube/config-local:~/.kube/config-remote
```

---

### **Step 3: Rename Contexts for Clarity**
Assign meaningful names to your clusters for easy identification:
```bash
# List existing contexts
kubectl config get-contexts

# Rename the local cluster context
kubectl config rename-context default local-k3s

# Rename the remote cluster context
kubectl config rename-context <remote-context-name> remote-cluster
```

---

### **Step 4: Switch Between Clusters**
Use `kubectl` or tools like `kubectx` to switch contexts:

#### **Using `kubectl`**:
```bash
# Switch to the local cluster
kubectl config use-context local-k3s

# Switch to the remote cluster
kubectl config use-context remote-cluster
```

#### **Using `kubectx` (Recommended)**:
1. Install `kubectx`:
   ```bash
   # For Linux/macOS
   brew install kubectx  # macOS
   sudo apt-get install kubectx  # Debian/Ubuntu
   ```

2. List and switch contexts:
   ```bash
   kubectx              # List all contexts
   kubectx local-k3s    # Switch to local cluster
   kubectx remote-cluster  # Switch to remote cluster
   ```

---

### **Step 5: Verify Cluster Access**
Check which cluster is active:
```bash
kubectl cluster-info
kubectl get nodes
```

---

### **Step 6: Simplify with Aliases (Optional)**
Add aliases to your shell profile (e.g., `~/.bashrc` or `~/.zshrc`):
```bash
alias kclocal="kubectl config use-context local-k3s"
alias kcremote="kubectl config use-context remote-cluster"
```

---

### **Example Workflow**
1. **Work on the local cluster**:
   ```bash
   kubectx local-k3s
   kubectl get pods -A
   ```

2. **Switch to the remote cluster**:
   ```bash
   kubectx remote-cluster
   kubectl get pods -A
   ```

---

### **Best Practices**
1. **Namespace Isolation**:
   - Use separate namespaces for different projects (e.g., `kubectl create ns my-project`).

2. **Tools for Better Management**:
   - **k9s**: A terminal UI for Kubernetes.
   - **Lens IDE**: A graphical Kubernetes dashboard.

3. **Security**:
   - Restrict permissions using `kubectl config set-credentials` for production clusters.
   - Use `~/.kube/config` file permissions (`chmod 600`).

---

### **Troubleshooting**
- **Permission Issues**: Ensure your kubeconfig file has correct permissions (`chmod 600 ~/.kube/config`).
- **Context Not Found**: Verify merged configs with `kubectl config view`.
- **Network Errors**: For remote clusters, ensure you have VPN/SSH access.

---

By following these steps, you can effortlessly switch between your local K3s cluster and any remote cluster using simple commands!
