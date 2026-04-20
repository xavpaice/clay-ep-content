---
title: FAQ
---

# Frequently Asked Questions

## Installation

<Accordion title="What Kubernetes versions are supported?">

Kubernetes 1.27 or later. We recommend 1.29+ for the best experience. Run preflight checks before installing to verify compatibility.

</Accordion>

<Accordion title="Can I use an existing PostgreSQL database?">

Yes. Set `postgres.managed=false` and provide your DSN via `--set postgres.external.dsn=<your-dsn>`. See [Database Configuration](../configuration/database) for details.

</Accordion>

<Accordion title="Which ingress controllers are supported?">

Traefik is the default and tested configuration. Other controllers work by setting `ingress.className` to your controller's class name.

</Accordion>

## Operations

<Accordion title="How do I check for updates?">

The admin panel shows a banner when updates are available. See [Upgrading](../operations/upgrading) for the upgrade process.

</Accordion>

<Accordion title="How do I collect diagnostic information?">

Generate a [support bundle](../operations/support-bundles) and upload it through the Enterprise Portal or send it to our support team.

</Accordion>

{{#if entitlements.enableFiringLogs}}
## Features

<Accordion title="How do firing logs work?">

Firing Logs let sellers track kiln sessions with temperature readings. See [Firing Logs](../configuration/firing-logs) for details.

</Accordion>
{{/if}}

## Troubleshooting

<Accordion title="My pods are stuck in ImagePullBackOff">

This usually means the Replicated SDK hasn't created the image pull secret yet, or your license has expired. See [Troubleshooting](../operations/troubleshooting#images-not-pulling) for details.

</Accordion>

<Accordion title="The application can't connect to the database">

Check that the CNPG PostgreSQL cluster is healthy with `kubectl get clusters.postgresql.cnpg.io -n clay`. See [Troubleshooting](../operations/troubleshooting#application-not-starting) for more.

</Accordion>
