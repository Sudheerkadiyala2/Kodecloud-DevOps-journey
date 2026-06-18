# Day 44: Create a Kubernetes Pod with Custom Labels and Container Name

**Date:** 2026-06-18  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** Kubernetes

## Challenge

The Nautilus DevOps team requested the creation of a Kubernetes Pod with the following specifications:

### Requirements

- Pod Name: `pod-nginx`
- Container Name: `nginx-container`
- Container Image: `nginx:latest`
- Label: `app=nginx_app`

The Pod needed to be deployed successfully and verified against all provided requirements.

---

## Solution

### Step 1: Remove Existing Pod

An existing Pod with the same name was already present and prevented modification of the container name. Delete it before creating the new Pod.

```bash
kubectl delete pod pod-nginx
```

### Step 2: Create the Pod Manifest

Create a file named `pod-nginx.yaml`.

```bash
vi pod-nginx.yaml
```

Add the following configuration:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-nginx
  labels:
    app: nginx_app
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
```

### Step 3: Deploy the Pod

Apply the manifest to create the Pod.

```bash
kubectl apply -f pod-nginx.yaml
```

---

## Manifest File

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-nginx
  labels:
    app: nginx_app
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
```

---

## Commands Used

```bash
kubectl delete pod pod-nginx

vi pod-nginx.yaml

kubectl apply -f pod-nginx.yaml

kubectl get pods --show-labels

kubectl get pod pod-nginx -o jsonpath='{.spec.containers[*].name}'
```

---

## Verification

### Verify Pod Status

```bash
kubectl get pods
```

Expected output:

```text
pod-nginx   Running
```

### Verify Labels

```bash
kubectl get pods --show-labels
```

Expected output:

```text
pod-nginx   ...   app=nginx_app
```

### Verify Container Name

```bash
kubectl get pod pod-nginx -o jsonpath='{.spec.containers[*].name}'
```

Expected output:

```text
nginx-container
```

### Verify Pod Details

```bash
kubectl describe pod pod-nginx
```

Confirm:

- Pod Name: `pod-nginx`
- Container Name: `nginx-container`
- Image: `nginx:latest`
- Label: `app=nginx_app`

---

## Outcome

Successfully created and deployed the Kubernetes Pod **pod-nginx** using a declarative YAML manifest. The Pod runs the **nginx:latest** image, uses the custom container name **nginx-container**, and includes the required label **app=nginx_app**, meeting all task requirements.
