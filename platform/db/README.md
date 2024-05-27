# PostgreSQL instance used for experimentation. Do not run in production!

## Set up

Requires a secret called `postgres-setup` in the namespace, with a key `POSTGRES_PASSWORD`. Instructions for managing
Kubernetes secrets are in the root README.md

## Connecting

Port forward to the instance with:

```shell
kubectl port-forward services/postgres 5433:5432
```

Then you can connect to it on port 5433.

If you just want to manage the cluster, start a shell on the pod so you can run commands:

```shell
kubectl exec -it services/postgres -- su - postgres
```

## Users

Using the above technique to log into a shell, create a separate database with its own user for each service:

```shell
createuser -P <USERNAME>
# Enter password
createdb -O <USERNAME> <DBNAME>
```

You can also create a separate user/DB for every environment of a service.
