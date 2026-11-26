---
name: losthub-be-maintainer
description: Implement or modify LostHub plain-Java backend production code while preserving its layered architecture, authentication, persistence, chat, image-storage, testing, and Railway constraints.
---

# LostHub Backend Maintainer

Use this skill for changes under `src/main`.

## Required Context

Before editing, read `AI.md`, neighboring production code, relevant tests, and the requirement provided for the task. Record dirty paths and preserve unrelated worktree changes.

## Hard Rules

- Use Java 17 and the Gradle Wrapper.
- Keep the server in plain Java. Do not introduce Spring or Spring Boot unless the user explicitly changes the architecture.
- Do not introduce Docker, Dockerfile, Docker Compose, or container-registry deployment.
- Do not add Java Swing client code to this backend repository.
- Do not create repository-local external API or design documents unless the user explicitly requests them.
- Read nearby code before adding abstractions or packages.
- Implement the smallest coherent change and avoid speculative managers, helpers, processors, or generic frameworks.
- Preserve unrelated user changes. Do not commit, push, open a pull request, or merge unless the user explicitly requests it.

## Architecture Rules

- Keep the HTTP dependency direction `server -> router -> controller -> service -> repository`.
- Keep the WebSocket dependency direction `server -> websocket handler -> service -> repository`.
- Use `Controller` for HTTP request-response entry points.
- Use `Handler` for WebSocket connection, message, close, and error events.
- Controllers and handlers may parse transport input, perform basic shape validation, call services, and map results to transport responses.
- Controllers and handlers must not execute SQL, call repositories directly, or own business decisions.
- Services own business validation, authentication, authorization, orchestration, and transaction boundaries.
- Repositories own parameterized SQL, JDBC resource handling, and row mapping. They must not own business-state decisions.
- External object-storage access belongs under `storage`; authenticated session state belongs under `session`.
- Keep shared error-to-response mapping centralized instead of duplicating it in controllers.

## Plain-Java Server Rules

- Keep method-and-path routing centralized.
- Keep JSON serialization and deserialization centralized.
- Keep dependency construction in one bootstrap/composition location.
- Read configuration and secrets from environment variables or explicit local configuration excluded from Git.
- Read Railway's port from `PORT` and bind the server to `0.0.0.0` when deployment is implemented.
- Ensure one failed request cannot terminate the server process.
- Add graceful shutdown for server resources, connection pools, and live WebSocket connections when those resources exist.

## Authentication And Session Rules

- Never store plaintext passwords. Use an established password-hashing algorithm through a dedicated abstraction.
- Normalize usernames consistently before uniqueness checks and lookup.
- Derive the authenticated user from the validated server session, never from a client-supplied user ID.
- Invalidate the server session on logout and reject missing, expired, or invalid sessions.
- Never log passwords, password hashes, session IDs, database credentials, bucket credentials, or other secrets.
- Keep authentication failure messages from revealing unnecessary account-existence details.

## Post Rules

- Require a non-blank item name.
- Only the author may update, complete, or delete a post.
- Allow only the irreversible `ACTIVE -> COMPLETED` status transition.
- Represent deletion separately with `deletedAt` and preserve related messages and image metadata.
- Reject updates to completed or deleted posts.
- Exclude completed and deleted posts from normal search and listing behavior as defined by the current requirements.
- Distinguish the loss/found event date from the post creation timestamp.
- Apply date filters to the event date, not the creation timestamp.

## Chat And Concurrency Rules

- Allow at most one chat room for each `(postId, inquirerId)` pair and enforce the invariant at the database level.
- Do not allow a post author to open an inquiry chat for the same post.
- Verify chat participation for room and message access.
- Reject new messages when the associated post is completed or deleted.
- Persist a message successfully before acknowledging or broadcasting it.
- Do not broadcast a message when persistence fails.
- Generate authoritative message IDs and timestamps on the server.
- Use thread-safe structures for live WebSocket connections and remove connections when they close.
- Avoid holding database transactions open while performing network delivery.
- Make retry and duplicate-message behavior explicit when it is implemented.

## Image Rules

- Allow no more than three images per post.
- Limit each image to 5 MB.
- Accept only the agreed JPG, JPEG, and PNG formats and validate content type in addition to the original filename.
- Generate object keys on the server.
- Keep object-storage credentials and internal keys out of client responses and logs.
- Compensate for partial failures between bucket upload and database metadata persistence.
- Clean up uploaded objects when the corresponding database operation fails.

## JDBC And Migration Rules

- Use `PreparedStatement`; never interpolate user input into SQL.
- Use try-with-resources for connections, statements, and result sets.
- Use transactions for multi-step writes and roll back on failure.
- Return connections to the pool on every path when a pool is introduced.
- Add schema changes as new versioned migrations. Do not edit a migration that has already been applied.
- Use explicit constraints and indexes for invariants and query patterns introduced by a feature.
- Never run automated tests against the production Railway database.

## Testing Rules

- Inspect existing tests before implementing a change.
- Identify success, validation, authorization, persistence-failure, and state-conflict paths relevant to the requirement.
- Add or update tests when current coverage does not verify the changed behavior.
- For bug fixes, add a regression test that fails without the fix when practical.
- Test observable public behavior instead of widening production visibility for private implementation details.
- Run the narrowest relevant test first, followed by the broader verification appropriate to the change.
- State why no new test was needed when existing tests already provide sufficient coverage.

## Javadoc And Contract Rules

- Add accurate Korean Javadoc to changed public and protected APIs that belong in the assignment manual.
- Keep Java identifiers and Javadoc tags in English.
- When an HTTP or WebSocket contract changes, report the exact contract change to the user. Do not create a local specification file.

## Change Workflow

1. Trace the affected request from transport entry point through service and persistence or infrastructure.
2. Identify success and failure paths.
3. Implement the smallest coherent change.
4. Add or update focused tests.
5. Update Korean Javadoc when required.
6. Run the narrowest relevant Gradle tasks.
7. Report changed paths, design decisions, test results, and API-contract impact for the verifier handoff required by `AI.md`.
