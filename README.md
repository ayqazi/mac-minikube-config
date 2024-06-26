# Minikube Kubernetes Configuration and Manifests

Holds the Kubernetes manifests to set up the services on a local Minikube cluster. This allows playing around and
learning allows in a cheap and safe manner. Designed to be synced with ArgoCD running in the same cluster.

Use the CLI or UI to create a new ArgoCD Application for each directory with manifests in it (unless they are a base
directory used by other Kustomize directories).

Check the README in each dir to see if deploying those manifests will require secrets to be set or any other set up.

## Installation and Booting

Minikube requires a "driver" on which to run. These instructions assume using the Docker driver (the default). This
requires Docker Desktop to be setup, so install that first and ensure the `docker` command interacts with it.

### Mac Installation

```shell
brew install minikube kubectl
```

### Windows/WSL2 Installation

TODO

### Starting Minikube

Minikube by default runs in a Docker container so any files will be wiped if the Minikube cluster is deleted or
recreated. By mounting a local filesystem path into it, files can be persisted if this occurs. The flags to do this must
be supplied every time Minikube is started.

The manifests in this project assume that `/srv` on the Minikube node is a location where all important data will be
persisted across reboots and recreations of the cluster. Thus, applications should mount their data directories within
`/srv` on the host.

```shell
MINIKUBE_SRV_DIR=/path/outside/minikube/to/persist/files

# will use default Docker driver with Docker Desktop
minikube start --mount --mount-string=$MINIKUBE_SRV_DIR:/srv
```

It will be easier to create a startup script that starts Minikube with the mount flags.

Cross-namespace security can be disabled with this command - this makes it easier to learn and play with the cluster,
and should not be done in a real production environment:

```shell
kubectl create rolebinding default-admin --clusterrole=admin --serviceaccount=default:default # do NOT ever do this in production!
```

## Knowledge Base

### Creating secrets

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

### Deploy a new version of an application

Go into the application directory, and run:

```shell
kustomize edit set image kustomize-managed-image-name='ghcr.io/[ACCOUNT]/[REPO]:[REVISION]'
```

Then commit and push the `kustomization.yaml` file.

### Override deployed image using parameter overrides in ArgoCD

It is allowed to dynamically change the deployed `dev` version of an ArgoCD application. Here is how:

```shell
argocd app set [APP]-dev --kustomize-image image-name-in-manifest=[HOST_AND_ACCOUNT]/[REPO]:[REVISION]
argocd app sync [APP]-dev
```

This is a convenient way of deploying a different version to a dev branch for testing.

Unset with:

```shell
argocd app unset donotdrivenow-dev --kustomize-image kustomize-managed-image-name
```

In all other cases, commit the change to the repo.
