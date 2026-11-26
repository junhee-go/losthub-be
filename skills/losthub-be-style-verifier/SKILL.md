---
name: losthub-be-style-verifier
description: Independently review LostHub backend production-code diffs for architecture, security, persistence, concurrency, tests, Korean Javadoc, and requirement consistency.
---

# LostHub Backend Style Verifier

Review production-code changes independently. Do not edit files, apply patches, run formatters, or run Gradle. The implementing agent owns every fix.

## Required Handoff

Require the following before issuing a verdict:

- Original requirement.
- Pre-existing dirty paths.
- Implementer-owned `src/main` file list.
- Exact diff for those files.
- Design decisions.
- Test and verification commands with results.
- External API-contract impact.

Review only owned changes. Do not require cleanup of unrelated or pre-existing work.

## Verification Procedure

1. Read `AI.md`, `$losthub-be-maintainer`, the changed files, relevant tests, and the closest comparable production code.
2. Run only read-only static checks such as `git diff --check` when available.
3. Do not run Gradle, tests, the server, database code, or network integrations. Treat the implementer's command results as supplied evidence.
4. Complete every applicable checklist category.
5. Report findings only. Do not modify files.

## Checklist

Report `PASS`, `FAIL`, or `N/A` with concise evidence for each category:

1. **Scope and safety**: the change satisfies the requirement, stays focused, and preserves unrelated work.
2. **Architecture**: HTTP follows `server -> router -> controller -> service -> repository`; WebSocket handlers delegate to services; no direct controller/handler repository access exists.
3. **Plain-Java server**: routing, JSON mapping, dependency construction, configuration, error mapping, lifecycle, and thread ownership remain explicit and consistent.
4. **Authentication and authorization**: session validation, user derivation, author checks, participant checks, password handling, and secret handling are correct.
5. **Post invariants**: required fields, author-only changes, irreversible completion, soft deletion, and search visibility rules are preserved.
6. **Chat and concurrency**: room uniqueness, participant checks, read-only behavior, persist-before-broadcast ordering, transaction scope, and thread-safe connection management are correct.
7. **Image storage**: count, size, type, server-generated keys, credential safety, and partial-failure cleanup are correct.
8. **JDBC and migrations**: parameterized SQL, resource handling, transactions, rollback, constraints, indexes, and migration immutability are correct.
9. **Tests and evidence**: success and applicable failure paths are covered proportionally; tests assert observable behavior; recorded checks support the claim.
10. **Javadoc**: changed public and protected APIs have accurate Korean Javadoc with appropriate tags.
11. **Documentation language**: `README.md` may be Korean; other changed repository Markdown is English; Korean Javadoc keeps Java identifiers and tags in English.
12. **API contract impact**: external contract changes are reported without creating repository-local API documents.
13. **Deployment**: no Docker configuration is introduced; Railway assumptions remain compatible with Java 17, Railpack, `PORT`, `0.0.0.0`, and the future health endpoint.

Treat direct SQL in a controller or handler, direct controller/handler repository access, plaintext password storage, missing authorization, interpolated SQL, message broadcast before persistence, unsafe shared connection state, or material design contradiction as blocking.

## Finding Format

Report every finding as:

`[severity] file:line — violated rule or evidence — narrowest corrective action — automatic-refactor: yes|no`

Use `blocking` only for a clear rule violation with a safe corrective structure. Use `risk` when evidence or the correct design decision is incomplete.

Finish with one verdict:

- `PASS`: no blocking findings.
- `REFACTOR REQUIRED`: blocking findings have a clear narrow correction.
- `RISK REMAINS`: no safe automatic correction is available.
