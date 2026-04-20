---
title: Upgrading
---

# Upgrading Clay.nz

<Warning title="Back Up Before Upgrading">
Always ensure your PostgreSQL data is backed up before upgrading. If using managed PostgreSQL, CloudNativePG handles continuous backup if configured.
</Warning>

## Check for Updates

The admin panel shows a banner when a new version is available on your channel. You can also check manually:

<HelmUpdateAssets />

## Upgrade Process

```bash
helm upgrade clay oci://registry.replicated.com/{{ app.slug }}/{{ channel.slug }}/clay \
  --namespace clay \
  --version <new-version>
```

Helm will perform a rolling update of the application. The process:

1. New application pods start with the updated image
2. Database migrations run automatically on startup
3. Old pods are terminated after new pods pass readiness checks
4. CloudNativePG handles PostgreSQL updates separately (rolling restart if the image changed)

## Rollback

If something goes wrong:

```bash
helm rollback clay -n clay
```

This restores the previous Helm release. Note that database migrations are not automatically rolled back -- contact support if you need to revert a migration.
