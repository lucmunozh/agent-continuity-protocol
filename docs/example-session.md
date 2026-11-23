# A session, end to end

A worked example of the loop, so the protocol is concrete rather than aspirational.
The project is fictional; the shape is taken from real use.

---

## Opening

**Me:**

> Read `AGENTS.md` and `CURRENT_STATE.md`. Confirm where we left off before making any changes.

**Agent:**

> Read both. Current position:
>
> We are diagnosing why the integration gateway's acknowledgement is being rejected by the contract validator. Task 42 established that the message *is* being delivered and processed — that is verified, with a log pointer. The open question is why the validator still reports a failure.
>
> `CURRENT_STATE.md` lists one constraint: the QA environment is read-only this week.
>
> Under "do not repeat": re-sending the message has already been ruled out as a diagnostic — it was tried in Task 38 and produced a duplicate.
>
> Next step per the file: compare the declared response schema against the actual response.
>
> This is Task 43. I have made no changes. Confirm and I will start.

Thirty seconds, and the agent is oriented *and* has stated its premises where they can be checked.

Note what the "do not repeat" section just prevented. Without it, "let me re-send it and watch the logs" is exactly what a helpful agent proposes next.

---

## Working

**Me:**

> Confirmed. Go.

**Agent:**

> **Verified**
>
> - Declared contract says `200`, no response body — `contracts/gateway.yaml:88`
> - Actual response is `201` with a body — `outputs/run-20261002/gateway.txt:613`, 16:44:52
> - The message was processed successfully — order history response, `outputs/order-101/history.xml`
>
> **Inferred**
>
> - The validator failure is a contract mismatch, not a delivery failure. Confirmed if updating the declared response to `201` + schema makes the error disappear.
>
> **Changed**
>
> - Nothing. `contracts/gateway.yaml` is in the shared environment; per `AGENTS.md` I am not modifying it without an explicit request.
>
> Want me to draft the change for review?

The last two lines are the protocol working. The agent found the fix, did not apply it, and said why.

---

## Closing

**Me:**

> Yes, draft it. Then update `CURRENT_STATE.md` and append to `CONTINUITY.md`.

**Agent writes to `CONTINUITY.md`:**

```markdown
## Task 43 — Locate the source of the acknowledgement rejection

**Date:** 2026-10-02 17:20 · **Environment:** dev

### Objective
Determine why the contract validator rejects an acknowledgement that is in fact processed.

### Verified
- Contract declares 200, no body — contracts/gateway.yaml:88
- Gateway returns 201 with body — outputs/run-20261002/gateway.txt:613, 16:44:52
- Message processed successfully — outputs/order-101/history.xml

### Inferred
- Contract mismatch, not a delivery failure — would be confirmed by updating
  the declared response to 201 + schema and re-running the validator.

### Decisions
- Fix the contract rather than the gateway: the gateway's 201 is correct
  REST semantics for a created resource.

### Changed
- Nothing applied. Draft patch at drafts/gateway-201.yaml, pending review.

### Still open
- Apply the patch and re-run the validator.
- Check whether other consumers assume a 200 here.
```

**And rewrites `CURRENT_STATE.md`:**

```markdown
**Last updated:** 2026-10-02 17:20 · **Next task number:** 44

## Where we are
Root cause of the acknowledgement rejection is identified: contract mismatch.
Patch drafted, not applied.

## Last verified fact
Gateway returns 201 with body where the contract declares 200 with none —
outputs/run-20261002/gateway.txt:613.

## Immediate next step
Review and apply drafts/gateway-201.yaml, then re-run the validator.

## Open questions
- Do other consumers depend on the declared 200?

## Active constraints
- QA is read-only this week.

## Do not repeat
- Re-sending the message as a diagnostic — Task 38, produced a duplicate.
- Restarting the listener — Task 40, no effect, costs 15 minutes.
```

---

## Why this is worth the two minutes

Tomorrow's session starts from a position, not from a reconstruction.

Three weeks from now, when someone asks *"why is that contract declaring 201?"*, the answer is a decision with its reasoning attached, not an archaeological dig.

And the "do not repeat" list keeps growing, which means the agent's known-bad space grows with the project instead of resetting every morning.
