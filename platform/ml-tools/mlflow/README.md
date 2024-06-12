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

### Model Registry and Files

The model registry is configured to store artifacts in `/srv/mlflow/artifacts`, which is mounted in the pods from the
same path on the host. This works well on Minikube which is expected to mount a local directory as `/srv`, thus allowing
the artifacts to persist across restarts/rebuilds of Minikube.

This works fine on Minikube for experimenting and learning, but is not a production-ready solution for the real world.

### Testing

[Test it and create test data by running this Python script that trains and logs a model](https://gist.github.com/ayqazi/beb5cea5dafc768a4522797105c9f72f)
