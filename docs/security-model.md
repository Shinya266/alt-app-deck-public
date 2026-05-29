# Security Model

Alt App Deck is designed to prevent autonomous AI changes from reaching production without human review.

Main controls:

- Human approval required before execution
- DevPlan-based change intake
- Diff preview before apply
- Backup before modification
- Build / restart / health check after apply
- Rollback and rollback-run support
- Audit history
- Registered app scope
- Allowed paths / denied paths
- Secret files excluded from public repository

This public repository is a sanitized portfolio version.
It does not include production tokens, logs, backups, runtime data, or private infrastructure configuration.
