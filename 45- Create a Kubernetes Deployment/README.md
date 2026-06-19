# Task 45: Create a Kubernetes Deployment

**Date:** 2026-06-19  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** Kubernetes

## Challenge

The Nautilus DevOps team requested the creation of a Kubernetes Deployment with the following specifications:

### Requirements

- Deployment Name: `nginx`
- Image: `nginx:latest`
- Replicas: `1`

The deployment should successfully create and manage the pod using the specified NGINX image.

---

## Solution

### Step 1: Create the Deployment Manifest

Create a file named `deployment.yaml`.

```bash
vi deployment.yaml
```

Add the following configuration:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx
  name: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx:latest
        name: nginx
```

### Step 2: Apply the Deployment

Deploy the manifest to the Kubernetes cluster.

```bash
kubectl apply -f deployment.yaml
```

Expected output:

```text
deployment.apps/nginx created
```

---

## Deployment Manifest

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx
  name: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx:latest
        name: nginx
```

---

## Commands Used

```bash
vi deployment.yaml

kubectl apply -f deployment.yaml

kubectl get deployments

kubectl get pods
```

---

## Verification

### Verify Deployment

```bash
kubectl get deployments
```

Sample output:

```text
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   1/1     1            1           10s
```

### Verify Pod Creation

```bash
kubectl get pods
```

Sample output:

```text
NAME                     READY   STATUS    RESTARTS   AGE
nginx-xxxxxxxxxx-xxxxx   1/1     Running   0          10s
```

### Verify Deployment Details

```bash
kubectl describe deployment nginx
```

Confirm:

- Deployment Name: `nginx`
- Image: `nginx:latest`
- Replicas: `1`
- Pod Status: `Running`

---

## Outcome

Successfully created and deployed a Kubernetes Deployment named **nginx** using the **nginx:latest** image. The deployment manages the pod lifecycle, maintains the desired replica count, and ensures the application remains available within the Kubernetes cluster.
