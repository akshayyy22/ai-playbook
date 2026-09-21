---
name: engineering-principles
description: Akshay's engineering principles for designing, implementing, reviewing, and refactoring software. Use when making architectural decisions, designing APIs or data models, writing production code, reviewing PRs, or evaluating tradeoffs.
---

# Engineering Principles

These are default engineering principles, not absolute laws. Apply judgment based on the actual requirements and constraints.

## 1. Simplicity First

Prefer the simplest solution that correctly solves the current problem.

Follow:

> Make it work → Make it simple → Make it fast/general when needed.

Avoid:

* Premature optimization
* Premature abstraction
* Premature generalization
* Unnecessary dependencies
* Unnecessary configuration
* Speculative features

Before adding complexity, ask:

> Can this be deleted?
> Can this be simpler?

---

## 2. YAGNI

Do not build functionality for hypothetical future requirements.

Build for known requirements.

Create extension points only when there is a concrete reason to expect future change.

---

## 3. Separation of Concerns

Keep different responsibilities separate.

Examples:

* Controllers → HTTP concerns
* Services → business logic
* Repositories → persistence
* DTOs → input/output contracts
* Infrastructure → external systems

Avoid classes or modules that become "do everything" components.

---

## 4. Composition Over Inheritance

Prefer composition when it provides better flexibility and simpler dependencies.

Avoid deep inheritance hierarchies.

---

## 5. SOLID With Judgment

Apply SOLID when it improves maintainability.

Do not introduce abstractions merely to satisfy a principle.

Particularly:

* One clear responsibility per module/class
* Depend on abstractions when useful
* Keep interfaces focused
* Preserve behavioral contracts

---

## 6. Consistency

Prefer existing project conventions over introducing new ones.

Maintain consistency in:

* Naming
* APIs
* Error handling
* Validation
* Pagination
* Project structure
* Database conventions

When an established industry convention exists, prefer it over inventing a custom convention.

---

# API Design

## Resource Identification

Use path parameters to identify resources:

```http
GET /users/123
GET /users/123/orders
```

## Query Parameters

Use query parameters for:

* Filtering
* Sorting
* Searching
* Pagination
* Optional response expansion

```http
GET /users?status=active
GET /users?sort=-created_at
GET /users?limit=20&cursor=abc
```

## Request Body

Use the body for substantial create/update data.

## Headers

Use headers for protocol metadata such as authentication and request metadata.

## HTTP Methods

Respect HTTP semantics:

```text
GET     → Read
POST    → Create/action
PUT     → Replace
PATCH   → Partial update
DELETE  → Delete
```

Do not hide state-changing operations inside GET requests.

---

# API Contracts

Treat public APIs as long-lived contracts.

Prioritize:

* Clear naming
* Consistent response structures
* Predictable errors
* Input validation
* Backward compatibility
* Explicit versioning where necessary

For TypeScript/NestJS, prefer typed DTOs over `any`.

```typescript
createUser(@Body() dto: CreateUserDto)
```

rather than:

```typescript
createUser(@Body() body: any)
```

Validate external input at the boundary.

---

# Data Modeling

Treat database schemas as long-lived contracts.

Before designing a schema, consider:

* Query patterns
* Row counts
* Write rates
* Indexes
* Relationships
* Data ownership
* Multi-tenancy
* Security
* Schema evolution

## Normalize by Default

Prefer normalized schemas to avoid unnecessary duplication and multiple sources of truth.

Denormalize deliberately when there is a demonstrated performance or query requirement.

Document the tradeoff.

## Schema Evolution

Prefer backward-compatible migrations.

For important production changes:

```text
Add
 ↓
Deploy compatible code
 ↓
Migrate/backfill
 ↓
Switch usage
 ↓
Deprecate
 ↓
Remove
```

Do not casually rename or drop production columns.

## Multi-Tenancy

For tenant-owned data:

* Scope records by `tenant_id` / `org_id`
* Enforce tenant isolation
* Design indexes around tenant-scoped queries
* Consider database-level enforcement such as RLS where appropriate

## Data Types

Use appropriate types.

Examples:

```text
Money       → NUMERIC / DECIMAL
Timestamps  → TIMESTAMPTZ
IDs         → UUID where appropriate
```

Never use floating-point numbers for monetary values.

---

# Scalability

Do not optimize for imaginary scale.

