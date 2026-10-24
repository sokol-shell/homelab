# Kubernetes Homelab

A single-node k3s Kubernetes cluster where I self-host various applications.

## GitOps with Flux

[Flux](https://fluxcd.io/) watches the `master` branch of this repo and reconciles the clusters. The dependency diagram currently looks like this:

```
infrastructure-controllers   (infrastructure/controllers/staging)
├── infrastructure-configs   (infrastructure/configs/staging)
└── apps                     (apps/staging)

monitoring-controllers       (monitoring/controllers/staging)
```

Children wait for the controller above them to be healthy before they reconcile.

## Infrastructure

### 1. [Renovate](https://docs.renovatebot.com/)

A self-hosted Renovate bot runs as a CronJob in the cluster and opens pull requests whenever a newer version of helm charts, container image or any other dependencies comes out.

### 2. [CloudNativePG](https://cloudnative-pg.io/)

A Kubernetes operator that provisions and manages the PostgreSQL clusters used by the applications hosted here.

### 3. [cert-manager](https://cert-manager.io/) + [traefik](https://traefik.io/traefik)

cert-manager obtains a wildcard Let's Encrypt certificate through a Cloudflare DNS-01 challenge, and traefik serves it as the default TLS certificate for every ingress in the cluster.

### 4. [pgAdmin](https://www.pgadmin.org/)

A web-based administration UI, deployed via helm, for browsing and managing the PostgreSQL databases run by CloudNativePG.

## Monitoring and observability

### 1. [Kube prometheus stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack)

I installed the kube-prometheus-stack helm chart and use Grafana to monitor the whole cluster. CloudNativePG custom dashboard has been added to monitor Postgres database clusters.

## Apps

### 1. [Miniflux](https://miniflux.app/)

A minimalist RSS reader hosted as a single pod with a postgres database running in the cluster. The operator behind the postgres database is [CloudNativePG](https://cloudnative-pg.io/).

### 2. [Atuin](https://atuin.sh/)

A self-hosted sync server for Atuin. The database is in a CloudNativePG postgres cluster.

## Environments

Currently, a single-node staging environment is running until a second node comes where I'll create a prod environment and leave the staging for testing and development.
