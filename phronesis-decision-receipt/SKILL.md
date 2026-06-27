---
name: phronesis-decision-receipt
description: >-
  Use to obtain, record, and verify a Phronesis Decision Receipt — the durable,
  auditable record of a consequential decision (the action boundary, the basis it
  rested on, and a reference for later outcome scoring). Call after a high-stakes
  action is bounded (see phronesis-action-boundary), or when you need to prove a past
  decision was defensible. A Decision Receipt records the decision basis; it is not a
  guarantee of outcome.
---

# phronesis-decision-receipt

## Purpose
A **Decision Receipt** is Phronesis's durable, verifiable record of a decision —
**Proof of Decision**. It captures *what* was decided, the *action boundary* returned,
and the *basis* it rested on (evidence maturity, mandate / principal, control-path,
decision materiality), with an integrity reference so the decision can be shown later.
It is **not** a guarantee, rating, or advice — it records the basis of the decision,
not a promise about the result.

> **Availability:** the Decision Receipt is returned by action-boundary scoring
> (`phronesis_score_action_boundary`), which is **forthcoming — not yet callable**. The
> live, callable `phronesis-hermes` tools today are `health_check` and `bench_query`
> (public, read-only). This skill describes the contract so you are ready when it ships.

## When to use
- After any consequential action bounded via `phronesis-action-boundary` — capture and
  store the receipt.
- When you need to **prove a past decision was defensible** (audit, reconciliation, dispute).
- When closing the loop: associating a resolved outcome with the original decision.

## What a receipt contains
- `decision_receipt` — the ID
- the action + the `action_boundary` returned (approve / review / block / …)
- `evidence_maturity` + a summary of mandate / principal / control-path / materiality
- timestamp + a methodology / version reference (immutable)
- `outcome` — added later, when the world resolves (honest-empty until then)

## How to get + verify
- The receipt is returned alongside the action boundary — **forthcoming** (the
  action-boundary call is not yet live; MCP server `phronesis-hermes`, principal-scoped;
  free diligence-tier JWT via `/api/v1/signup`).
- Verify integrity via its reference / hash (Proof of Decision). Public benchmark surface
  available now for orientation: `GET https://api.phronesisintel.com/.well-known/bench.json`.

## Bind it to the execution
- Write the `decision_receipt` ID to your execution / audit log — **one receipt per
  consequential action**.
- Keep it: it is the artifact that proves the decision later.

## Anti-patterns (do NOT)
- Do **not** execute a high-risk action without capturing its receipt.
- Do **not** treat a receipt as a guarantee of outcome — it records the decision basis,
  not a promise.
- Do **not** alter, fabricate, or discard a receipt.
- Do **not** present a receipt as advice, a rating, or a guarantee.

## Outcome scoring (closing the loop)
When the action's outcome resolves, it is scored against the receipt — this is how
calibration accrues over time. Until an outcome resolves, calibration is **honest-empty**
(reported as insufficient sample, never manufactured).

_Phronesis — the decision-assurance layer. Operated by Sustainable Finance Partners, LLC.
Not investment, legal, or other professional advice; no ratings or guarantees._
