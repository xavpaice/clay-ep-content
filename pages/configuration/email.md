---
title: Email (SMTP)
---

# Email Configuration

Clay.nz sends order notification emails to sellers when customers place orders. Configure SMTP to enable this.

## SMTP Settings

```yaml
config:
  SMTP_HOST: smtp.example.com
  SMTP_PORT: "587"
  SMTP_FROM: orders@clay.nz
  ORDER_EMAIL: admin@clay.nz    # fallback recipient for order notifications

secrets:
  SMTP_USER: ""    # set via --set at install time
  SMTP_PASS: ""    # set via --set at install time
```

<Warning title="Security">
Pass SMTP credentials at install time using `--set`, not in a values file.
</Warning>

## Example

```bash
helm upgrade clay oci://registry.replicated.com/{{ app.slug }}/{{ channel.slug }}/clay \
  --namespace clay \
  --set config.SMTP_HOST=smtp.sendgrid.net \
  --set config.SMTP_PORT=587 \
  --set config.SMTP_FROM=orders@clay.nz \
  --set secrets.SMTP_USER=apikey \
  --set secrets.SMTP_PASS=<your-api-key>
```

## Without SMTP

If SMTP is not configured, the application runs normally but order notification emails are not sent. Preflight checks will warn about SMTP connectivity if `SMTP_HOST` is set but the server is unreachable.
