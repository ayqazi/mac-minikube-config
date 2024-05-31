# mlflow

## Set up

### Tracking Server

Create a k8s secret called `mlflow` with key `BACKEND_STORE_URI` that points to the DB to use for the tracking server.
If using the in-cluster PostgreSQL server, it will look something like this:

```yaml
BACKEND_STORE_URI: 'postgresql+psycopg2://mlflow:<PASSWORD>@postgres/mlflow'
```

This assumes an in-cluster PostgreSQL DB called `mlflow` with user `mlflow`.

Check the top-level README for how to manage k8s secrets.

### Model Registry

The model registry directly mounts `/data/mlflow/artifacts` from the node as `artifacts`. This works fine on
minikube, but won't work so well in a real environment so do not imitate it if serving real users.

Some setup is required for `/data/mlflow/artifacts` which can be achieved by logging into the minikube node:

```shell
minikube ssh
sudo mkdir -p /data/mlflow/artifacts
```
