# Task 53 - Deploy Grafana on Kubernetes and Expose via NodePort Service

## Problem Statement

The Nautilus DevOps team planned to deploy Grafana on a Kubernetes cluster for monitoring and analytics purposes. The Grafana application needed to be deployed using a Kubernetes Deployment and exposed externally using a NodePort Service.

## Requirements

1. Create a Deployment named `grafana-deployment-xfusion`.
2. Use a Grafana container image.
3. Create a NodePort Service to expose the application.
4. Configure the Service with `nodePort: 32000`.
5. Ensure the Grafana login page is accessible after deployment.

## Solution

### Deployment and Service Manifest

Created a file named `deployment.yaml` containing both the Deployment and Service definitions.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
      - name: grafana
        image: grafana/grafana
        ports:
        - containerPort: 3000

---
apiVersion: v1
kind: Service
metadata:
  name: grafana-service-xfusion
spec:
  type: NodePort
  selector:
    app: grafana
  ports:
  - port: 3000
    targetPort: 3000
    nodePort: 32000
```

## Deployment

Apply the manifest:

```bash
kubectl apply -f deployment.yaml
```

Output:

```text
deployment.apps/grafana-deployment-xfusion created
service/grafana-service-xfusion created
```

## Verification

### Check Deployment

```bash
kubectl get deployments
```

Output:

```text
NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
grafana-deployment-xfusion   1/1     1            1           22s
```

### Check Pod Status

```bash
kubectl get pods
```

Output:

```text
NAME                                          READY   STATUS    RESTARTS   AGE
grafana-deployment-xfusion-5fd578786b-dpwd5   1/1     Running   0          63s
```

### Check Service

```bash
kubectl get svc
```

Expected Output:

```text
NAME                      TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
grafana-service-xfusion   NodePort   <cluster-ip>    <none>        3000:32000/TCP   <age>
```

## Access Grafana

Open the Grafana login page using:

```text
http://<Node-IP>:32000
```

## Result

Successfully deployed Grafana using the `grafana-deployment-xfusion` Deployment and exposed it through the `grafana-service-xfusion` NodePort Service on port `32000`. The Grafana login page was accessible, confirming successful deployment and service configuration.
