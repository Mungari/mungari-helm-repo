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

## Globals


