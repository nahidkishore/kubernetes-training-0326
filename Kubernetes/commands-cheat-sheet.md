# 🧪 Minikube Command Cheat Sheet

---

# 🚀 1️⃣ Cluster Lifecycle Commands

### Start Cluster

```bash
minikube start
minikube start --driver=docker
minikube start --cpus=4 --memory=8192
minikube start --kubernetes-version=v1.29.0
```

### Stop Cluster

```bash
minikube stop
```

### Restart Cluster

```bash
minikube delete
minikube start
```

### Pause / Unpause

```bash
minikube pause
minikube unpause
```

### Delete Cluster

```bash
minikube delete
minikube delete --all
```

---

# 📊 2️⃣ Cluster Status & Info

### Check Status

```bash
minikube status
```

### Cluster Info

```bash
minikube ip
minikube version
```

### View Running Profile

```bash
minikube profile list
```

---

# 🧠 3️⃣ Node & SSH Access

### SSH into Minikube Node

```bash
minikube ssh
```

### Run Command Inside Node

```bash
minikube ssh "docker ps"
```

### Get Node IP

```bash
minikube ip
```

---

# 🌐 4️⃣ Service & Networking

### Get Service URL

```bash
minikube service <service-name>
```

### Open Service in Browser

```bash
minikube service <service-name> --url
```

### Enable Ingress

```bash
minikube addons enable ingress
```

### List Addons

```bash
minikube addons list
```

---

# 🧩 5️⃣ Addons Management

### Enable Addon

```bash
minikube addons enable metrics-server
minikube addons enable dashboard
```

### Disable Addon

```bash
minikube addons disable dashboard
```

---

# 📦 6️⃣ Image & Docker Integration

### Use Minikube Docker Environment

```bash
eval $(minikube docker-env)   # Linux/macOS
```

PowerShell:

```powershell
minikube docker-env | Invoke-Expression
```

### Load Image into Minikube

```bash
minikube image load my-image:latest
```

### List Images

```bash
minikube image list
```

---

# 🛠 7️⃣ Resource Configuration

### Start with Custom Resources

```bash
minikube start --cpus=2 --memory=4096
```

### Update Resources (delete & restart required)

```bash
minikube config set memory 8192
minikube config set cpus 4
```

### View Config

```bash
minikube config view
```

---

# 🔍 8️⃣ Logs & Troubleshooting

### View Logs

```bash
minikube logs
```

### Specific Component Logs

```bash
minikube logs --problems
```

### Dashboard

```bash
minikube dashboard
```

---

# 📁 9️⃣ Mount Local Directory

```bash
minikube mount /local/path:/container/path
```

Useful for development testing.

---

# 🔄 🔟 Profiles (Multiple Clusters)

### Create Profile

```bash
minikube start -p dev-cluster
```

### Switch Profile

```bash
minikube profile dev-cluster
```

### Delete Specific Profile

```bash
minikube delete -p dev-cluster
```

---

# ⚙️ 1️⃣1️⃣ Kubernetes Integration

### Check Nodes

```bash
kubectl get nodes
```

### Check Pods

```bash
kubectl get pods -A
```

Minikube automatically configures `kubectl`.

---

# 🧪 1️⃣2️⃣ Common Workflow (Real Dev Flow)

```bash
minikube start
kubectl apply -f deployment.yaml
kubectl get pods
minikube service my-service
minikube dashboard
```

---

# 🏁 Quick Summary

| Category       | Command                          |
| -------------- | -------------------------------- |
| Start Cluster  | `minikube start`                 |
| Stop Cluster   | `minikube stop`                  |
| Delete Cluster | `minikube delete`                |
| Status         | `minikube status`                |
| SSH Access     | `minikube ssh`                   |
| Service URL    | `minikube service <name>`        |
| Enable Addon   | `minikube addons enable <addon>` |
| Logs           | `minikube logs`                  |
| Dashboard      | `minikube dashboard`             |

---
