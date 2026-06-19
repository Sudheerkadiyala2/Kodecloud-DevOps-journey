# Day 46: Create a Kubernetes Pod with Resource Requests and Limits

**Date:** 2026-06-19  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** Kubernetes

## Challenge

The Nautilus DevOps team requested the creation of a Kubernetes Pod with specific resource requirements to ensure efficient resource utilization within the cluster.

### Requirements

- Pod Name: `httpd-pod`
- Container Name: `httpd-container`
- Image: `httpd:latest`
- Memory Request: `15Mi`
- CPU Request: `100m`
- Memory Limit: `20Mi`
- CPU Limit: `100m`

---

## Solution

### Step 1: Create the Pod Manifest

Create a file named `httpd-pod.yaml`.

```bash
vi httpd-pod.yaml
```

Add the following configuration:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
  - name: httpd-container
    image: httpd:latest
    resources:
      requests:
        memory: "15Mi"
        cpu: "100m"
      limits:
        memory: "20Mi"
        cpu: "100m"
```

### Step 2: Apply the Manifest

Create the Pod using the YAML file.

```bash
kubectl apply -f httpd-pod.yaml
```

Expected output:

```text
pod/httpd-pod created
```

---

## Pod Manifest

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
  - name: httpd-container
    image: httpd:latest
    resources:
      requests:
        memory: "15Mi"
        cpu: "100m"
      limits:
        memory: "20Mi"
        cpu: "100m"
```

---

## Commands Used

```bash
vi httpd-pod.yaml

kubectl apply -f httpd-pod.yaml

kubectl get pods

kubectl describe pod httpd-pod
```

---

## Verification

### Verify Pod Status

```bash
kubectl get pods
```

Expected output:

```text
NAME        READY   STATUS    RESTARTS   AGE
httpd-pod   1/1     Running   0          10s
```

### Verify Resource Requests and Limits

```bash
kubectl describe pod httpd-pod
```

Under the **Containers** section, verify:

```text
Requests:
  cpu:     100m
  memory:  15Mi

Limits:
  cpu:     100m
  memory:  20Mi
```

### Verify Container Details

Confirm:

- Pod Name: `httpd-pod`
- Container Name: `httpd-container`
- Image: `httpd:latest`
- Memory Request: `15Mi`
- CPU Request: `100m`
- Memory Limit: `20Mi`
- CPU Limit: `100m`

---

## Resource Configuration Explanation

| Resource | Request | Limit |
|-----------|---------|--------|
| Memory | 15Mi | 20Mi |
| CPU | 100m | 100m |

### Requests

Requests define the minimum resources Kubernetes reserves for the container.

- Memory Request: `15Mi`
- CPU Request: `100m` (0.1 CPU core)

### Limits

Limits define the maximum resources the container can consume.

- Memory Limit: `20Mi`
- CPU Limit: `100m` (0.1 CPU core)

---

## Outcome

Successfully created the Kubernetes Pod **httpd-pod** using the **httpd:latest** image and configured the required CPU and memory requests and limits. The Pod was deployed successfully and verified to be running with the specified resource constraints.
