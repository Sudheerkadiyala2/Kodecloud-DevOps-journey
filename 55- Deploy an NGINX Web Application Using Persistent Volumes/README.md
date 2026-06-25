# Task 55: Deploy an NGINX Web Application Using Persistent Volumes

**Date:** 2026-06-20  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** Kubernetes Storage (Persistent Volumes & Persistent Volume Claims)

---

## Challenge

The Nautilus DevOps team needed a Kubernetes template to deploy an NGINX web application with persistent storage.

### Requirements

- Create a **PersistentVolume** named `pv-devops`.
- Configure:
  - Storage Class: `manual`
  - Capacity: `3Gi`
  - Access Mode: `ReadWriteOnce`
  - Volume Type: `hostPath`
  - Host Path: `/mnt/security`
- Create a **PersistentVolumeClaim** named `pvc-devops`.
- Request `1Gi` storage using the `manual` storage class.
- Create a Pod named `pod-devops`.
- Use the image `nginx:latest`.
- Container name: `container-devops`.
- Mount the PVC at the NGINX document root:
  ```
  /usr/share/nginx/html
  ```
- Create a **NodePort** Service named `web-devops`.
- Expose the application on **NodePort 30008**.

---

# Solution

## Step 1: Create the Manifest File

```bash
vi deployment.yaml
```

Add the following configuration:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-devops               # Name of the PersistentVolume

spec:
  storageClassName: manual      # Storage class used by this PV

  capacity:
    storage: 3Gi                # Total storage available

  accessModes:
    - ReadWriteOnce             # Read-write access from one node

  hostPath:
    path: /mnt/security         # Existing directory on the Kubernetes node

---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-devops              # Name of the PVC

spec:
  storageClassName: manual      # Must match the PV storage class

  accessModes:
    - ReadWriteOnce

  resources:
    requests:
      storage: 1Gi              # Request 1Gi from the PV

---
apiVersion: v1
kind: Pod
metadata:
  name: pod-devops
  labels:
    app: devops-web

spec:
  volumes:
    - name: web-storage
      persistentVolumeClaim:
        claimName: pvc-devops   # Connects the Pod to the PVC

  containers:
    - name: container-devops
      image: nginx:latest

      ports:
        - containerPort: 80

      volumeMounts:
        - name: web-storage
          mountPath: /usr/share/nginx/html   # NGINX document root

---
apiVersion: v1
kind: Service
metadata:
  name: web-devops

spec:
  type: NodePort

  selector:
    app: devops-web

  ports:
    - port: 80
      targetPort: 80
      nodePort: 30008
```

---

## Step 2: Apply the Manifest

```bash
kubectl apply -f deployment.yaml
```

Expected output:

```text
persistentvolume/pv-devops created
persistentvolumeclaim/pvc-devops created
pod/pod-devops created
service/web-devops created
```

---

# Commands Used

```bash
vi deployment.yaml

kubectl apply -f deployment.yaml

kubectl get pv

kubectl get pvc

kubectl get pods

kubectl get svc
```

---

# Verification

### Verify Persistent Volume

```bash
kubectl get pv
```

Expected output:

```text
NAME         CAPACITY   ACCESS MODES   STATUS
pv-devops    3Gi        RWO            Bound
```

---

### Verify Persistent Volume Claim

```bash
kubectl get pvc
```

Expected output:

```text
NAME          STATUS   VOLUME
pvc-devops    Bound    pv-devops
```

---

### Verify Pod

```bash
kubectl get pods
```

Expected output:

```text
NAME          READY   STATUS
pod-devops    1/1     Running
```

---

### Verify Service

```bash
kubectl get svc web-devops
```

Expected output:

```text
NAME          TYPE       PORT(S)
web-devops    NodePort   80:30008/TCP
```

---

### Verify Volume Mount

```bash
kubectl describe pod pod-devops
```

Confirm:

- PersistentVolumeClaim: `pvc-devops`
- Mounted Path:

```text
/usr/share/nginx/html
```

---

# Code Explanation

### PersistentVolume

```yaml
storageClassName: manual
```

```text
# Defines the storage class.
# PVC must use the same storage class to bind successfully.
```

```yaml
capacity:
  storage: 3Gi
```

```text
# Creates a PersistentVolume with 3Gi storage.
```

```yaml
hostPath:
  path: /mnt/security
```

```text
# Uses an existing directory on the Kubernetes node to store data.
```

---

### PersistentVolumeClaim

```yaml
resources:
  requests:
    storage: 1Gi
```

```text
# Requests 1Gi storage from the PersistentVolume.
```

---

### Pod

```yaml
persistentVolumeClaim:
  claimName: pvc-devops
```

```text
# Connects the Pod to the PersistentVolume using the PVC.
```

```yaml
volumeMounts:
  mountPath: /usr/share/nginx/html
```

```text
# Mounts the persistent storage at the NGINX document root.
# Website files stored here remain available after container restarts.
```

---

### Service

```yaml
type: NodePort
```

```text
# Exposes the application outside the Kubernetes cluster.
```

```yaml
nodePort: 30008
```

```text
# Opens port 30008 on every Kubernetes node.
```

---

# Architecture

```text
PersistentVolume (pv-devops)
        │
        ▼
PersistentVolumeClaim (pvc-devops)
        │
        ▼
Pod (pod-devops)
        │
        ▼
container-devops
        │
        ▼
/usr/share/nginx/html
        │
        ▼
NodePort Service (web-devops)
        │
        ▼
Port 30008
```

---

# Key Concepts Learned

- **PersistentVolume (PV):** Physical storage resource available to the cluster.
- **PersistentVolumeClaim (PVC):** Requests storage from a PersistentVolume.
- **hostPath:** Uses a directory on the Kubernetes node as storage.
- **Volume Mount:** Makes persistent storage accessible inside the container.
- **NodePort Service:** Exposes the application externally through a fixed port.

---

# Outcome

Successfully created a **PersistentVolume**, **PersistentVolumeClaim**, **Pod**, and **NodePort Service**. The NGINX container uses persistent storage mounted at its document root, ensuring data persistence while exposing the application externally through **NodePort 30008**.
