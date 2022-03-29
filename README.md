# Minio

Object storage service, with Amazon S3 compatibility

## Use Minio Client (mc)

### Run mc and defining alias for remote

This repository only contains deployment definition for Minio Server, but you can run Minio Client locally with Docker and connect to remote Minio Server already deployed.

You only have to run `minio/mc` Docker container with shell as entrypoint and run any `mc` command you want. Remember to create an alias pointing to remote Minio Server.

```sh
docker run --rm -it --entrypoint=/bin/sh minio/mc

$ mc alias set minio https://minioapi.redmic.grafcan.es '<access-key>' '<secret-key>'
```

With this alias defined, you can run any command on remote Minio Server from your local Minio Client.

Check official docs at <https://docs.min.io/docs/minio-client-complete-guide>

### Set lifecycle rules

Manage bucket lifecycle with `ilm` command.

Add simple rule (30 days expiry time):

```sh
mc ilm add minio/<bucket> --expiry-days "30"
```

Add complex rule (30 days expiry time for objects with `test/` prefix):

```sh
mc ilm import minio/<bucket> <<EOF
{
    "Rules": [
        {
            "Expiration": {
                "Days": 30
            },
            "ID": "testFiles",
            "Filter": {
                "Prefix": "test/"
            },
            "Status": "Enabled"
        }
    ]
}
EOF
```

List lifecycle configuration set on a bucket:

```sh
mc ilm ls minio/<bucket>
```
