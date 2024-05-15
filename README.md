# mac-minikube-config

Holds the Kubernetes manifests to set up a minikube cluster. Designed to be synced with ArgoCD running in the same
cluster.

Use the CLI or UI to create a new ArgoCD Application for each directory with manifests in it.

Check the README in each dir to see if deploying those manifests will require secrets to be set or any other set up.

## Creating secrets

```shell
# Replace [SECRET] with a the sensitive value to store. Generates a base64-encoded secret to go into the secret manifest
 secret="$(echo -n '[SECRET]' | base64)"
echo "${secret}"

# Create opaque secret
cat > /tmp/k8s.secret.yaml << EOL
---
apiVersion: v1
kind: Secret
metadata:
  name: name-of-secret
data:
  SECRET: '${secret}'
EOL

kubectl apply -f /tmp/k8s.secret.yaml

# Will display newly created secret
kubectl get secrets/name-of-secret -o yaml
```
