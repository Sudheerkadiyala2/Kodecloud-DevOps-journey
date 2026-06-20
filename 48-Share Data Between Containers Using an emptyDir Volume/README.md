# Day 48: Share Data Between Containers Using an emptyDir Volume

**Date:** 2026-06-20  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** Kubernetes Storage

## Challenge

The Nautilus DevOps team needed to create a Pod containing multiple containers that share temporary data using a common volume.

### Requirements

- Create a Pod named `volume-share-devops`.
- Create two containers using the image `fedora:latest`.
- Container 1:
  - Name: `volume-container-devops-1`
  - Mount shared volume at `/tmp/blog`
  - Run a sleep command to keep the container running.
- Container 2:
  - Name: `volume-container-devops-2`
  - Mount shared volume at `/tmp/apps`
  - Run a sleep command to keep the container running.
- Create a shared volume named `volume-share` using the `emptyDir` volume type.
- Create a file named `blog.txt` in the first container with the content:
  
  ```text
  Welcome to xFusionCorp Industries
  ```

- Verify that the same file is accessible from the second container through the shared volume.

---

## Solution

### Step 1: Create the Pod Manifest

Create a file named `pod.yaml`.

```bash
vi pod.yaml
```

Add the following configuration:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-devops
spec:
  containers:
  - name: volume-container-devops-1
    image: fedora:latest
    command: ["/bin/bash", "-c", "sleep 3600"]
    volumeMounts:
    - name: volume-share
      mountPath: /tmp/blog

  - name: volume-container-devops-2
    image: fedora:latest
    command: ["/bin/bash", "-c", "sleep 3600"]
    volumeMounts:
    - name: volume-share
      mountPath: /tmp/apps

  volumes:
  - name: volume-share
    emptyDir: {}
```

### Step 2: Create the Pod

```bash
kubectl apply -f pod.yaml
```

Expected output:

```text
pod/volume-share-devops created
```

### Step 3: Verify Pod Status

```bash
kubectl get pods
```

Expected output:

```text
NAME                  READY   STATUS    RESTARTS   AGE
volume-share-devops   2/2     Running   0          XXs
```

### Step 4: Create the Test File

Create the file inside the first container.

```bash
kubectl exec -it volume-share-devops -c volume-container-devops-1 -- \
sh -c "echo 'Welcome to xFusionCorp Industries' > /tmp/blog/blog.txt"
```

### Step 5: Verify Shared Volume Functionality

Access the file from the second container.

```bash
kubectl exec -it volume-share-devops -c volume-container-devops-2 -- \
cat /tmp/apps/blog.txt
```

Expected output:

```text
Welcome to xFusionCorp Industries
```

---

## Pod Manifest

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-devops
spec:
  containers:
  - name: volume-container-devops-1
    image: fedora:latest
    command: ["/bin/bash", "-c", "sleep 3600"]
    volumeMounts:
    - name: volume-share
      mountPath: /tmp/blog

  - name: volume-container-devops-2
    image: fedora:latest
    command: ["/bin/bash", "-c", "sleep 3600"]
    volumeMounts:
    - name: volume-share
      mountPath: /tmp/apps

  volumes:
  - name: volume-share
    emptyDir: {}
```

---

## Commands Used

```bash
vi pod.yaml

kubectl apply -f pod.yaml

kubectl get pods

kubectl exec -it volume-share-devops -c volume-container-devops-1 -- \
sh -c "echo 'Welcome to xFusionCorp Industries' > /tmp/blog/blog.txt"

kubectl exec -it volume-share-devops -c volume-container-devops-2 -- \
cat /tmp/apps/blog.txt
```

---

## Verification

### Verify Pod Status

```bash
kubectl get pods
```

Expected result:

```text
volume-share-devops   2/2   Running
```

### Verify Shared Volume

Create the file in Container 1:

```bash
kubectl exec -it volume-share-devops -c volume-container-devops-1 -- \
ls -l /tmp/blog
```

Verify the same file exists in Container 2:

```bash
kubectl exec -it volume-share-devops -c volume-container-devops-2 -- \
ls -l /tmp/apps
```

### Verify File Contents

```bash
kubectl exec -it volume-share-devops -c volume-container-devops-2 -- \
cat /tmp/apps/blog.txt
```

Expected output:

```text
Welcome to xFusionCorp Industries
```

---

## Key Concepts Learned

### emptyDir Volume

An `emptyDir` volume is created when a Pod starts and exists as long as the Pod is running.

Features:

- Shared among all containers within the Pod.
- Temporary storage.
- Data is deleted when the Pod is removed.
- Useful for caching and inter-container communication.

### Volume Sharing

Both containers mount the same volume at different paths:

| Container | Mount Path |
|------------|------------|
| volume-container-devops-1 | `/tmp/blog` |
| volume-container-devops-2 | `/tmp/apps` |

Any file created by one container becomes immediately visible to the other container.

---

## Outcome

Successfully created the Pod **volume-share-devops** with two Fedora containers sharing an **emptyDir** volume. A test file created in the first container was immediately accessible from the second container, confirming successful volume sharing and inter-container communication within the Pod.
