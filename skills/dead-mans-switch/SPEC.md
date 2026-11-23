# Dead Man's Switch — Design Spec v1.0
## 2026-11-23

### Problem

Scheduled automations die silently. A Zapier flow, a Make scenario, a cron job, or a Grok automation that stops firing produces no error — just absence. The owner discovers it days or weeks later when the output it was supposed to produce is missing. By then the damage (missed deposits, missed listings, missed deadlines) is done.

### Solution

A heartbeat-based liveness layer. Every monitored automation emits a proof-of-life file on each successful run. A separate daily monitor checks for those proofs and fires an instant alert on any gap. The monitor's own liveness is proven by its own heartbeat, preventing cascade false alarms.

### Architecture

**Layer 1 — Heartbeat emitters.** Any automation (Grok Automations, Zapier, Make, cron, GitHub Action) adds one line to its success path: write `heartbeat-YYYY-MM-DD.md` to the shared Drive folder with component name, UTC timestamp, and one-line status. Cost: one file write per run. No SDK, no new service.

**Layer 2 — Daily monitor.** Grok automation `dead-mans-switch-daily-check`, 09:00 AEST daily. Checks: (a) heartbeat files for yesterday and today exist in Drive folder 1OMcGQQpHTHwgRm33F9eLJi_YxFvuBRSG; (b) this repo has a commit within 48 hours. On any miss: immediate app notification `DEAD MAN'S SWITCH: [component] silent since [timestamp].` On all-clear: silent PASS file `pass-YYYY-MM-DD.md`.

**Layer 3 — Alert protocol.** Instant, app-only, no batching. Repeated identical failures coalesce to one notification per hour (open question 1). Email-only or off is never used for failure-class alerts.

**Layer 4 — Self-monitoring.** The monitor writes its own heartbeat. If the monitor's heartbeat is stale beyond 48 hours, that is classified as monitor-dead (different from component-dead) and triggers a separate re-arm attempt before human escalation.

### Inherited from tiered-memory-system v3.2.1

- Instant Alert Protocol (eight failure classes, adapted to heartbeat semantics).
- Sweep-heartbeat separation (cascade-failure fix) — monitor-dead vs component-dead are different states.
- Under-ten-word dead-end statements (standing parameter 2).
- Brevity: one sentence per finding (standing parameter 4).
- Source ranking: live checks beat cached state (standing parameter 3).

### Novel to this project

1. **Universal emitter contract.** One-line heartbeat write works for any automation platform — Grok, Zapier, Make, cron, GitHub Actions — with zero platform-specific code. The monitor doesn't care what emitted it.
2. **Dual-surface proof.** Drive file + GitHub commit as independent liveness channels. A compromised or broken Drive doesn't blind the monitor because GitHub still proves the emitter ran.
3. **Coalesced alerting.** Open question 1: repeated identical failures should collapse to one notification per hour to avoid spam without losing the first-failure instant signal.

### Open Questions

1. Should repeated identical failures coalesce into one notification per hour?
2. Heartbeat staleness threshold: 48 hours fixed, or adaptive to each component's expected cadence?
3. Should the monitor itself be monitored by a second, independent automation on a different schedule?
4. Multi-user: one Drive folder per user, or shared folder with per-user subfolders?
5. Can heartbeat files be auto-pruned after 30 days without losing the audit trail?

### Monetization path

Sell as: spec + one-file emitter template + hosted monitor. Target: solopreneurs and small teams running 3+ scheduled automations who cannot afford silent failure. Pricing hypothesis: $9/month or $79 one-time for the spec pack.
