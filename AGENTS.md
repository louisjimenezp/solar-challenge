# Planet Guidelines

## Governance Delegation (Required)

**This layer (planets/solar-challenge/AGENTS.md):**
- **Authority:** Domain-specific governance for Solar technical challenge productization
- **Delegates to:** `../../AGENTS.md` (root) for global orchestration

## Scope
- Domain: Solar technical challenge productization.
- In scope: challenge strategy, scope definition, implementation planning, deliverables (demo/repo/README), and decision logs specific to this challenge.
- Out of scope: unrelated personal career tasks, Uhorizon client operations, or framework-level governance changes in `core/` unless explicitly requested.

## Governance
- Required checks: every proposed change must map to at least one challenge evaluation criterion (problem-solving, full-stack fundamentals, AI usage, code quality, decision clarity, UX/product thinking).
- Security/data rules: do not expose secrets, API keys, or private user data in demos, repo history, or documentation.
- Operational limits: keep scope within a 6-8 hour build budget unless user explicitly extends scope.

## Input Contract (Sun -> Planet)
- Objective:
- Constraints:
- Context:

## Output Contract (Planet -> Sun)
- Status:
- Deliverables:
- Risks:
- Next steps:

## Planet Sync Rule (Optional)

If `agents/`, `commands/`, or `skills/` are created in this planet:

**Sync command:**
```bash
bash ../../core/scripts/sync-clients.sh
```

**What it does:**
- Syncs planet resources to AI clients
- Auto-applies `solar-challenge:<name>` prefix on conflicts (npm-style)

See `../../AGENTS.md` for full protocol details.
