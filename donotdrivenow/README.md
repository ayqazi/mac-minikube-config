# `donotdrivenow` app

## DB setup

Before creating this service, database connection details must be configured. A user and database owned by that user
should be created in the PostgreSQL instance created by `platform/db`. The database URI containing that user's username
and password, as well as the name of the database, must be put into a Kubernetes secret called `secure-donotdrivenow`.
The name of the variable should be `DB_URI`. It should look something like this (remember to base64 encrypt it before
putting in the secret though):

At time of writing, the DB server's hostname is set as `postgres`.

```
DB_URI: postgresql+asyncpg://username:password@postgres/database_name
```

Instructions for managing Kubernetes secrets are in the root README.md.

## Use with Kustomize

Update the image in deployment.yaml with:

```shell
kustomize edit set image kustomize-managed-image-name=x.org/repo/name:newtag
```
