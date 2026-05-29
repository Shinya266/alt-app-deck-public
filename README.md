# Alt App Deck

Human-Gated AI Operations Platform

AI can propose. Only humans can release.

Alt App Deck is a control plane for AI-generated application changes.  
AI agents can submit DevPlans, but execution requires explicit human approval.

The system provides:

- AI-generated DevPlan intake
- Diff preview before execution
- Human approval gate
- Safe apply pipeline
- Backup before change
- Build / restart / health check flow
- Execution history
- Rollback and rollback-run verification
- Audit logging

This public repository is a sanitized portfolio version.  
It does not include production tokens, private data, logs, backups, or live infrastructure configuration.

---

# Alt App Deck

Alt App Deck is a standalone human-approved app operations deck.

It receives DevPlans or fileOps-like instructions from a user or an upstream AI Agent, previews the diff, waits for explicit human approval, then safely applies changes to registered apps.

Definition:

Human-approved deterministic deploys for AI-generated changes.

AI proposes.
Human approves.
AppDeck applies, backs up, builds, restarts, checks health, records history, and can roll back.

What it is not:

Alt App Deck is not Altora Core.

It does not contain private reasoning, memory, persona, Twin, Observer, Fugyaa, Board state, or private Core state.

It is a standalone external operations layer.

Flow:

USER or AI Agent
-> DevPlan / fileOps
-> Alt AppDeck pending queue
-> Diff preview
-> Human approve
-> Apply
-> Backup
-> Build
-> Restart
-> Health check
-> History
-> Rollback / Rollback Run

Core boundary:

Alt App Deck is not Altora Core. It does not contain private reasoning, memory, persona, Twin, Observer, Fugyaa, or Board state. It is a standalone human-gated app operations deck. The user provides intent. Alt App Deck previews and applies app changes only after explicit human approval.

Ports:

Backend API:
- Internal only: 127.0.0.1:4300

UI + Proxy:
- Public: example.com:4310
- Public UI proxies /api/* to internal backend.

Public firewall should not expose 4300.

Security model:

- Human-gated by default
- Token required for mutating routes
- Agent token can only create pending DevPlans
- Admin token required for approve/apply/rollback/build/restart
- No arbitrary shell from DevPlan
- Registered app commands only
- allowedPaths required
- denyPaths enforced
- .env and secret files blocked
- Symlink traversal blocked
- File extension allowlist
- File size limit
- Delete requires confirmDelete:true
- Backup before apply
- History after apply
- Rollback available
- Rollback Run performs restore, build, restart, and health check

Main APIs:

GET /api/health

GET /api/apps
GET /api/apps/:appId/status
POST /api/apps/:appId/build
POST /api/apps/:appId/restart

POST /api/devplans
GET /api/devplans
GET /api/devplans/:id
GET /api/devplans/:id/diff
POST /api/devplans/:id/approve
POST /api/devplans/:id/decline
POST /api/devplans/:id/apply

POST /api/agent/devplans

GET /api/history
GET /api/history/:id
POST /api/history/:id/rollback
POST /api/history/:id/rollback-run

GET /api/logs/audit

Agent Intake:

Upstream AI agents should only submit DevPlans.

They must not approve, apply, rollback, build, or restart.

Example:

ALT_APP_DECK_AGENT_TOKEN=$(cat /path/to/appdeck-data/agent-token.txt) appdeck-agent-submit ./devplan.json

DevPlan example:

{
  "title": "Update message",
  "targetAppId": "appdeck-demo",
  "summary": "Change one safe visible line.",
  "fileOps": [
    {
      "type": "update",
      "path": "/root/appdeck-demo/src/message.txt",
      "content": "Hello from AppDeck.\n",
      "reason": "Demo update."
    }
  ],
  "commands": {
    "build": true,
    "restart": true,
    "healthCheck": true
  }
}

Current implemented phases:

Phase 1: Sandbox MVP complete.
Phase 2: Real App Onboarding Demo complete.
Phase 3: Token Guard complete.
Phase 4: AI Agent / CLI Adapter complete.
Phase 5-1: UI Admin Token support complete.
Phase 5-2: rollback-run API complete.
Phase 5-3: Safety Hardening complete.
Phase 5-4: 4300 public API closed and UI proxy enabled.
Phase 5-5: UI rollback-run complete.
Phase 5-6: UI / Audit / Product Demo Polish in progress.

Product line:

AI-generated changes should not go straight to production.

Alt App Deck gives them a human-approved execution path.

More changes per token.
More control per deploy.

