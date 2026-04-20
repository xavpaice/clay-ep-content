---
title: Database Configuration
---

# Database Configuration

Clay.nz uses PostgreSQL for all application data. You can use the bundled managed database or bring your own.

## Managed PostgreSQL (Default)

By default, Clay.nz deploys a PostgreSQL instance managed by the CloudNativePG operator. No additional configuration is needed.

```yaml
postgres:
  managed: true
  cluster:
    instances: 1
    storage:
      size: 5Gi
      storageClass: ""  # uses default StorageClass
```

### Production Recommendations

For production deployments, consider increasing storage and using a dedicated StorageClass:

```yaml
postgres:
  cluster:
    instances: 2      # enable HA with a standby replica
    storage:
      size: 20Gi
      storageClass: fast-ssd
```

## External PostgreSQL (BYO)

To use an existing PostgreSQL database, disable managed mode and provide a DSN:

```yaml
postgres:
  managed: false
  external:
    dsn: ""  # set via --set at install time, not in a values file
```

<Warning title="Security">
Never embed database credentials in a values file. Pass the DSN at install time using `--set postgres.external.dsn=<your-dsn>` or reference a Kubernetes Secret.
</Warning>

When using external PostgreSQL:
- The CloudNativePG operator subchart can be disabled (`cloudnative-pg.enabled: false`)
- The database must be accessible from within the cluster
- Preflight checks will validate connectivity before installation
