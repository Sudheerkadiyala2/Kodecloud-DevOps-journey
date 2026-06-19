# Task 47: Perform a Kubernetes Deployment Rolling Update and Rollback

**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** Kubernetes

## Challenge

The Nautilus DevOps team requested an application upgrade for an existing Kubernetes Deployment named `nginx-deployment`.

The task involved:

- Identifying the container name within the deployment.
- Updating the container image to `nginx:1.17`.
- Monitoring the rollout process.
- Verifying the deployment update.
- Performing a rollback to restore the previous stable version.

This exercise demonstrated how Kubernetes handles zero-downtime application updates and quick recovery using rollout rollback functionality.

---

## Solution

### Step 1: Verify the Deployment

Inspect the deployment and identify the container name.

```bash
kubectl get deployment nginx-deployment -o wide
```

### Step 2: Update the Container Image

Perform a rolling update to upgrade the application image.

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.17
```

Expected output:

```text
deployment.apps/nginx-deployment image updated
```

### Step 3: Monitor the Rollout

Track the update progress until completion.

```bash
kubectl rollout status deployment/nginx-deployment
```

Expected output:

```text
deployment "nginx-deployment" successfully rolled out
```

### Step 4: Verify the Updated Image

Confirm that the deployment is running the new image version.

```bash
kubectl get deployment nginx-deployment \
-o jsonpath='{.spec.template.spec.containers[0].image}'
```

Expected output:

```text
nginx:1.17
```

### Step 5: Roll Back the Deployment

Restore the previous working version.

```bash
kubectl rollout undo deployment/nginx-deployment
```

Expected output:

```text
deployment.apps/nginx-deployment rolled back
```

---

## Commands Used

```bash
kubectl get deployment nginx-deployment -o wide

kubectl set image deployment/nginx-deployment nginx=nginx:1.17

kubectl rollout status deployment/nginx-deployment

kubectl get deployment nginx-deployment \
-o jsonpath='{.spec.template.spec.containers[0].image}'

kubectl rollout undo deployment/nginx-deployment
```

---

## Verification

### Verify Deployment Status

```bash
kubectl get deployments
```

### Verify Rollout Status

```bash
kubectl rollout status deployment/nginx-deployment
```

### Verify Current Image

```bash
kubectl get deployment nginx-deployment \
-o jsonpath='{.spec.template.spec.containers[0].image}'
```

Expected output after update:

```text
nginx:1.17
```

### Verify Rollback

After executing the rollback command, check the image again:

```bash
kubectl get deployment nginx-deployment \
-o jsonpath='{.spec.template.spec.containers[0].image}'
```

Expected output:

```text
Previous stable image version restored
```

### View Rollout History

```bash
kubectl rollout history deployment/nginx-deployment
```

---

## Key Concepts Learned

### Rolling Update

A Rolling Update gradually replaces old Pods with new Pods, ensuring application availability during deployment.

Benefits:

- Zero downtime
- Controlled rollout
- Automatic replacement of Pods
- Easy monitoring

### Rollback

Rollback allows reverting a Deployment to its previous revision if an update introduces issues.

Benefits:

- Quick recovery
- Reduced downtime
- Safe deployment strategy
- Revision tracking

---

## Outcome

Successfully performed a Kubernetes Deployment Rolling Update by upgrading the application image to **nginx:1.17**, monitored the rollout process, verified the update, and safely restored the previous version using **kubectl rollout undo**. This demonstrated Kubernetes' built-in capabilities for zero-downtime deployments and rapid recovery through rollback mechanisms.
