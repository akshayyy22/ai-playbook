# Code Review

Review changes for bugs, security issues, and quality problems. Report findings; don't fix unless asked.

## Gather the diff

Get the full set of changed lines (e.g. `git diff <base>...HEAD`, or the PR diff). If truncated, read each changed file fully. List every changed file before reviewing.

## Attack surface

For each changed file, note: user inputs (params, headers, body), database queries, auth/authorization checks, session/state ops, external calls, crypto.

## Security checklist (every item, every file)

- **Injection**: SQL, command, template, header.
- **XSS**: outputs escaped in templates/markup.
- **Authentication**: checks on all protected operations.
- **Authorization/IDOR**: access control per resource, not just auth.
- **CSRF**: state-changing operations protected.
- **Race conditions**: TOCTOU in read-then-write paths.
- **Session**: fixation, expiration, secure flags.
- **Cryptography**: secure random, sound algorithms, no secrets in logs.
- **Information disclosure**: error messages, logs, timing.
- **DoS**: unbounded operations, missing rate limits.
- **Business logic**: edge cases, state-machine violations, numeric overflow.

## Verify before reporting

- Is it already handled elsewhere in the change?
- Is there a test covering it?
- Read surrounding context to confirm it's real.

## Output

Prioritize: security > bugs > quality. Skip style/formatting. For each issue:

- **File:Line** — brief description
- **Severity**: Critical/High/Medium/Low
- **Problem** — what's wrong
- **Evidence** — why it's real (not already fixed, no existing test)
- **Fix** — concrete suggestion

If nothing significant, say so. Don't invent issues.
