---
title: Requirements
---

# Installation Requirements

Ensure your environment meets these requirements before installing Clay.nz.

## Kubernetes Cluster

- Kubernetes 1.27 or later
- At least 2 CPU cores and 2Gi memory allocatable (4 CPU / 4Gi recommended for production)
- A default StorageClass with ReadWriteOnce support
- Ingress controller (Traefik recommended, others supported)

<Warning title="Unsupported Distributions">
Docker Desktop and MicroK8s are not supported. Use a production-grade distribution such as k3s, EKS, GKE, or AKS.
</Warning>

## Workstation Tools

- [Helm](https://helm.sh/docs/intro/install/) 3.x or later
- [kubectl](https://kubernetes.io/docs/tasks/tools/) configured with cluster access

## Network Requirements

The cluster must be able to reach:

- `proxy.clay.nz` -- container image proxy registry
- `registry.replicated.com` -- Helm chart registry and SDK images
- Your SMTP server (if order notification emails are required)

## Storage

Clay.nz uses persistent storage for:

- **PostgreSQL data** (managed by CloudNativePG) -- default 5Gi
- **Product image uploads** -- default 5Gi

Both can be configured via Helm values. If no default StorageClass is available, set `persistence.storageClass` and `postgres.cluster.storage.storageClass` explicitly.

## Preflight Checks

First, log in to the Replicated registry:

```bash
helm registry login registry.replicated.com --username {{ customer.email }} --password {{ license.id }}
```

Then run preflight checks to verify your environment. The required values must be set for the template to render correctly:

```bash
helm template clay oci://registry.replicated.com/{{ app.slug }}/{{ channel.slug }}/clay \
  --version {{ release.version }} \
  --set secrets.ADMIN_PASS=placeholder \
  --set secrets.SESSION_SECRET=placeholder-at-least-32-characters-long \
  | kubectl preflight -
```

<Tip>
The `secrets` values are required for the chart to template but are not used during preflight checks. Any non-empty value will work.
</Tip>

This validates Kubernetes version, cluster resources, storage, and distribution compatibility.

{{#if entitlements.isHelmInstallEnabled}}
If you have configured an external database, also set those values so the database connectivity check runs:

```bash
helm template clay oci://registry.replicated.com/{{ app.slug }}/{{ channel.slug }}/clay \
  --version {{ release.version }} \
  --set secrets.ADMIN_PASS=placeholder \
  --set secrets.SESSION_SECRET=placeholder-at-least-32-characters-long \
  --set postgres.managed=false \
  --set postgres.external.dsn=<your-dsn> \
  | kubectl preflight -
```
{{/if}}
