# TP4 – GitOps, ArgoCD & Observability

## Repository layout
```
app/
  base/                 # Deployment + Service (common to every env)
  overlays/dev/         # namespace demo-dev, 2 replicas, image tag
  overlays/prod/        # namespace demo-prod, 3 replicas, image tag
argocd/                 # ArgoCD Application manifests (App-of-Apps)
monitoring/             # ServiceMonitors, PrometheusRules, Grafana dashboard
```

## Application
`quay.io/brancz/prometheus-example-app` is a small HTTP server that exposes
Prometheus metrics (`http_requests_total`, ...) on `:8080/metrics`.

## Technical choices
- **Kustomize base + overlays**: one source of truth, per-env differences
  (namespace, replicas, image tag) declared as small patches.
- **Hardened pods**: non-root, read-only root FS, all capabilities dropped,
  seccomp RuntimeDefault, resource requests/limits, liveness/readiness probes.

Render locally: `kubectl kustomize app/overlays/dev`
