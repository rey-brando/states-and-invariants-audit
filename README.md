# The States & Invariants Audit

**A free, read-only pre-launch audit for AI-built apps.** Run it with Claude Code (or any coding agent) before real users touch your app.

Your app works on the happy path. Your users won't stay on it. This audit finds where your app gets stuck, the rules it relies on that nothing enforces, and the bugs that only show up when something goes wrong.

## What it checks

| Part | What it finds |
| --- | --- |
| **A. State machines** | Every flow that moves through stages (jobs, payments, credits, trials, uploads). It draws each one and finds stuck stages, missing failure paths, races, and money out of step with what the user got. |
| **B. Unenforced invariants** | Every rule your app assumes but nothing forces, like "a refund happens once" or "credits never go below zero". For each: how it's enforced today, how it breaks, and the smallest fix. |
| **C. Eight hunts** | Partial success, retries that act twice, facts that drift apart, events out of order, one rule applied four ways, screens that say something untrue, silent errors, and whether each bad state is really reachable. |
| **D. Launch readiness** | An 11-point checklist: privacy, your user data map, keys out of the browser, no `.env` in the repo, safe logs, no oversharing APIs, auth on every route, SQL injection and XSS, security headers, rate limits on paid APIs, locked database rules. |

## How to run it

1. Open your app's project in Claude Code.
2. Copy `PROMPT.md` and `CHECKLIST.md` into an `audit/` folder in your project.
3. Pick your strongest model and a high effort setting. This is a long job.
4. Type: `Read audit/PROMPT.md and follow it.`
5. It maps your app first and stops, so you can approve the rest. Then it writes its findings into `audit/`.

## Safe by design

- **Read-only.** It doesn't change your code, touch your live database, call paid APIs or deploy anything.
- **Every finding is challenged.** A second agent tries to disprove each one before it's reported.
- **Anything that needs live data** is listed with a read-only query for you to run, never claimed as fact.
- **You decide the fixes.** It hands you a ranked list grouped into batches, so you can fix everything in one go.

## What you get back

- A diagram of every state machine in your app.
- A plain-words top 10, then every finding ranked by severity.
- The 11 launch checks marked Pass, Fail or Needs a live check.
- Fixes grouped into batches, and the read-only queries in one place.

## Credits

- [awesome-prompts](https://github.com/Drix10/awesome-prompts) by Drix10 (MIT): its state-machine and "unicorn hunt" prompts inspired Parts A to C.
- [GoShipIt](https://github.com/Capta1nRaj/goshipit) by Capta1nRaj (Apache-2.0): a broader pre-launch audit skill, recommended for a deeper automated pass.

Everything in this repo is written from scratch and released under the MIT license.

## Made by Rey Brando

Performance marketer who builds AI products without writing code. More free workflows on X: [@ReyBrando1](https://x.com/ReyBrando1).
