# Deploying NGINX with Argo CD Using the UI

This project demonstrates how to deploy and manage an **NGINX application on Kubernetes using Argo CD through the Argo CD Web UI**.

Instead of creating the Argo CD Application using the CLI or a declarative Application manifest, this approach uses the **Argo CD UI** to configure the Git repository, Kubernetes destination, namespace, and synchronization settings.

## 📁 Project Structure

```text
app-deployment/
├── cli-approach/
├── declarative-approach/
└── ui-approach/
    ├── nginx-deployment.yml
    ├── nginx-service.yml
    └── README.md
```

## 🏗️ Architecture

```text
GitHub Repository
       │
       │ Git Repository
       ▼
    Argo CD
   Web UI
       │
       │ Sync
       ▼
Kubernetes Cluster
       │
       ├── NGINX Deployment
       └── NGINX Service
```

## 📄 Kubernetes Manifests

The `ui-approach` directory contains the Kubernetes manifests managed by Argo CD.

### NGINX Deployment

File:

```text
nginx-deployment.yml
```

The Deployment manages the NGINX Pods and ensures the desired number of replicas are running.

Example components:

- NGINX container
- Deployment configuration
- Replica management
- Pod labels and selectors

### NGINX Service

File:

```text
nginx-service.yml
```

The Service exposes the NGINX application inside or outside the Kubernetes cluster depending on the configured Service type.

---

# 🚀 Prerequisites

Before starting, ensure you have:

- A Kubernetes cluster
- Argo CD installed in the cluster
- Access to the Argo CD Web UI
- A Git repository containing the Kubernetes manifests
- `kubectl` configured to access the Kubernetes cluster

Check your cluster:

```bash
kubectl get nodes
```

Check Argo CD Pods:

```bash
kubectl get pods -n argocd
```

---

# 🌐 Access the Argo CD UI

If Argo CD is exposed using port forwarding:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Open:

```text
https://localhost:8080
```

## Get the Argo CD Admin Password

```bash
kubectl -n argocd get secret argocd-initial-admin-secret \
-o jsonpath="{.data.password}" | base64 -d
```

Login using:

```text
Username: admin
Password: <decoded-password>
```

---

# 🖥️ Create an Application Using the Argo CD UI

After logging into the Argo CD dashboard:

### 1. Click **NEW APP**

Click the **+ NEW APP** button.

### 2. Configure General Settings

Enter an application name:

```text
nginx-ui-app
```

Select the appropriate project:

```text
default
```

### 3. Configure the Source

Select:

```text
Repository URL:
```

Enter your GitHub repository URL:

```text
https://github.com/rajendrakmr/argocd.git
```

Specify the revision:

```text
main
```

Specify the manifest path:

```text
app-deployment/ui-approach
```

### 4. Configure the Destination

Select your Kubernetes cluster:

```text
https://kubernetes.default.svc
```

Specify the namespace:

```text
default
```

Alternatively, choose another namespace such as:

```text
nginx
```

If using a new namespace, create it:

```bash
kubectl create namespace nginx
```

### 5. Configure Sync Policy

Choose one of the following:

### Manual Sync

Argo CD detects Git changes, but deployment requires manual synchronization.

### Automatic Sync

Argo CD automatically synchronizes Kubernetes resources when changes are pushed to Git.

Recommended for GitOps practice:

```text
Automated Sync
```

---

# 🔄 Sync the Application

After creating the application:

1. Open the `nginx-ui-app` application.
2. Review the Deployment and Service resources.
3. Click **SYNC**.
4. Click **SYNCHRONIZE**.

Argo CD will apply the Kubernetes manifests from the Git repository to the Kubernetes cluster.

---

# ✅ Verify the Deployment

Check the Deployment:

```bash
kubectl get deployment
```

Check the Pods:

```bash
kubectl get pods
```

Check the Service:

```bash
kubectl get svc
```

Example:

```text
NAME                     READY   UP-TO-DATE   AVAILABLE
nginx-deployment         2/2     2            2
```

---

# 🔍 Check Application Status in Argo CD

After synchronization, Argo CD should display:

```text
Health Status: Healthy
Sync Status: Synced
```

### Sync Status

```text
Synced
```

Means the Kubernetes cluster matches the desired state stored in Git.

### Health Status

```text
Healthy
```

Means the deployed Kubernetes resources are running correctly.

---

# 🔄 GitOps Workflow

```text
Developer
    │
    │ Push Kubernetes Manifest
    ▼
GitHub Repository
    │
    ▼
Argo CD
    │
    │ Detect Changes
    ▼
Sync Application
    │
    ▼
Kubernetes Cluster
    │
    ▼
NGINX Deployment
```

## Example Workflow

Update the number of replicas in:

```text
nginx-deployment.yml
```

For example:

```yaml
spec:
  replicas: 3
```

Commit and push the changes:

```bash
git add .
git commit -m "Scale NGINX deployment to 3 replicas"
git push
```

Then:

- With **Manual Sync**, click **SYNC** in the Argo CD UI.
- With **Automatic Sync**, Argo CD automatically applies the changes.

Verify:

```bash
kubectl get pods
```

---

# 📊 Key Concepts Demonstrated

- GitOps workflow
- Argo CD Application creation using the Web UI
- Kubernetes Deployment management
- Kubernetes Service management
- Git repository as the source of truth
- Manual and automated synchronization
- Application health monitoring
- Git-based infrastructure and application deployment

---

# 🆚 Deployment Approaches

This repository demonstrates multiple ways to deploy applications using Argo CD:

| Approach | Description |
|---|---|
| CLI Approach | Create and manage Argo CD applications using the `argocd` CLI |
| Declarative Approach | Define Argo CD Application resources using YAML |
| UI Approach | Create and manage applications using the Argo CD Web UI |

---

# 🧹 Delete the Application

You can delete the application directly from the Argo CD UI.

Alternatively, delete the Kubernetes resources:

```bash
kubectl delete -f nginx-deployment.yml
kubectl delete -f nginx-service.yml
```

---

# 🎯 Learning Outcome

After completing this project, you will understand how to:

- Deploy Kubernetes applications using Argo CD.
- Create Argo CD Applications through the Web UI.
- Connect a Git repository to Argo CD.
- Synchronize Git manifests with a Kubernetes cluster.
- Monitor application health and synchronization status.
- Manage application changes using a GitOps workflow.

---

## 👤 Author

**Rajendra Kumar Marandi**

- GitHub: `https://github.com/rajendrakmr`
- LinkedIn: `https://linkedin.com/in/rajendraakmr`
- Role: DevOps Engineer | AWS | Kubernetes | Terraform | CI/CD | DevSecOps