# 🪟 Minikube Installation on Windows (PowerShell Method)

---

## ✅ Prerequisites

* Run **PowerShell as Administrator**
* Ensure virtualization is enabled (WSL2 / Hyper-V / Docker driver installed)

---

## Step 1️⃣ Create Minikube Directory

```powershell
New-Item -Path 'C:\' -Name 'minikube' -ItemType Directory -Force
```

This creates a dedicated folder for the Minikube binary.

---

## Step 2️⃣ Download Minikube Binary

```powershell
$ProgressPreference = 'SilentlyContinue'; `
Invoke-WebRequest -OutFile 'C:\minikube\minikube.exe' `
-Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' `
-UseBasicParsing
```

This downloads the latest Minikube executable into `C:\minikube`.

---

## Step 3️⃣ Add Minikube to System PATH

```powershell
$oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)

if ($oldPath.Split(';') -inotcontains 'C:\minikube'){
  [Environment]::SetEnvironmentVariable(
    'Path',
    ('{0};C:\minikube' -f $oldPath),
    [EnvironmentVariableTarget]::Machine
  )
}
```

This ensures `minikube` can be executed from any directory in PowerShell or Command Prompt.

---

## Step 4️⃣ Verify Installation

Close and reopen PowerShell, then run:

```powershell
minikube version
```

If installed correctly, the version details will be displayed.

---

# 🎯 Final Result

Minikube is now installed and globally accessible on your Windows system.
You can start the cluster using:

```powershell
minikube start --driver=docker
```


