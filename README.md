# mac-minikube-config

Holds the Kubernetes manifests to set up the minikube cluster on my local Mac. Synced with ArgoCD, also running on the
same minikube cluster.

If using ArgoCD, create a new ArgoCD Application for each dir with the command-line or the UI.

Check the README in each dir to see if deploying those manifests will require any secrets.

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
