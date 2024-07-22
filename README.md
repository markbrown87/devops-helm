# (DevOps) Helm Charts
Custom Helm charts for varies services/applications.

## Coder Chart

Houses the umbrella chart that includes an IngressRoute for clusters using Traefik. 

### Installation
1. Install postgres bitname chart first.
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami && \
helm repo update && \
helm upgrade --install coder-db bitnami/postgresql \
    --namespace postgres \
    --create-namespace \
    --set auth.username=coder \
    --set auth.password=coder \
    --set auth.database=coder \
    --set persistence.size=10Gi \
    --version v15.5.17 \
    --atomic
```
2. Install Coder Umbrella Chart. Take note of the postgres information you changed above and apply them below either during install (similar to below) or by modiying [the values file](coder/values.yaml).
```bash
helm upgrade --install coder . \
    --namespace coder \
    --create-namespace \
    --set ingressRoute.url=coder.example.com \
    --set database.url=coder-db-postgresql.postgres.svc.cluster.local:5432 \
    --atomic
```

## Democratic CSI Values File
### Installation
Note that the IP Addresses below are localhost. Please change those - as well as the password for root - to what your TrueNAS is.
1. [Follow this guide](https://jonathangazeley.com/2021/01/05/using-truenas-to-provide-persistent-storage-for-kubernetes/) for NFS or iSCSI to setup your TrueNAS in preparation for Democratic CSI installation. Note that the values file is targetting ONLY NFS and thus should be configured differently using the values file or injection based on the guide linked above.
2. Install using the values file.
```bash
helm repo add democratic-csi https://democratic-csi.github.io/charts/ && \
helm repo update && \
helm upgrade --install democratic-csi democratic-csi/democratic-csi \
    --namespace csi \
    --create-namespace \
    --set driver.config.httpConnection.host=127.0.0.1 \
    --set driver.config.httpConnection.password="my-root-password" \
    --set driver.config.sshConnection.host=127.0.0.1 \
    --set driver.config.sshConnection.password="my-root-password" \
    --set driver.config.nfs.shareHost=127.0.0.1 \
    --version 0.14.6 \
    --atomic
```