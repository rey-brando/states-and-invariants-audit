# States & Invariants Audit: the prompt

Give this file to your coding agent (Claude Code, Codex, Cursor or similar) inside your app's project and say: "Read audit/PROMPT.md and follow it."

---

You are auditing this codebase before real users arrive. Answer one question: **where can this app get stuck, contradict itself, charge wrongly or leak data?**

You find problems and write them down. You do **not** fix them.

## Ground rules

1. **Read-only.** Do not change application code, run migrations, write to any live database, call paid APIs, send emails or messages, or deploy anything. You may read files, search the code, and run commands that only read (and the project's own local tests, if they touch nothing live).
2. **Evidence or it didn't happen.** Every finding names the files and lines involved and the exact steps that make it break.
3. **Live data is not your call.** If a finding depends on what's in the live database, put it under "Needs a live check" with the read-only query that would confirm it. Never mark it confirmed.
4. **Save as you go.** Write your findings to the `audit/` folder after every phase. If you run in a cloud session that disappears when it ends, commit and push them after every phase.
5. **Plain words in the summaries.** Assume the owner is not a developer. Define any technical term the first time you use it.

## Phase 0: map the app, then stop

1. List every flow that moves through stages. Typical ones: sign-up and sign-in, payments, refunds, credits or usage limits, free trials, background jobs, uploads and file processing, emails and notifications, admin actions, scheduled jobs and clean-up tasks, webhooks from outside services (payments, auth, storage), and feature switches.
2. Group them into 5 to 10 lanes (for example: money, background jobs, uploads, accounts, notifications, admin, scheduled jobs, launch readiness). Write them to `audit/lanes.md`.
3. **Budget gate. Stop here.** Report the lanes, how many agents you plan to run, and which lanes look biggest. Wait for the owner's go before Phase 1. This audit is long and uses a lot of usage.

## Phase 1: one agent per lane (in parallel if your tool can)

Each lane runs Parts A, B and C on its own flows.

### Part A: state machines

A state machine is any flow with stages, like a job that goes queued, working, done or failed, or a charge that goes held, kept or refunded. In AI-built apps these stages usually live in flags and status columns spread across files, so nobody ever drew them.

For every flow with stages:

- List every stage and every move between stages: what triggers it and which code does it.
- Draw it as a Mermaid `stateDiagram-v2`.
- For each stage, write what the user sees, what money or credits are held, and what moves it on if nothing happens (a timer, a sweeper, or nothing at all).

Then find:

- **Stuck stages:** no way out, or the only way out depends on something that can fail silently.
- **Impossible mixes:** values that can be true together but shouldn't be ("paid" and "refunded", "done" with no file).
- **Missing failure moves:** a step that can fail with no stage to fail into.
- **Races:** two events that can arrive at once and both act.
- **Money out of step with the stage:** charged but not delivered, delivered but not charged, refunded twice.
- **Screens that don't know a stage:** a stage the interface can't display, so the user sees a spinner forever.
- **Rules copied twice:** the same move written in two places that can drift apart.

### Part B: unenforced invariants

An invariant is a rule that must always be true. "A finished job always has its file." "A refund happens at most once." "Credits never go below zero." "One free trial per person."

For every rule the app relies on:

- **Where it's relied on** (files and lines).
- **How it's enforced today:** by the database (a constraint or unique index), by one server code path, only by the screen, or nowhere.
- **How it breaks,** step by step.
- **The smallest fix,** database first and additive only (a constraint, a unique index, a check), plus a test that proves the fix holds.
- **A read-only query** that counts rows already breaking it, for the owner to run.

### Part C: eight hunts

1. **Partial success:** money moves but the work doesn't, or the work happens but the money doesn't.
2. **Retries that act twice:** webhooks, queues, double clicks, timeouts that retry.
3. **Two copies of one fact drifting apart:** the same status stored in two places, a cache that goes stale.
4. **Order and timing:** events that arrive out of order (payment webhooks are the classic case).
5. **One rule, applied differently** by the screen, the server, background workers and the admin page.
6. **Screens that say something untrue:** "done" when it isn't, a price that isn't the one charged.
7. **Errors that disappear silently:** caught and ignored, logged nowhere, never retried.
8. **Reachability:** for each bad state, can a real user actually get there? List "allowed but unreachable" separately.

## Phase 2: try to disprove every finding

Give each finding to a fresh agent whose only job is to prove it wrong: find the guard, constraint or code path that already prevents it. Keep only the findings that survive. Mark each survivor **Confirmed** or **Needs a live check**.

## Phase 3: launch readiness

Run every check in `audit/CHECKLIST.md`. Mark each one **Pass**, **Fail** or **Needs a live check**, with the evidence.

## What to hand back

- `audit/state-machines.md`: Part A, one section per flow, each with its diagram.
- `audit/invariants.md`: a plain-words top 10 first, then the invariants table, then Parts B and C ranked by severity, then "allowed but unreachable" and "needs a live check".
- `audit/launch-readiness.md`: every checklist item with its evidence, plus the data map from check 2.
- `audit/summary.md`: one ranked list with no duplicates, every read-only query in one place, the fixes grouped into batches that don't touch the same files twice, and open questions for the owner in plain words.

**Severity:** Critical (money or user data at risk), High (users stuck or misled), Medium, Low.

Do not start fixing. The owner reads the summary first and decides what gets fixed, in one batch.
