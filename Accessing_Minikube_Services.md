# Accessing Services in Minikube from Windows

When you create a service inside your Minikube cluster, it's only accessible within the cluster's internal network by default. To access it from your Windows browser, you need to expose it. Here are the two most common methods.

---

## Method 1: The `minikube service` Command (Easiest)

This is the simplest way to access a service. The command creates a network tunnel from your Windows machine to the service inside the cluster and automatically opens the URL in your default browser.

### Step-by-Step Example:

1.  **Start Minikube** (if not already running):

    ```powershell
    minikube start
    ```

2.  **Create a Deployment:**
    Let's create a simple Nginx deployment.

    ```powershell
    kubectl create deployment web-server --image=nginx
    ```

3.  **Expose the Deployment as a Service:**
    This creates a `ClusterIP` service, which is only reachable inside the cluster.

    ```powershell
    kubectl expose deployment web-server --type=ClusterIP --port=80
    ```

4.  **Access the Service:**
    Now, use the `minikube service` command.
    ```powershell
    minikube service web-server
    ```

**What Happens:**

- Minikube will print a table with a URL like `http://127.0.0.1:50123`.
- It will automatically open this URL in your browser, and you will see the Nginx welcome page.
- Leave the terminal window running. Closing it will terminate the tunnel.

---

## Method 2: Using a `NodePort` Service

This method manually exposes your service on a specific port on the Minikube node (the VM/container running Kubernetes). You then combine the node's IP address with that port to get the URL.

### Step-by-Step Example:

1.  **Expose the Deployment as a `NodePort` Service:**

    ```powershell
    kubectl expose deployment web-server --type=NodePort --port=80 --name=web-server-nodeport
    ```

2.  **Get the NodePort:**
    Kubernetes automatically assigns a high-numbered port (usually between 30000-32767). Find out which port was assigned.

    ```powershell
    kubectl get service web-server-nodeport
    ```

    The output will show a port mapping like `80:31569/TCP`. The high number (`31569` in this example) is your NodePort.

3.  **Get the Minikube IP Address:**

    ```powershell
    minikube ip
    ```

    This will return the IP address of your cluster, for example: `192.168.49.2`.

4.  **Construct the URL and Access:**
    Combine the IP address from step 3 and the NodePort from step 2. Open your browser and navigate to: `http://<minikube-ip>:<node-port>`

    Using our example values: `http://192.168.49.2:31569`
