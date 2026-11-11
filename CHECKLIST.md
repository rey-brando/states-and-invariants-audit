# Launch readiness checklist (11 checks)

Part D of the States & Invariants Audit. Mark each check **Pass**, **Fail** or **Needs a live check**, and write down the evidence (files, settings, or the query to run).

| # | Check | What "Pass" looks like |
| --- | --- | --- |
| 1 | **Privacy policy is live and true** | It's published, linked from the app, and matches what the app actually collects and who it shares it with. |
| 2 | **A map of your user data** | Every kind of user data listed: where it's stored, which outside services receive it, and when it's deleted. |
| 3 | **API keys stay out of the browser** | No secret key appears in frontend code or the built files the browser downloads. |
| 4 | **No `.env` file in the repo** | Not in the code, not in the build output, not anywhere in the git history. |
| 5 | **No secrets in logs** | Logs never print keys, tokens, passwords or personal data. |
| 6 | **No response returns more than it should** | No endpoint sends other users' data or internal fields the screen doesn't need. |
| 7 | **Sign-in and ownership checked on every server function** | Every server route checks who's asking and that they own what they're touching. |
| 8 | **SQL injection and cross-site scripting** | All database queries use parameters; user text is never inserted into pages as raw HTML. |
| 9 | **Security headers** | Content-Security-Policy, framing protection (X-Frame-Options or frame-ancestors), Permissions-Policy and HSTS are set. |
| 10 | **Rate limits on everything that costs money** | Every endpoint that calls a paid API, sends an email or SMS, or uses credits has a limit per user and per IP. |
| 11 | **Database rules are locked** | Row-level security (or your database's equivalent) is on, and no table is readable or writable by the public unless it's meant to be. |

**Going deeper:** for a broader automated pass (secrets, code quality, security, tests, build, reliability, billing and legal), see [GoShipIt](https://github.com/Capta1nRaj/goshipit). For attackers and trust boundaries, plan a dedicated security review before you launch.
