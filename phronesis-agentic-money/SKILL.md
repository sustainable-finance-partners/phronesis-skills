---
name: phronesis-agentic-money
description: >-
  Use BEFORE an agent moves money — a payment, refund, transfer, subscription
  change, x402 micropayment, or on-chain send. Calls Phronesis to check the action
  is authorized and defensible (valid mandate, known counterparty, amount within
  materiality, payment context bound) and returns an action boundary
  (approve / review / block / limit / require-human) plus a Decision Receipt.
  Phronesis never moves the money — the calling agent or a payment rail does.
---

# phronesis-agentic-money

## Purpose
Before an agent moves money, call Phronesis to assess whether the action is
**authorized and defensible** — and get a Decision Receipt. Phronesis **consumes**
payment, mandate, and identity context (AP2 / Verifiable Intent / Stripe / Coinbase /
x402 / wallet); it **does not move money, hold funds, or guarantee outcomes** — the
calling agent or the payment rail executes the transfer.

> A signed mandate or payment token proves the agent is **authorized** to pay. It does
> **not** prove the payment is **prudent**. Phronesis assesses defensibility on top —
> *can-pay is not should-pay.*

## When to use
Before any money movement: payments · refunds · transfers · subscription changes ·
x402 micropayments · on-chain sends.

## Required context (collect before calling)
- `principal_context` + `mandate_scope` — who authorized it, and for what
- `payment_context` — amount · rail (card / ACH / x402 / on-chain) · counterparty · token/scope
- `counterparty_status` — known / rated / **unrated**
- `decision_materiality` — how consequential the amount is
- `evidence_context` — supporting evidence + maturity
- `control_path_context` — see `phronesis-mcp-control-path`

## Outputs — and how to act on each
- `approve` — proceed; record the Decision Receipt ID on the transfer.
- `review` — pause; route to the human owner before paying.
- `block` — do not pay; surface the reason.
- `limit_size` — pay only within the stated bound.
- `seek_evidence` — e.g. counterparty unrated → gather evidence, then re-call.
- `require_human` — amount/risk over threshold → human approval before paying.

## Anti-patterns (do NOT)
- Do **not** let the model move money directly — propose, get the boundary, then the
  caller or the payment rail executes the transfer.
- Do **not** trust client-supplied identity for authorization — verified context only.
- Do **not** treat a valid payment authorization (signed mandate / token) as approval
  to act — Phronesis assesses defensibility on top.
- Do **not** skip the boundary on "small" amounts without a materiality check.

## Bind the receipt
Every money action should carry its `decision_receipt` ID, written to your execution /
audit log (see `phronesis-decision-receipt`). Outcomes are scored over time;
calibration is honest-empty until they resolve.

## The pattern: propose → assure → review → execute → record
Propose the payment + context → call Phronesis → action boundary + Receipt → human
review if `review`/`require_human` → the agent or rail executes (**Phronesis never
does**) → record the Receipt ID.

_Phronesis — the decision-assurance layer. Operated by Sustainable Finance Partners, LLC.
Not investment, legal, or other professional advice; no ratings or guarantees.
Support: support@phronesisintel.com_
