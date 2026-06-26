---
name: phronesis-action-boundary
description: >-
  Use BEFORE any high-stakes or consequential autonomous action — moving money,
  changing identity/access, altering security or infrastructure config, deploying
  to production, exporting data, accepting terms, selecting a vendor, or binding on
  a user's behalf. Calls Phronesis to get an auditable action boundary
  (approve / review / block / seek-evidence / limit / require-human) plus a Decision
  Receipt, before the action executes. Phronesis never executes the action.
---

# phronesis-action-boundary

## Purpose
Phronesis is a **decision-assurance layer**. Before an agent takes a consequential
action, call Phronesis with the action's context; it returns an **auditable action
boundary** plus a **Decision Receipt** — a durable record of the basis on which the
action was bounded. Phronesis **does not execute the action**, and is **not** advice,
a rating, or a guarantee — the calling agent or human retains the action.

> Identity and access systems tell you the agent **can** act. Phronesis tells you
> whether the action is **defensible enough to act** — and gives you the receipt to
> prove it later. **Valid access is not approval to act.**

## When to call this skill
Call before any **consequential** action, e.g.:
- moving money · payments · refunds · x402
- changing identity, credentials, or access scope
- altering security or infrastructure config (Zero Trust / firewall / IAM policy)
- production deploys · dependency changes · data exports
- accepting terms · selecting or committing to a vendor · binding on a user's behalf

Do **not** call for read-only / low-stakes actions (queries, drafts, retrieval).

## Required context (collect before calling — more complete → tighter boundary)
- `principal_context` — who the action is for (verified; never client-asserted)
- `mandate_scope` — what the principal authorized
- `action_type` + the proposed action
- `evidence_context` — supporting evidence and its maturity
- `control_path_context` — the identity / MCP / policy / credential path the action
  traveled (consumed from your IdP / MCP portal / gateway — e.g. Okta, Cloudflare Access)
- `payment_context` — when money moves
- `change_context` — for config/infra/code changes: diff, blast radius, owner, rollback plan
- `decision_materiality` — how consequential the action is

## How to call
- **MCP server:** `phronesis-hermes` (Streamable-HTTP, `https://api.phronesisintel.com/mcp`).
  The action-boundary tool is **principal-scoped** — get a free diligence-tier JWT at
  `/api/v1/signup` and pass `Authorization: Bearer <JWT>`.
- **Orientation, no signup (read-only, not the boundary call):** `bench_query` /
  `GET https://api.phronesisintel.com/.well-known/bench.json`.
- Send the collected context → receive `{ action_boundary, reason, evidence_maturity,
  decision_receipt, ... }`.

## Outputs — and how to act on each
- `approve` — proceed; record the Decision Receipt ID on the execution.
- `review` — pause; route to the human owner with the reason (and diff) before proceeding.
- `block` — do not proceed; surface the reason.
- `seek_evidence` — gather the named missing evidence, then re-call.
- `limit_size` — proceed only within the stated bound.
- `require_human` / `stage_only` — human approval (or stage-without-apply) before execution.

## Anti-patterns (do NOT)
- Do **not** let the model execute money movement, binding, or identity changes directly —
  propose, get the boundary, then the caller executes.
- Do **not** trust client-supplied identity for authorization — use verified
  principal / control-path context only.
- Do **not** treat valid MCP / tool access as action approval — *access is not assurance*.
- Do **not** apply a high-risk config/infra change without diff, owner, and rollback in
  `change_context`.
- Do **not** act before the boundary returns; do **not** fabricate evidence or maturity.

## Bind the receipt
Every executed consequential action should carry its `decision_receipt` ID, written to
your execution / audit log, so the decision is provable later. Outcomes are scored over
time; calibration is honest-empty until outcomes resolve.

## The pattern: propose → assure → review → execute → record
1. Propose the action and collect context.
2. Call Phronesis → action boundary + Decision Receipt.
3. If `review` / `require_human` → human approves (with diff / owner / rollback).
4. Execute — the agent or human acts; **Phronesis never does**.
5. Record the Receipt ID; outcome scoring closes the loop.

_Phronesis — the decision-assurance layer. Operated by Sustainable Finance Partners, LLC.
Not investment, legal, or other professional advice; no ratings or guarantees._
