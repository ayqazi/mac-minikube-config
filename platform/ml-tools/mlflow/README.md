# mlflow

## Set up

Create a k8s secret called `mlflow` with key `BACKEND_STORE_URI` that points to the DB to use for the tracking server.
If using the in-cluster PostgreSQL server, it will look something like this:

```yaml
BACKEND_STORE_URI: 'postgresql+psycopg2://mlflow:<PASSWORD>@postgres/mlflow'
```

This assumes an in-cluster PostgreSQL DB called `mlflow` with user `mlflow`.

Check the top-level README for how to manage k8s secrets.
