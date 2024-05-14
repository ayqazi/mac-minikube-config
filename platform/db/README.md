# PostgreSQL instance used for experimentation. Do not run in production!

Requires a secret called `postgres-setup` in the namespace, with a key `initial_password`

```shell
# Replace [PASSWORD] with a PostgreSQL root password. Generates a base64-encoded password to go into the secret manifest
pw="$(echo -n '[PASSWORD]' | base64)"
echo "${pw}"

# Create opaque secret
cat > /tmp/postgres-setup.secret.yaml << EOL
---
apiVersion: v1
kind: Secret
metadata:
  name: postgres-setup
data:
  initial_password: '${pw}'
EOL

kubectl apply -f /tmp/postgres-setup.secret.yaml

# Will display newly created secret manifest if it was properly configured
kubectl get secrets/postgres-setup -o yaml
```
