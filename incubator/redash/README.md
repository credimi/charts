# Redash

[Redash](http://redash.io/) is an open source tool built for teams to query, visualize and collaborate. Redash is quick to setup and works with any data source you might need so you can query from anywhere in no time.

## TL;DR

```bash
$ helm install incubator/redash
```

## Introduction

This chart bootstraps a [Redash](https://github.com/getredash/redash) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- At least 3 GB of RAM available on your cluster
- Kubernetes 1.4+ with Beta APIs enabled
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release incubator/redash
```

The command deploys Redash on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the Redash chart and their default values.

| Parameter                              | Description                                           | Default            |
|----------------------------------------|-------------------------------------------------------|--------------------|
| `image.registry`                       | Redash Image registry                                 | `docker.io`        |
| `image.repository`                     | Redash Image name                                     | `redash/redash`    |
| `image.tag`                            | Redash Image tag                                      | `{VERSION}`        |
| `image.pullPolicy`                     | Image pull policy                                     | `IfNotPresent`     |
| `image.pullSecrets`                    | Specify docker-ragistry secret names as an array      | `nil`              |
| `cookieSecret`                         | Secret used for cookie session management             | Randomly generated |
| `env`                                  | Environment variables from [Redash settings](https://redash.io/help-onpremise/setup/settings-environment-variables.html) and [example Docker Compose](https://github.com/getredash/redash/blob/master/docker-compose.production.yml). Variables applied to both server and worker containers. | `PYTHONUNBUFFERED: 0`<br>`REDASH_LOG_LEVEL: "INFO"` |
| `server.name`                          | Name used for Redash server deployment                | `redash`           |
| `server.env`                           | Environment variables from [Redash settings](https://redash.io/help-onpremise/setup/settings-environment-variables.html) and [example Docker Compose](https://github.com/getredash/redash/blob/master/docker-compose.production.yml). Variables applied to only server containers. | `REDASH_WEB_WORKERS: 4` |
| `server.replicaCount`                  | Number of Redash server replicas to start             | `1`                |
| `server.resources`                     | Server CPU/Memory resource requests/limits            | Memory `2GB`       |
| `server.nodeSelector`                  | Node labels for server pod assignment                 | `{}`               |
| `service.name`                         | Name used for service                                 | `redash`           |
| `service.type`                         | Kubernetes Service type                               | `ClusterIP`        |
| `service.externalPort`                 | Service external port                                 | `80`               |
| `service.internalPort`                 | Internal service and container port                   | `5000`             |
| `ingress.enabled`                      | Enable ingress controller resource                    | `false`            |
| `ingress.hosts`                        | Ingress resource hostnames                            | `nil`              |
| `ingress.annotations`                  | Ingress annotations configuration                     | `nil`              |
| `ingress.tls`                          | Ingress TLS configuration                             | `nil`              |
| `adhocWorker.name`                     | Name used for Redash ad-hoc worker deployment         | `redash-worker-adhoc` |
| `adhocWorker.env`                      | Environment variables from [Redash settings](https://redash.io/help-onpremise/setup/settings-environment-variables.html) and [example Docker Compose](https://github.com/getredash/redash/blob/master/docker-compose.production.yml). Variables applied to only ad-hoc worker containers. Default worker count will run 2 worker threads per-replica. | `QUEUES: "queries,celery"`<br>`WORKERS_COUNT: 2` |
| `adhocWorker.replicaCount`             | Number of Redash adhoc worker replicas to start       | `1`                |
| `adhocWorker.resources`                | Ad-hoc worker CPU/Memory resource requests/limits     | `nil`              |
| `adhocWorker.nodeSelector`             | Node labels for scheduled worker pod assignment       | `{}`               |
| `scheduledWorker.name`                 | Name used for Redash scheduled worker deployment      | `redash-worker-scheduled` |
| `scheduledWorker.env`                  | Environment variables from [Redash settings](https://redash.io/help-onpremise/setup/settings-environment-variables.html) and [example Docker Compose](https://github.com/getredash/redash/blob/master/docker-compose.production.yml). Variables applied to only scheduled worker containers. Default worker count will run 2 worker threads per-replica. | `QUEUES: "scheduled_queries"`<br>`WORKERS_COUNT: 2` |
| `scheduledWorker.replicaCount`         | Number of Redash scheduled worker replicas to start   | `1`                |
| `scheduledWorker.resources`            | Scheduled worker CPU/Memory resource requests/limits  | `nil`              |
| `scheduledWorker.nodeSelector`         | Node labels for scheduled worker pod assignment       | `{}`               |
| `postgresql.imageTag`                  | PostgreSQL image version                              | `9.5.6-alpine`     |
| `postgresql.postgresUser`              | PostgreSQL User to create                             | `redash`           |
| `postgresql.postgresPassword`          | PostgreSQL Password for the new user                  | `redash`           |
| `postgresql.postgresDatabase`          | PostgreSQL Database to create                         | `redash`           |
| `postgresql.persistence.enabled`       | Use a PVC to persist PostgreSQL data                  | `true`             |
| `postgresql.persistence.size`          | PVC Storage Request size for PostgreSQL volume        | `10Gi`             |
| `postgresql.persistence.accessMode`    | Use PostgreSQL volume as ReadOnly or ReadWrite        | `ReadWriteOnce`    |
| `postgresql.persistence.storageClass`  | Storage Class for PostgreSQL backing PVC              | `nil`<br>(uses alpha storage class annotation) |
| `postgresql.persistence.existingClaim` | Provide an existing PostgreSQL PersistentVolumeClaim  | `nil`              |
| `redis.redisPassword`                  | Redis Password to use                                 | `redash`           |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```bash
$ helm install --name my-release \
  --set cookieSecret=verysecret \
    incubator/redash
```

The above command sets the Redash cookie secret to `verysecret`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install --name my-release -f values.yaml incubator/redash
```
> **Tip**: You can use the default [values.yaml](values.yaml)