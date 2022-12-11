- [Prerequisites](#prerequisites)
- [What the Chart will install](#what-the-chart-will-install)
- [Variables](#variables)
  - [Globals](#globals)
  - [Demo-app](#demo-app)
  - [Posgtresql](#posgtresql)
  - [Prometheus](#prometheus)
  - [AlertManager](#alertmanager)


# Prerequisites

1. Kubernetes version 1.25.2
2. Helm 3.10.2
3. Container Network Interface
4. A loadbalancer service (if using minikube run `minikube tunnel`)

# What the Chart will install

- Perceptolab's demo app version 0.0.2
- A postgresql statefulset (if you already have one set the `db_deploy` variable to `False` and set `postgres.namespace` to the namespace where your instance resides. If it's not on kubernetes set `db_deploy` to `False` and `postgres.external` to the url of the DB)
- A prometheus instance to monitor the app and a corresponding AlertManager instance 

# Variables

<br>

## Globals

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| db_deploy | bool | `true` | Set to false if you already have a kubernetes instance |
| db_external | bool | `false` | Set to true if you have an external db connection |
| db_external_address | string | `"changeme"` | Set address if you have an external DB |

<br>

## Demo-app

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| demo_app.autoscaling.enabled | bool | `false` |  |
| demo_app.configMap.name | string | `"demo-app-configs"` | ConfigMap name for demoapp |
| demo_app.fullname | string | `"devops-demo"` |  |
| demo_app.image.pullPolicy | string | `"IfNotPresent"` |  |
| demo_app.image.repository | string | `"ghcr.io/perceptolab/devops-demo-app"` | DevOps demo container image |
| demo_app.imagePullSecrets | object | `{}` |  |
| demo_app.labels.app | string | `"demo_app"` | Default label to select deployment |
| demo_app.namespace | string | `"default"` | Change if you want to run in a different namespace |
| demo_app.podAnnotations | object | `{}` |  |
| demo_app.podSecurityContext | object | `{}` |  |
| demo_app.replicaCount | int | `3` | Default number of replicas |
| demo_app.resources.limits.memory | string | `"1Gi"` | How much memory we need |
| demo_app.resources.requests.memory | string | `"1Gi"` | How much we request  |
| demo_app.secret.name | string | `"demo-app-secrets"` | Secret name for demoapp |
| demo_app.selectorLabels | object | `{"app":"demo_app"}` | Select deployment |
| demo_app.service.port | int | `80` | Exposes on port 80 externally |
| demo_app.service.targetPort | int | `8080` | To port 8080 |
| demo_app.service.type | string | `"LoadBalancer"` | LoadBalancer type |
| demo_app.serviceAccountName | object | `{}` |  |

<br>

## Posgtresql

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| postgres.autoscaling.enabled | bool | `false` | Disabling autoscaling |
| postgres.configMap.data.db_name | string | `"test_db1"` | What db is created, also sets demoapp DB_NAME var |
| postgres.configMap.data.pg_data | string | `"/data/pgdata"` | Postgres data dir |
| postgres.configMap.name | string | `"postgres-configs"` | ConfigMap name for postgres |
| postgres.fullname | string | `"postgresql-db"` |  |
| postgres.image.pullPolicy | string | `"IfNotPresent"` |  |
| postgres.image.repository | string | `"postgres"` | Postgres image |
| postgres.image.tag | string | `"latest"` | Using latest tag |
| postgres.imagePullSecrets | object | `{}` |  |
| postgres.labels.app | string | `"postgres"` | Label to select pod |
| postgres.namespace | string | `"default"` | If you're running in a different namespace or if you already have a postgresql instance |
| postgres.podAnnotations | object | `{}` |  |
| postgres.podSecurityContext.runAsUser | string | `"postgres"` |  |
| postgres.pv.hostPath | string | `"/mnt/data"` | Where we're saving the data on the host |
| postgres.pv.name | string | `"postgres-pv"` | Pv definition for postgres |
| postgres.pvClaim.accessModes | list | `["ReadWriteOnce"]` | AccessModes, change as needed. Also sets postgres.pv.accessModes |
| postgres.pvClaim.capacity | string | `"25Gi"` | Capacity, change as needed. Also sets postgres.pv.capacity |
| postgres.pvClaim.name | string | `"postgres-pv-claim"` | PvClaim for postgres |
| postgres.pvClaim.storageClass | string | `"manual"` | StorageClass, change as needed. Also sets postgres.pv.storageClass |
| postgres.replicaCount | int | `1` | Default number of replicas |
| postgres.resources.limits.cpu | string | `"500m"` |  |
| postgres.resources.limits.memory | string | `"512Mi"` |  |
| postgres.resources.requests.cpu | string | `"250m"` | Bare minimum resources to run postgres |
| postgres.resources.requests.memory | string | `"265Mi"` | Bare minimum resources to run postgres |
| postgres.secret.data.db_master_password | string | `"MCZBbmI1dDBvWWpnWHZOb3hZMkw="` | Postgress pass, also sets demoapp DB_PASS. B64 encoded, change if you already have a DB with the proper password |
| postgres.secret.data.db_user | string | `"dGVzdF91c2Vy"` | Postgres user, also sets demoapp DB_USER. B64 encoded, change if you already have a DB with the proper user |
| postgres.secret.name | string | `"postgres-secrets"` | Secret name for postgres |
| postgres.selectorLabels.app | string | `"postgres"` |  |
| postgres.service.port | int | `5432` |  |
| postgres.service.targetPort | int | `5432` |  |
| postgres.service.type | string | `"NodePort"` |  |
| postgres.serviceAccountName | object | `{}` |  |
| postgres.serviceName | string | `"postgres"` | ServiceName for StatefulSet |
| postgres.volumeMount.name | string | `"postgres-volume"` | VolumeMount name for postgres |
| postgres.volumeMount.path | string | `"/data"` | Path where vol is mounted |

<br>

## Prometheus

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| prometheus.autoscaling.enabled | bool | `false` | Disabling autoscaling |
| prometheus.configMap.config.name | string | `"prometheus-configs"` | ConfigMap name for prometheus |
| prometheus.fullname | string | `"prometheus"` |  |
| prometheus.image.pullPolicy | string | `"IfNotPresent"` |  |
| prometheus.image.repository | string | `"prom/prometheus"` | prometheus image |
| prometheus.image.tag | string | `"latest"` | Using latest tag |
| prometheus.imagePullSecrets | object | `{}` |  |
| prometheus.labels.app | string | `"prometheus"` | Label to select pod |
| prometheus.namespace | string | `"default"` | If you're running in a different namespace or if you already have a prometheusql instance |
| prometheus.podAnnotations | object | `{}` |  |
| prometheus.podSecurityContext | object | `{}` |  |
| prometheus.replicaCount | int | `1` | Default number of replicas |
| prometheus.resources.limits.cpu | int | `1` |  |
| prometheus.resources.limits.memory | string | `"1Gi"` |  |
| prometheus.resources.requests.cpu | string | `"500m"` | Bare minimum resources to run postgres |
| prometheus.resources.requests.memory | string | `"500M"` | Bare minimum resources to run prometheus |
| prometheus.rules | string | `"groups:\n- name: Probe-HTTP\n  rules:\n  - alert: Probe-HTTP\n    expr: avg_over_time(http_server_requests_seconds_sum[10m]) > 0\n    for: 1m\n    labels:\n      severity: warning\n    annotations:\n      summary: Blackbox probe slow HTTP (instance {{ $labels.instance }})\n      description: \"HTTP request took more than 5s\\n  VALUE = {{ $value }}\\n  LABELS = {{ $labels }}\"\n"` | Rules for Prometheus, change as needed. |
| prometheus.selectorLabels.app | string | `"prometheus"` |  |
| prometheus.service.port | int | `9090` |  |
| prometheus.service.targetPort | int | `9090` |  |
| prometheus.service.type | string | `"LoadBalancer"` |  |
| prometheus.serviceAccountName | object | `{}` |  |
| prometheus.serviceName | string | `"prometheus"` | ServiceName for StatefulSet |
| prometheus.volumeMount.config.name | string | `"prometheus-config"` | VolumeMount name for prometheus |
| prometheus.volumeMount.config.path | string | `"/etc/prometheus"` | Path where vol is mounted |
| prometheus.volumeMount.storage.name | string | `"prometheus-storage"` | VolumeMount name for prometheus |
| prometheus.volumeMount.storage.path | string | `"/prometheus"` | Path where vol is mounted |
<br>

## AlertManager

| Key | Type | Default | Description |
|-----|------|---------|-------------|
