---
name: tiered-memory-entry
description: Lightweight tiered memory starter for AI agents. Hot/warm/cold tiers, access-frequency promotion, one canonical marker, instant-alert hooks. Install with one command. Full governance spec sold separately as the $49 bundle.
version: 1.0.0
price: 12-19 USD
channel: SkillHQ, Agensi, Claude Code marketplace
---

# Tiered Memory Entry SKU

## What this is
A starter memory layer for AI agents and automation runtimes. Three tiers — hot, warm, cold — with access-frequency-based promotion between them. One canonical marker string for integrity verification. Instant-alert hooks that fire on failure instead of waiting for a scheduled check.

## Why it exists
Most agent memory is flat text that silently drifts, gets overwritten, or dies without warning. This starter gives you the minimum viable governance: tiered storage, a promotion rule, a marker you can search for, and an alert hook. No multi-surface redundancy, no provenance chains, no cross-instance consensus — those live in the full $49 bundle.

## Install
```
cp SKILL.md ~/.claude/skills/tiered-memory-entry/SKILL.md
```
Or drop it into your agent's skill directory. One file. No dependencies.

## How it works

### Three tiers
- **Hot** — actively used facts, loaded every session.
- **Warm** — recently accessed, promoted to hot on next use.
- **Cold** — dormant, archived but preserved. Resurfaces on mention.

### Promotion rule
Any fact accessed in a session moves up one tier. Hot facts that go untouched for 30 days move down. Simple, deterministic, no LLM calls required for the move itself.

### Canonical marker
Every memory file begins with:
```
TIERED-MEMORY-ENTRY-ACTIVE
```
Search your filesystem or repo for that string to verify the layer is installed and intact. If it's missing, the layer is gone.

### Instant-alert hook
On any write failure, sync error, or missing marker, emit one line:
```
ALERT: tiered-memory-entry [reason]
```
Pipe that to your notification surface — email, Slack, app push. No waiting for a nightly check.

## What you get
- SKILL.md (this file) — the complete spec
- No runtime code required; works as instructions for any agent that reads skill files
- Optional: pair with the Dead Man's Switch ($29) for heartbeat-based liveness on the memory layer itself

## Upgrade path
The full Tiered Memory System v3.2 ($49 bundle) adds: four-surface redundancy with SHA checksums, versioned self-healing bootstrap, provenance chains per fact, contradiction surfacing, cross-instance consensus, resurrection logging with load-bearing classification, anomaly gating, bi-temporal timestamps, and a daily integrity sweep with auto-restore. This entry SKU is the on-ramp.

## Pricing
$12-19 one-time. Free micro-version (marker + one-tier stub) available as a teaser to seed ratings.

## Support
Docs and issues in the boq-income-pipeline repo. Updates pushed quarterly.
