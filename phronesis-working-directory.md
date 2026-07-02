# The `.phronesis/` working directory

A per-task assurance trail. The five skills in this repo already instruct an agent to
collect decision context, call the action boundary, record the Decision Receipt, route
human review, and follow up on outcomes. This convention gives those artifacts one
durable, predictable home — so a person (or another agent) can audit the task afterward
without reconstructing it from chat history.

## The convention

When a skill fires for a consequential action, create a task directory:

```
.phronesis/<task-slug>/
```

and maintain these files through the task:

| File | What it holds | When |
|---|---|---|
| `decision_requirement.md` | what is being decided, for whom (principal + mandate), and how consequential it is (`decision_materiality`) | at task start |
| `evidence_notes.md` | the evidence collected and its maturity; the control-path snapshot (see `phronesis-mcp-control-path`) | while collecting context |
| `action_boundary_request.json` | the exact request body sent to the action-boundary call | at the call |
| `decision_receipt.json` | the returned Decision Receipt, verbatim | on response |
| `review_log.md` | who reviewed and what they decided, when the boundary routes to a human (`review` / `require_human` / `stage_only`) | before proceeding |
| `outcome_followup.md` | what actually happened, once the world resolves — the input to outcome scoring | after execution |

Not every task produces every file — a `block` outcome has no `review_log.md` or
`outcome_followup.md`; a task with no control-path plane has thinner `evidence_notes.md`.
Write what the task actually produced.

## Honest scope

This is an **agent-side convention**: the calling agent writes these files in its own
working directory. Phronesis does not read, require, or verify them — the Decision
Receipt remains verifiable against Phronesis regardless of whether the trail is kept.
The trail's value is local auditability: the receipt proves the decision; the directory
shows the work.

_Phronesis — the decision-assurance layer. Operated by Sustainable Finance Partners, LLC.
Not investment, legal, or other professional advice; no ratings or guarantees.
Support: support@phronesisintel.com_
