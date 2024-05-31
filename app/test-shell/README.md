# Test shell

Create a pod that can be logged into for testing the Kubernetes environment.

To get a shell:

```shell
kubectl exec -it deployments/test-shell -- /bin/bash
```

It is also useful for testing various components - e.g. if persistent volumes are not working correctly, try attaching
them to this deployment and investigate them by logging into the container
