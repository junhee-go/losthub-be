# LostHub Backend AI Development Guide

## Project Baseline

- LostHub is a client-server application for campus lost-and-found posts and real-time chat.
- This repository contains only the backend server. Do not add Java Swing client code.
- Use Java 17, the Gradle Wrapper, Groovy Gradle DSL, JUnit 5, and Javadoc.
- Implement the server in plain Java. Do not introduce Spring or Spring Boot unless the user explicitly changes the architecture.
- Use HTTP or HTTPS for request-response features and WebSocket for real-time chat.
- Use MySQL for persistent data and Railway Bucket for post images when those integrations are implemented.
- Deploy the server to Railway with Railpack. Do not add Docker, Dockerfile, Docker Compose, or container-registry workflows unless the user explicitly changes the deployment architecture.
- Do not create or maintain repository-local external API specification files unless the user explicitly requests them.

## Repository Skills

- `$losthub-be-maintainer`: implement or modify LostHub backend production code.
- `$losthub-be-style-verifier`: perform the required read-only review of production-code changes.
- `$javadoc-api-manual`: write and verify Korean Javadoc used as the assignment API manual.
- `$readme-maintainer`: keep repository setup, verification, and deployment information accurate.
- `$git-commit-korean`: prepare small Korean commits when the user explicitly requests a commit.

Read the matching `skills/<skill-name>/SKILL.md` before doing work covered by a skill. Use the smallest set of skills that covers the request.

## Architecture Boundaries

Use these packages as the default structure:

- `server`: server bootstrap and lifecycle.
- `router`: HTTP method and path routing.
- `controller`: HTTP request entry points and response mapping.
- `websocket`: WebSocket lifecycle and message entry points.
- `service`: business rules, authorization, orchestration, and transaction boundaries.
- `repository`: JDBC persistence and database mapping.
- `domain`: entities, value objects, and domain enums.
- `dto`: transport request and response values.
- `session`: authenticated server-session management.
- `storage`: external object-storage access.
- `config`: environment and infrastructure configuration.
- `exception`: application exceptions and error mapping.

The normal dependency directions are:

`HTTP server -> Router -> Controller -> Service -> Repository -> MySQL`

`WebSocket server -> ChatWebSocketHandler -> ChatService -> Repository`

Controllers and handlers must not execute SQL or call repositories directly. Repositories must not own authentication, authorization, or business-state decisions.

Use `Controller` for HTTP request-response entry points. Use `Handler` for WebSocket connection, message, close, and error events. Both must delegate business behavior to services.

## Mandatory Production-Code Workflow

Apply this workflow whenever a task changes files under `src/main`. Do not apply the full verifier workflow to documentation-only, test-only, or build-only changes.

1. Record pre-existing dirty paths and preserve unrelated user changes.
2. Read `$losthub-be-maintainer`, nearby production code, relevant tests, and the requirement provided for the task.
3. Identify the required success and failure paths.
4. Implement the smallest coherent change.
5. Add or update tests when existing tests do not adequately verify the changed behavior.
6. Run the narrowest relevant tests first.
7. Update Korean Javadoc for changed public or protected APIs.
8. Identify and report external API-contract changes without creating local API documents.
9. Prepare a handoff containing the requirement, owned production files, exact diff, design decisions, and commands/results.
10. Use a fresh read-only verifier with `$losthub-be-style-verifier`.
11. Apply only the minimum refactoring required by blocking findings, rerun relevant checks, and use a fresh verifier again.
12. Stop after two correction cycles and report any remaining risk.

Work is complete only when relevant checks pass and the final verifier reports no unresolved blocking findings.

## Documentation Language

- Write repository Markdown files in English, except `README.md`, which may be written in Korean.
- The English rule applies to `AGENTS.md`, `AI.md`, repository-local `SKILL.md` files, pull request templates, and other `*.md` files.
- Use Korean for `README.md` when it makes setup, verification, and deployment instructions clearer for the team.
- Keep commands, Java identifiers, file paths, environment-variable names, API paths, JSON fields, and code examples in their original form.
- Write Javadoc descriptions for the assignment API manual in Korean while keeping Java identifiers and Javadoc tags in English.

## Documentation Gates

- When a public or protected Java API changes, use `$javadoc-api-manual` and regenerate Javadoc.
- When an external request or response contract changes, report the changed contract to the user. Do not create a local API specification.
- When setup, verification, environment variables, Railway behavior, or limitations change, use `$readme-maintainer`.

## Verification Commands

Use the Gradle Wrapper from the repository root:

```powershell
.\gradlew.bat compileJava
.\gradlew.bat test
.\gradlew.bat javadoc
.\gradlew.bat fatJar
.\gradlew.bat verifyLostHubBe
```

On Linux and Railway, use `./gradlew` instead of `.\gradlew.bat`.

Prefer the narrowest relevant task while iterating. Run `verifyLostHubBe` before a milestone handoff or release claim.

## CI And Deployment

- GitHub Actions is the CI system and must run on pull requests and pushes to `main`.
- CI must run `verifyLostHubBe` with JDK 17.
- Railway is the deployment platform and must use Railpack, not a repository-managed Dockerfile.
- Configure Railway with `RAILPACK_JDK_VERSION=17`, Wait for CI, the final Fat JAR start command, and a `/health` health check once the server bootstrap exists.
- Do not claim Railway deployment readiness before the application has a real server entry point and health endpoint.
- Never use the production Railway database in automated tests.

## Completion Criteria

- The implementation matches the requirement and current LostHub design.
- Layer boundaries, authorization rules, persistence rules, and concurrency constraints are preserved.
- Relevant automated checks pass and manual checks are reported honestly.
- Required Korean Javadoc is accurate and generated successfully.
- External API-contract changes are reported without creating repository-local API documentation.
- No unrelated user changes are overwritten, committed, pushed, or merged.
