# Github Action to Build and Publish Backstage TechDocs into a Minio S3 Compatible Server

This action is inspired on [backstage-techdocs-action](https://github.com/Staffbase/backstage-techdocs-action)

## Features

Builds techdocs from mkdocs.yaml and docs/, at repo root level
Publishes then into a Minio S3 compatible bucket, under BUCKET_NAME/backstage-namespace/backstage-kind/backstage-entity-name

## Usage

```yaml
name: Publish TechDocs Site

on:
  push:
    branches:
      - main
    paths:
      - 'docs/**'
      - 'mkdocs.yml'
      - '.github/workflows/techdocs.yaml'

jobs:
  publish-techdocs-site:
    name: Publish TechDocs Site
    runs-on: ubuntu-20.04
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Build TechDocs
        uses: gabtec/backstage-techdocs-minio-action:v0.0.2
        with:
          entity-name: example # the entity name in backstage
          entity-kind: 'Service' # the entity kind, default is Component
          entity-namespace: 'default' # the namespace, 'default' is the default
          storage-name: 'techdocs' # bucket name at your choice (Must already exist)
          aws-access-key-id: ${{ secrets.S3_ACCESS_KEY }} # Minio accessKeyId
          aws-secret-access-key: ${{ secrets.S3_SECRET_KEY }} # Minio accessSecretKey
          aws-server-url: ${{ secrets.S3_SERVER_URL }} # e.g. https://minio.example.org:9000 (api)
```

## Config Variables

| Name                    | Description                                                                        | Required | Default     |
| ----------------------- | ---------------------------------------------------------------------------------- | -------- | ----------- |
| `entity-namespace`      | Entity namespace in Backstage                                                      | `true`   | `default`   |
| `entity-kind`           | Kind of the Backstage entity                                                       | `true`   | `Component` |
| `entity-name`           | Name of the Backstage entity                                                       | `true`   |             |
| `storage-name`          | The bucket name                                                                    | `true`   | `techdocs`  |
| `aws-access-key-id`     | Minio Access Key ID                                                                | `true`   |             |
| `aws-secret-access-key` | Minio Secret Access Key                                                            | `true`   |             |
| `aws-server-url`        | Minio API Server Url                                                               | `true`   |             |
| `additional-plugins`    | Space separated list of additional python plugins (Bash quoting for special chars) | `false`  |             |
| `skip-publish`          | Indicates whether publish step should be skipped                                   | `false`  | `false`     |

## License

This project is licensed under the Apache-2.0 License - see the LICENSE.md file for details.
