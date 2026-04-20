---
title: Troubleshooting
---

# Troubleshooting

Common issues and how to resolve them.

## Application Not Starting

**Symptom:** The `clay` pod is in CrashLoopBackOff or not ready.

**Check logs:**
```bash
kubectl logs -n clay deploy/clay
```

**Common causes:**

- **Database unreachable:** Look for `DB ping failed` or `connection refused` in the logs. Verify the PostgreSQL cluster is healthy:
  ```bash
  kubectl get clusters.postgresql.cnpg.io -n clay
  ```
- **Migration failure:** Look for `Failed to run migrations` in the logs. This usually means the database schema is in an unexpected state. Contact support with a [support bundle](support-bundles).
- **License validation failed:** The app validates its license on startup. If the Replicated SDK is not ready, it retries for up to 5 minutes. Check SDK pod status:
  ```bash
  kubectl get pods -n clay -l app.kubernetes.io/name=clay-sdk
  ```

## Images Not Pulling

**Symptom:** Pods stuck in `ImagePullBackOff`.

**Check the event:**
```bash
kubectl describe pod -n clay <pod-name> | grep -A5 "Events"
```

**Common causes:**

- **Expired license:** The `enterprise-pull-secret` is managed by the Replicated SDK and tied to your license. If your license has expired, image pulls will fail with `401 Unauthorized`.
- **SDK not running:** The SDK creates the pull secret. If the SDK pod is not running, other pods cannot pull images. Check SDK status first.
- **External registry not configured:** If you see `404 Not Found` from the proxy, the upstream registry may not be configured in the Vendor Portal.

## TLS Certificate Issues

**Symptom:** Browser shows certificate errors or cert-manager pods are not ready.

**Check cert-manager:**
```bash
kubectl get pods -n clay -l app.kubernetes.io/name=cert-manager
kubectl get clusterissuers
kubectl get certificates -n clay
```

**Common causes:**
- **ACME rate limits:** Let's Encrypt has rate limits. Use `production: false` for testing.
- **DNS not resolving:** The ingress host must resolve to your cluster's external IP for HTTP-01 challenges.

## PostgreSQL Issues

**Symptom:** Database errors in application logs.

**Check CNPG cluster status:**
```bash
kubectl get clusters.postgresql.cnpg.io -n clay -o yaml
kubectl logs -n clay -l cnpg.io/cluster=clay-postgres
```

**Common causes:**
- **Storage full:** Check PVC usage with `kubectl exec -n clay clay-postgres-1 -- df -h /var/lib/postgresql/data`
- **Operator not running:** Check `kubectl get pods -n clay -l app.kubernetes.io/name=cloudnative-pg`
