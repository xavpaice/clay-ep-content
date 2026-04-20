---
title: Support Bundles
---

# Support Bundles

Support bundles collect diagnostic information from your Clay.nz deployment for troubleshooting.

## What's Collected

The bundle includes:
- Application, PostgreSQL, CNPG operator, cert-manager, and SDK logs
- Pod status, events, and resource usage
- Health and readiness check results
- Storage class and PVC information
- Service and endpoint details

Sensitive values (secrets, passwords) are not included.

## Generate a Bundle

<HelmBundles />

Or generate manually using the support-bundle kubectl plugin:

```bash
# Install the plugin (one-time)
curl -L https://github.com/replicatedhq/troubleshoot/releases/latest/download/support-bundle_linux_amd64.tar.gz | \
  tar xz -C /usr/local/bin support-bundle

# Collect the bundle
kubectl support-bundle -n clay secret/clay-support-bundle
```

This produces a `.tar.gz` file you can upload to the support portal or send to our team.

## Analyzers

The bundle automatically checks for common issues:

| Check | What it detects |
|-------|----------------|
| App readiness | Application not responding or database unreachable |
| Deployment status | Pods down for clay, CNPG operator, or cert-manager |
| Job status | Webhook wait jobs that failed during install/upgrade |
| Database errors | Connection failures or migration errors in app logs |
| Storage class | Missing default StorageClass |
| Node readiness | Nodes in NotReady state |

## Upload a Bundle

If you already have a bundle file, upload it through the Enterprise Portal for our team to review.

<UploadBundles />
