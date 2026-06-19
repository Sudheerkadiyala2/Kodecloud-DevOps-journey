# Task 48: Troubleshoot and Fix Nginx + PHP-FPM Pod in Kubernetes
  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** Kubernetes Troubleshooting

## Challenge

The Nautilus DevOps team reported an issue with the Nginx and PHP-FPM application running in the Kubernetes cluster.

### Requirements

- Investigate the issue affecting the `nginx-phpfpm` pod.
- Identify and correct the configuration problem.
- Ensure the application becomes accessible again.
- Copy the `/home/thor/index.php` file from the jump host into the `nginx-container`.
- Place the file inside the Nginx document root.
- Verify successful website access.

### Environment Details

- Pod Name: `nginx-phpfpm`
- ConfigMap Name: `nginx-config`

---

## Investigation

First, inspect the Pod configuration to identify the issue.

```bash
kubectl get pod nginx-phpfpm -o yaml > pod.yaml
```

Review and modify the Pod configuration as required.

```bash
vi pod.yaml
```

After correcting the configuration issue, recreate the Pod.

---

## Solution

### Step 1: Export Existing Pod Configuration

```bash
kubectl get pod nginx-phpfpm -o yaml > pod.yaml
```

### Step 2: Edit the Pod Manifest

```bash
vi pod.yaml
```

Correct the configuration issue causing the Nginx and PHP-FPM setup to fail.

### Step 3: Replace the Pod

```bash
kubectl replace -f pod.yaml --force
```

Expected output:

```text
pod "nginx-phpfpm" deleted
pod/nginx-phpfpm replaced
```

### Step 4: Verify Pod Status

```bash
kubectl get pods
```

Expected output:

```text
NAME           READY   STATUS    RESTARTS   AGE
nginx-phpfpm   2/2     Running   0          XXs
```

### Step 5: Copy Website File

Copy the PHP application file from the jump host into the Nginx container document root.

```bash
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html -c nginx-container
```

### Step 6: Verify Website Functionality

```bash
kubectl exec -it nginx-phpfpm -c nginx-container -- curl -I http://localhost:8099
```

Expected output:

```text
HTTP/1.1 200 OK
Server: nginx
X-Powered-By: PHP
```

---

## Commands Used

```bash
kubectl get pod nginx-phpfpm -o yaml > pod.yaml

vi pod.yaml

kubectl replace -f pod.yaml --force

kubectl get pods

kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html -c nginx-container

kubectl exec -it nginx-phpfpm -c nginx-container -- curl -I http://localhost:8099
```

---

## Verification

### Verify Pod Health

```bash
kubectl get pods
```

Expected result:

```text
nginx-phpfpm   2/2   Running
```

### Verify File Copy

```bash
kubectl exec -it nginx-phpfpm -c nginx-container -- ls -l /var/www/html
```

Expected result:

```text
index.php
```

### Verify Nginx and PHP-FPM Integration

```bash
kubectl exec -it nginx-phpfpm -c nginx-container -- curl -I http://localhost:8099
```

Expected result:

```text
HTTP/1.1 200 OK
```

### Verify Website Access

Use the **Website** button provided in the KodeKloud lab environment.

The webpage should load successfully and execute the PHP application.

---

## Troubleshooting Notes

During the investigation:

- Exported the running Pod definition for analysis.
- Identified and corrected the configuration issue affecting communication between Nginx and PHP-FPM.
- Recreated the Pod using the updated configuration.
- Copied the required `index.php` file into the Nginx document root.
- Verified successful HTTP responses from the application.

---

## Outcome

Successfully diagnosed and fixed the Nginx + PHP-FPM configuration issue in the Kubernetes cluster. After recreating the Pod with the corrected configuration and copying the required `index.php` file into the document root, the application became fully accessible and returned a successful **HTTP 200 OK** response.
