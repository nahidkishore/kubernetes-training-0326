# Setting Up a Local Kubernetes Cluster with Minikube (Windows)

Minikube is a tool that lets you run Kubernetes locally. It runs a single-node Kubernetes cluster inside a Virtual Machine (VM) or Container on your personal computer.

---

## 1. Prerequisites

Before installing Minikube, ensure your Windows PC meets these requirements:

- **OS:** Windows 10 or Windows 11 (64-bit).
- **Virtualization:** Must be enabled in BIOS (VT-x or AMD-V).
- **Container/VM Driver:** You need _one_ of the following:
  - **Docker Desktop** (Recommended for easiest setup).
  - **Hyper-V** (Built-in to Windows Pro/Enterprise).
  - **VirtualBox** (Third-party hypervisor).

---

## 2. Installation

The easiest way to install Minikube on Windows is using a package manager like `winget` or `choco`.

### Option A: Using Winget (Built-in)

Open PowerShell as Administrator and run:

```powershell
winget install minikube
```

### Option B: Using Chocolatey

If you have Chocolatey installed:

```powershell
choco install minikube
```

### Option C: Direct Download

1.  Download the latest installer (.exe).
2.  Run the installer and follow the prompts.

_Note: You will also need `kubectl` (the Kubernetes command-line tool). Minikube can download it for you, or you can install it separately via `winget install kubectl`._

---

## 3. Starting the Cluster

Open PowerShell (or Command Prompt) as Administrator.

### If using Docker Desktop (Recommended)

```powershell
minikube start --driver=docker
```

### If using Hyper-V

```powershell
minikube start --driver=hyperv
```

_The first start may take a few minutes as it downloads the Kubernetes ISO/Image._

---

## 4. Verifying the Setup

Once Minikube has started, verify the cluster status:

```powershell
# Check cluster status
minikube status

# List nodes (should see one node named 'minikube')
kubectl get nodes

# Deploy a test Nginx pod
kubectl run nginx --image=nginx
kubectl get pods
```

---

## 5. Common Minikube Commands

| Command              | Description                                          |
| :------------------- | :--------------------------------------------------- |
| `minikube start`     | Starts the local Kubernetes cluster.                 |
| `minikube stop`      | Stops the cluster (saves state).                     |
| `minikube delete`    | Deletes the cluster and all data (fresh start).      |
| `minikube dashboard` | Opens the web-based Kubernetes UI.                   |
| `minikube ip`        | Shows the IP address of the cluster node.            |
| `minikube ssh`       | Log directly into the Minikube VM/Container.         |
| `minikube pause`     | Pauses Kubernetes resources without stopping the VM. |

---

## 6. Troubleshooting Tips

- **Virtualization Error:** If Minikube fails to start, check Task Manager -> Performance -> CPU to ensure "Virtualization" is "Enabled".
- **Driver Issues:** If Docker isn't working, try forcing Hyper-V: `minikube start --driver=hyperv`.
- **Kubectl Not Found:** If `kubectl` commands fail, run `minikube kubectl -- get pods` (or add minikube to your PATH).
