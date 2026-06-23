# Task 52 - Configure Environment Variables in a Kubernetes Pod

## Problem Statement

The Nautilus DevOps team needed to configure a Kubernetes Pod that prints a greeting message using environment variables. The Pod should execute a command that combines the values of the configured environment variables and display the output in the container logs.

## Requirements

- Create a Pod named `print-envars-greeting`
- Use a container named `print-env-container`
- Use the `bash` image
- Configure the following environment variables:
  - `GREETING=Welcome to`
  - `COMPANY=xFusionCorp`
  - `GROUP=Group`
- Use the following command:

```bash
["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
```

- Set `restartPolicy` to `Never`

## Solution

Created the following Pod manifest:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting
spec:
  containers:
  - name: print-env-container
    image: bash
    command: ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
    env:
    - name: GREETING
      value: "Welcome to"
    - name: COMPANY
      value: "xFusionCorp"
    - name: GROUP
      value: "Group"
  restartPolicy: Never
```

## Deployment

Apply the manifest:

```bash
kubectl apply -f pod.yaml
```

## Verification

Check the Pod status:

```bash
kubectl get pods
```

View the logs:

```bash
kubectl logs -f print-envars-greeting
```

Expected output:

```text
Welcome to xFusionCorp Group
```

## Result

Successfully created the `print-envars-greeting` Pod with the required environment variables. The container executed the specified command and printed the greeting message in the logs. The Pod completed successfully without entering a restart loop due to the `Never` restart policy.
