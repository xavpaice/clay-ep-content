---
title: Ingress and TLS
---

# Ingress and TLS

Clay.nz uses an Ingress resource with TLS for serving the shop over HTTPS.

## Enable Ingress

```yaml
ingress:
  enabled: true
  className: traefik    # change for non-Traefik controllers
  host: shop.example.com
  tls:
    mode: letsencrypt   # letsencrypt | selfsigned | custom
```

## TLS Modes

### Let's Encrypt (Recommended)

Automatically provisions and renews certificates via cert-manager and the ACME protocol.

```yaml
ingress:
  tls:
    mode: letsencrypt
    acme:
      email: admin@example.com
      production: true    # false = staging (for testing)
```

<Tip>
Start with `production: false` to test certificate issuance against the Let's Encrypt staging endpoint. Switch to `true` once verified.
</Tip>

### Self-Signed

Generates a self-signed CA and certificate. Useful for internal or development environments.

```yaml
ingress:
  tls:
    mode: selfsigned
```

Browsers will show a certificate warning. You can add the generated CA to your trust store to suppress it.

### Custom Certificate

Use a pre-existing TLS Secret in the cluster.

```yaml
ingress:
  tls:
    mode: custom
    secretName: my-tls-secret
```

The Secret must be of type `kubernetes.io/tls` and exist in the release namespace before installation.
