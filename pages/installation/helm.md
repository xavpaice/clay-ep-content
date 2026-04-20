---
title: Helm Installation
---

# Helm Installation

Install Clay.nz on your Kubernetes cluster using Helm.

## Prerequisites

- Review the [Requirements](requirements) page
- Run [preflight checks](requirements#preflight-checks) to validate your environment

## Configuration

<KubernetesDistribution />
<NetworkAvailability installType="helm" />
<RegistryAccess />
<VersionSelector installType="helm" />

## Install

<HelmInstallAssets />

<InstanceName />

### Required Values

At minimum, you must set these values:

| Value | Description |
|-------|-------------|
| `secrets.ADMIN_PASS` | Admin panel password |
| `secrets.SESSION_SECRET` | Session encryption key (min 32 chars) |
| `ingress.host` | Your domain name |
| `ingress.tls.mode` | TLS mode: `letsencrypt`, `selfsigned`, or `custom` |

### Example

```bash
helm install clay oci://registry.replicated.com/{{ app.slug }}/{{ channel.slug }}/clay \
  --namespace clay --create-namespace \
  --set secrets.ADMIN_PASS=<your-admin-password> \
  --set secrets.SESSION_SECRET=<random-32-char-string> \
  --set ingress.enabled=true \
  --set ingress.host=shop.example.com \
  --set ingress.tls.mode=letsencrypt \
  --set ingress.tls.acme.email=admin@example.com
```

## Post-Install Verification

After installation, verify all components are running:

```bash
# Check all pods are ready
kubectl get pods -n clay

# Verify the app is responding
kubectl exec -n clay deploy/clay -- wget -qO- http://localhost:8080/healthz
```

You should see pods for:
- `clay` -- the application
- `clay-postgres-1` -- PostgreSQL database (managed by CloudNativePG)
- `clay-cloudnative-pg` -- CloudNativePG operator
- `clay-cert-manager` -- cert-manager (if TLS is enabled)
- `clay-sdk` -- Replicated SDK

## Next Steps

- Configure [Ingress and TLS](../configuration/ingress) for your domain
- Set up [Email](../configuration/email) for order notifications
- Access the admin panel at `https://<your-domain>/admin`