Do consider obvious growth constraints.

Ask:

* How many rows are expected?
* What are the hottest queries?
* What are the highest write rates?
* Which queries need indexes?
* Where could hotspots occur?
* What happens at 10x current scale?

Prefer measurable requirements over speculative infrastructure.

---

# Consistency

Separate core state from derived state when appropriate.

Core business data should prioritize correctness.

Derived data may use eventual consistency when it provides meaningful performance or scalability benefits.

Examples of potentially asynchronous work:

* Search indexing
* Notifications
* Webhooks
* Analytics
* Derived counters

Do not introduce asynchronous systems without a concrete reason.

---

# Documentation

> Code explains WHAT. Documentation explains WHY.

For significant technical decisions document:

```text
Problem
Decision
Alternatives
Trade-offs
Failure modes
Assumptions
When to reconsider
```

Use design documents for:

* Ambiguous changes
* Major architectural changes
* New services
* Cross-system changes
* Significant migrations
* Difficult-to-reverse decisions

Keep documentation concise and maintainable.

---

# Comments

Comments should explain WHY, not repeat WHAT the code does.

Bad:

```typescript
// Increment counter
counter++;
```

Good:

```typescript
// Cap retries to prevent a thundering-herd effect
// during downstream outages.
```

Use comments for:

* Non-obvious decisions
* Business rules
* Constraints
* Workarounds
* Historical context

---

# 1-Way vs 2-Way Doors

Classify important decisions.

### 1-Way Door

Difficult or expensive to reverse.

Examples:

* Public API contracts
* Database schema
* Source-of-truth decisions
* Persistent data ownership
* Major architecture

→ Research, compare alternatives, document, and review carefully.

### 2-Way Door

Easy to reverse.

Examples:

* Internal implementation details
* Stateless code
* Refactorable internals

→ Move quickly and avoid unnecessary planning.

---

# AI-Assisted Engineering

AI recommendations are proposals, not conclusions.

For ambiguous decisions:

1. Identify assumptions.
2. Challenge those assumptions.
3. Compare alternatives.
4. Check the existing codebase.
5. Research established conventions.
6. Consider reversibility.
7. Review important decisions with another source when appropriate.

Never choose an approach simply because an AI describes it as "best".

The engineer owns the decision.

---

# Debugging

Debug systematically.

```text
Reproduce
   ↓
Locate the failing boundary
   ↓
Inspect logs / metrics
   ↓
Trace data flow
   ↓
Form hypothesis
   ↓
Verify hypothesis
   ↓
Fix root cause
   ↓
Add regression test
```

Do not randomly modify code until the symptom disappears.

---

# Code Review

Review in this order:

## Correctness

* Does it satisfy the requirement?
* Are edge cases handled?
* Are async operations awaited?
* Are errors handled?
* Can race conditions occur?

## API

* Correct HTTP method?
* Correct path/query/body?
* Proper DTO validation?
* Consistent errors?

## Architecture

* Clear responsibilities?
* Appropriate abstraction?
* Unnecessary complexity?
* Duplicated business logic?

## Database

* Correct relationships?
* Appropriate indexes?
* Safe migrations?
* N+1 queries?
* Transaction requirements?

## Security

* Authentication?
* Authorization?
* Input validation?
* Sensitive data exposure?
* Injection risks?
* Tenant isolation?

## Reliability

* What happens when dependencies fail?
* Are retries safe?
* Is the operation idempotent where necessary?
* Are timeouts defined?
* Is the failure observable?

## Maintainability

* Is the code easy to understand?
* Are names meaningful?
* Is there unnecessary abstraction?
* Can future changes be made easily?

---

# Pull Requests

Prefer small, focused PRs.

A PR should:

* Solve one logical problem
* Minimize unrelated changes
* Be easy to review
* Include appropriate tests
* Explain important design decisions

Avoid mixing unrelated refactors, formatting changes, and feature work.

---

# Core Mental Model

For every significant engineering decision, ask:

```text
What problem are we solving?
        ↓
What is the simplest solution?
        ↓
What assumptions are we making?
        ↓
What trade-offs are we accepting?
        ↓
What can fail?
        ↓
How will we observe it?
        ↓
How will we change it later?
```

The goal is not perfect code.

The goal is:

> Simple, correct, maintainable, observable, and evolvable software.
