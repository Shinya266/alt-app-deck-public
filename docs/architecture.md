# Architecture

Alt App Deck is a human-gated control plane for AI-generated application changes.

Flow:

AI Agent / User
→ DevPlan
→ Pending Queue
→ Diff Preview
→ Human Approval
→ Safe Apply
→ Backup
→ Build
→ Restart
→ Health Check
→ History
→ Rollback Ready

The core principle is simple:

AI can propose changes.
Only humans can approve release.
