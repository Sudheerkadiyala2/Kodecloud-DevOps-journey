# Task 54 - Troubleshoot and Fix Redis Deployment

## Problem Statement

The Nautilus DevOps team reported that the existing Redis application deployment was not running successfully on the Kubernetes cluster. The deployment name was `redis-deployment`, and the associated pod remained in a `Pending` state.

The objective was to identify the root cause of the issue and restore the application to a healthy running state.

## Investigation

### Check Pod Status

```bash
kubectl get pods
```

The Redis pod was stuck in the `Pending` state.

### Describe the Pod

```bash
kubectl describe pod redis-deployment-6bc546f779-62r6v
```

The Events section revealed the following error:

```text
Warning  FailedMount  MountVolume.SetUp failed for volume "config" : configmap "redis-conig" not found
```

## Root Cause

A typo was present in the Deployment configuration.

Incorrect ConfigMap reference:

```yaml
configMap:
  name: redis-conig
```

The ConfigMap name was misspelled as `redis-conig`.

## Fix Applied

Edited the deployment configuration:

```bash
kubectl edit deployment redis-deployment
```

Corrected the ConfigMap name from:

```yaml
redis-conig
```

to:

```yaml
redis-config
```

After saving the changes, Kubernetes automatically rolled out a new pod with the corrected configuration.

## Verification

### Check Deployment Status

```bash
kubectl get deployments
```

Output:

```text
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
redis-deployment   1/1     1            1           <age>
```

### Check Pod Status

```bash
kubectl get pods
```

Output:

```text
NAME                                READY   STATUS    RESTARTS   AGE
redis-deployment-xxxxxxxxxx-xxxxx   1/1     Running   0          <age>
```

### Verify Application Logs

```bash
kubectl logs deployment/redis-deployment
```

Output:

```text
Starting Redis Server
Ready to accept connections tcp
```

## Result

Successfully identified and fixed the issue preventing the Redis pod from starting. The root cause was a misspelled ConfigMap name (`redis-conig`). After correcting the reference in the Deployment configuration, the pod started successfully and Redis became available for connections.
