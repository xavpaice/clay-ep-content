---
title: Firing Logs
visible_when:
  entitlements:
    - enableFiringLogs
---

# Firing Logs

Firing Logs let sellers track kiln sessions, recording clay body, glaze notes, outcome, and temperature readings over time.

## Availability

This feature is controlled by your license. When enabled, sellers see a "Firings" section in their dashboard where they can create and manage firing log entries.

## Configuration

No additional configuration is needed. When your license includes the `enableFiringLogs` entitlement, the feature is automatically enabled at application startup.

The app checks the license field via the Replicated SDK on each restart. If the entitlement is removed, firing log routes become unavailable on the next pod restart.

## Usage

Once enabled, sellers can:

1. Navigate to **Dashboard > Firings**
2. Create a new firing log with title, clay body, glaze notes, and firing date
3. Record temperature readings with timestamps
4. View temperature charts for each firing session
5. Edit or delete their own firing logs
