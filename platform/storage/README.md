# MinIO

## Set up

Create a k8s secret called `minio-setup` in the namespace, with the key `MINIO_ROOT_PASSWORD`. This will be the password
of the `admin` user and used for logging into the web UI. Instructions for creating Kubernetes secrets are in the root
README.md.

## Files

The service is configured to store files from `/srv/minio/data` on the Minikube host. If Minikube is configured
correctly and maps `/srv` to the host, then these files will be persisted across reboots and recreations of the cluster.

## Connecting

From inside the Kubernetes cluster, the API can be accessed using the hostname `minio`, e.g.

```shell
aws s3 ls s3:// --endpoint-url http://minio:9000/
```

To access the web UI on port 9001 and the API on 9000, run the following:

```shell
kubectl port-forward services/minio 9000:9000 9001:9001
```

Now the web UI can be accessed on http://127.0.0.1:9000/. Use username `admin` and the password created above.

The API can be accessed on http://127.0.0.1:9001/, e.g.

```shell
aws s3 ls s3:// --endpoint-url http://127.0.0.1:9001/
```

Create keys in the web UI and add them to your profile or environment to use the service.
