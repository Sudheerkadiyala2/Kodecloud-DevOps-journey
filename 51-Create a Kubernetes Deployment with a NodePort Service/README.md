# Task 51: Create a Kubernetes Deployment with a NodePort Service

**Date:** 2026-06-20  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** Kubernetes Deployments & Services

## Challenge

The Nautilus development team needed a highly available and scalable deployment for their static website.

### Requirements

- Create a Deployment named `nginx-deployment`.
- Use the image `nginx:latest`.
- Container name must be `nginx-container`.
- Configure `3` replicas.
- Create a NodePort Service named `nginx-service`.
- Expose the application using NodePort `30011`.

---

## Solution

### Step 1: Create the Deployment and Service Manifest

Create a file named `deployment.yaml`.

```bash
vi deployment.yaml
```

Add the following configuration:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx-container
        image: nginx:latest

---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service

spec:
  type: NodePort

  selector:
    app: nginx

  ports:
  - port: 80
    targetPort: 80
    nodePort: 30011
```

### Step 2: Apply the Manifest

```bash
kubectl apply -f deployment.yaml
```

Expected output:

```text
deployment.apps/nginx-deployment created
service/nginx-service created
```

---

## Commands Used

```bash
vi deployment.yaml

kubectl apply -f deployment.yaml

kubectl get deployments

kubectl get pods

kubectl get svc
```

---

## Verification

### Verify Deployment

```bash
kubectl get deployments
```

Expected output:

```text
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           XXs
```

### Verify Pods

```bash
kubectl get pods
```

Expected output:

```text
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-xxxxxxxxxx-xxxxx   1/1     Running   0          XXs
nginx-deployment-xxxxxxxxxx-xxxxx   1/1     Running   0          XXs
nginx-deployment-xxxxxxxxxx-xxxxx   1/1     Running   0          XXs
```

### Verify Service

```bash
kubectl get svc nginx-service
```

Expected output:

```text
NAME            TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)
nginx-service   NodePort   xxx.xxx.xxx.xx  <none>        80:30011/TCP
```

### Verify NodePort

```bash
kubectl describe svc nginx-service
```

Confirm:

```text
Type: NodePort
NodePort: 30011/TCP
```

---

## Deployment Manifest

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx-container
        image: nginx:latest

---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service

spec:
  type: NodePort

  selector:
    app: nginx

  ports:
  - port: 80
    targetPort: 80
    nodePort: 30011
```

---

## Key Concepts Learned

### Deployment

A Deployment provides:

- Declarative application management.
- Automatic pod creation.
- Self-healing capabilities.
- Rolling updates and rollbacks.
- Horizontal scalability.

### NodePort Service

A NodePort Service:

- Exposes an application externally.
- Opens a fixed port on every cluster node.
- Routes incoming traffic to backend pods.

### High Availability

By configuring:

```yaml
replicas: 3
```

Kubernetes ensures:

- Multiple application instances are running.
- Traffic is distributed across pods.
- Application remains available if a pod fails.

---

## Outcome

Successfully deployed a highly available NGINX application using a Kubernetes Deployment named **nginx-deployment** with **3 replicas**. Created a NodePort Service named **nginx-service** exposing the application on port **30011**, enabling external access while ensuring scalability and fault tolerance.
