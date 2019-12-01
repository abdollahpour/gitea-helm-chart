# Gitea

[Gitea](https://gitea.io) is a community managed lightweight code hosting solution written in Go. It is published under the MIT license.

clone the repo then create you own configuration file. For example (char-value.yaml):

```
ingress:
  tls:
    - hosts:
      - your-domain.com
      secretName: gitea-tls

  annotations:
    kubernetes.io/ingress.class: "traefik"
    # SSL Will not work if you don't have cer-manager & letsencrypt-prod cluster-issuer installed
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
    # If you don't have SSL config, disable this line
    ingress.kubernetes.io/ssl-redirect: "true"
  hosts:
    - host: your-domain.com
```
Then run:
```console
$ helm install --name gitea --namespace gitea .
```

## Introduction

This chart uses [PostgreSQL](https://github.com/helm/charts/tree/master/stable/postgresql) for data storage.

## Prerequisites

- Kubernetes 1.10+
- PV provisioner support in the underlying infrastructure

## Installing the Chart
To install the chart with the release name `my-release`:

```console
$ helm install --name my-release .
```

The command deploys Gitea on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following tables lists the configurable parameters of the gitea chart and their default values.

| Parameter                                     | Description                                                                                                            | Default                                                     |
| --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `replicaCount`                                | Replication count                                                                                                      | `1`                                                         |
| `image.repository`                            | Gitea Image name                                                                                                       | `bitnami/postgresql`                                        |
| `image.tag`                                   | Gitea Image tag                                                                                                        | `{TAG_NAME}`                                                |
| `image.pullPolicy`                            | Gitea Image pull policy                                                                                                | `IfNotPresent`                                              |
| `image.pullSecrets`                           | Specify Image pull secrets                                                                                             | `nil` (does not add image pull secrets to deployed pods)    |
| `nameOverride`                                | String to partially override gitea.fullname template with a string (will prepend the release name)                     | `nil`                                                       |
| `service.type`                                | Kubernetes Service type                                                                                                | `ClusterIP`                                                 |
| `service.httpPort`                            | HTTP port                                                                                                              | `80`                                                        |
| `service.sshPort`                             | SSH port                                                                                                               | `2200`                                                      |
| `ingress.enabled`                             | Active or disable ingres                                                                                               | `true`                                                      |
| `ingress.certManager`                         | Active cert-manager to use SSL                                                                                         | `false`                                                     |
| `ingress.annotations`                         | Extra ingres annotation                                                                                                | `[]`                                                        |
| `ingress.host`                                | Hosted domain                                                                                                          | `your_domain`                                               |
| `ingress.path`                                | URL Path                                                                                                               | `/`                                                         |
| `resources`                                   | Deployment resources                                                                                                   | `{}`                                                        |
| `nodeSelector`                                | Deployment nodeSelector                                                                                                | `{}`                                                        |
| `tolerations`                                 | Deployment tolerations                                                                                                 | `[]`                                                        |
| `affinity`                                    | Deployment affinity                                                                                                    | `{}`                                                        |
| `postgresql.postgresqlUsername`               | Password username                                                                                                      | `postgres`                                                  |
| `postgresql.postgresqlPassword`               | Postgres password                                                                                                      | `postgres`                                                  |
| `postgresql.postgresqlDatabase`               | Postgres database name                                                                                                 | `postgres`                                                  |
| `persistence.giteaData.enabled`               | Enable permanent storage                                                                                               | `true`                                                      |
| `persistence.giteaData.size`                  | Size of the storage                                                                                                    | `5Gi`                                                       |
| `persistence.giteaData.accessMode`            | If defined, volume.beta.kubernetes.io/storage-class: <storageClass>                                                    | `ReadWriteOnce`                                             |
