# django-app Helm Chart

A production-ready Helm chart for the container-aware Django application.

## Features

| Template | Always rendered | Toggle |
|---|---|---|
| `ServiceAccount` | ✅ | `serviceAccount.create` |
| `ConfigMap` | ✅ | — |
| `Deployment` | ✅ | — |
| `Service` | ✅ | — |
| `ApisixRoute` + `ApisixTls` | ⬜ | `apisix.enabled=true` |
| `AzureLoadBalancerBackendPool` | ⬜ | `azureLoadBalancer.enabled=true` |
| `HorizontalPodAutoscaler` | ⬜ | `autoscaling.enabled=true` |

---

## Quick start

```bash
# Dry-run to see what will be rendered
helm template django-app ./django-app-chart

# Install with defaults
helm upgrade --install django-app ./django-app-chart -n my-namespace --create-namespace

# Staging
helm upgrade --install django-app ./django-app-chart \
  -f examples/values-staging.yaml -n staging --create-namespace

# Production (APISIX + Azure LB + HPA)
helm upgrade --install django-app ./django-app-chart \
  -f examples/values-production.yaml -n production --create-namespace
```

---

## Configuring the application

Values under `appConfig` are rendered into a `config.json` file that gets
mounted at `/etc/app/config.json` inside every pod. The application reads that
file first, then falls back to environment variables.

```bash
# Change the accent colour at deploy time
helm upgrade --install django-app ./django-app-chart \
  --set appConfig.accentColor="#ff6b35" \
  --set appConfig.message="Hello from Helm!"
```

---

## APISIX route

Requires the [APISIX Ingress Controller](https://apisix.apache.org/docs/ingress-controller/getting-started/) and its CRDs.

```bash
helm upgrade --install django-app ./django-app-chart \
  --set apisix.enabled=true \
  --set apisix.host=django-app.example.com \
  --set apisix.tls.enabled=true \
  --set apisix.tls.secretName=my-tls-secret
```

---

## Azure Load Balancer backend pool

Analogous to AWS ALB `TargetGroupBinding` in **instance mode**.  
Requires the [Azure Load Balancer Controller](https://github.com/Azure/azure-load-balancer-controller) and its CRDs.

```bash
helm upgrade --install django-app ./django-app-chart \
  --set azureLoadBalancer.enabled=true \
  --set azureLoadBalancer.loadBalancerName=aks-prod-lb \
  --set azureLoadBalancer.resourceGroup=rg-aks-prod \
  --set azureLoadBalancer.backendPoolName=django-app-pool
```

---

## Values reference

See [`values.yaml`](./values.yaml) — every key is documented inline.

Key toggles:

| Key | Default | Effect |
|---|---|---|
| `apisix.enabled` | `false` | Create `ApisixRoute` (and `ApisixTls` if TLS enabled) |
| `apisix.tls.enabled` | `false` | Create `ApisixTls` resource |
| `azureLoadBalancer.enabled` | `false` | Create `AzureLoadBalancerBackendPool` |
| `autoscaling.enabled` | `false` | Create `HorizontalPodAutoscaler` |
| `serviceAccount.create` | `true` | Create a dedicated `ServiceAccount` |
