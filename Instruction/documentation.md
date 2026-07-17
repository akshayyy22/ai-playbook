# Documentation

Templates and rules for READMEs, API docs, comments, and handoffs. Keep docs scannable, example-first, and up to date. Outdated docs mislead.

## README

Order: title + one-liner → quick start (<5 min) → features → configuration → docs links → license.

## Code comments

- Comment the why, not the what.
- Comment: business logic, complex algorithms, non-obvious behavior, API contracts.
- Don't comment: obvious or self-explanatory code, every line.

## API documentation

Per endpoint: method + path, purpose, auth, request (params/body with types and constraints), success response, all error codes with example bodies.

- Mark fields required vs optional.
- Use realistic example data, not "foo"/"bar".
- Document every error code and its meaning.
- Keep examples working and in sync with code.

## API handoff

When handing an API to frontend, write a doc so integration needs no back-and-forth. For simple CRUD, endpoint plus example request/response JSON is enough. Otherwise include:

- Business context: what/who/why.
- Endpoints: method, path, auth, request/response JSON, error shapes, edge cases.
- Data models/DTOs: types, nullability, enums.
- Enums and constants with display labels.
- Validation rules the frontend should mirror.
- Integration notes: recommended flow, optimistic-UI safety, caching, real-time.
- Test scenarios: happy path, validation error, not found, permission denied.

## ADR

For notable decisions: Status (Accepted/Deprecated/Superseded), Context (why), Decision (what), Consequences (trade-offs).
