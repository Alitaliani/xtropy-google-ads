# Google Ads Plugin Status

Show the current configuration status, tier, and connection health.

## What to Do

Call `check_setup_status` and present a clear diagnostic report.

## Format

Present results as:

```
Google Ads Plugin Status
========================

Tier: {tier} — {tier_name}
  Tier 1 (Query):         {check/x} Google Ads API connected
  Tier 2 (Reports):       {check/x} Report generation available
  Tier 3 (Full Pipeline): {check/x} Drive + email notifications

Connections:
  Google Ads API:  {status} ({account_count} accounts)
  Gmail API:       {status}
  Google Drive:    {status} (rclone: {available/not found})

Loaded Modules:
  reports:       {loaded/not loaded}
  landing_pages: {loaded/not loaded}
  analytics:     {loaded/not loaded}
  serp:          {loaded/not loaded}

Configuration:
  .env file:       {exists/missing}
  accounts.json:   {exists/missing} ({count} accounts)
  Credentials:     {configured/missing}
  Developer token: {configured/missing}
```

If the tier is 0 (unconfigured), suggest:
> "Run /google-ads:setup to configure the plugin."

If the tier is below 3, show what's needed to reach the next tier.
