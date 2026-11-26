---
name: readme-maintainer
description: Keep the LostHub backend README aligned with implemented setup, verification, Railway deployment, features, and limitations.
---

# README Maintainer

Use this skill when repository setup, build commands, runtime configuration, deployment behavior, implemented features, or limitations change.

## Language

- Write the README in Korean so that setup, verification, deployment, and limitation information is clear to the team.
- Keep commands, environment-variable names, API paths, Java identifiers, and file paths in their original form.
- Do not add a repository-local external API manual unless the user explicitly requests it.

## Required Scope

Keep the README aligned with:

- Java 17 requirements.
- Gradle Wrapper commands.
- Local server startup once an entry point exists.
- Required environment variables once introduced.
- Local MySQL connection setup once implemented.
- Test, Javadoc, and Fat JAR commands.
- Railway Railpack build and start behavior.
- Railway `PORT`, `0.0.0.0`, Wait for CI, and health-check requirements.
- Implemented features and known limitations.

## Deployment Constraint

- Do not add Docker, Dockerfile, Docker Compose, container-registry, or Docker-based deployment instructions unless the user explicitly changes the architecture.
- Do not claim that Railway deployment works before a real server entry point and health endpoint exist.

## Accuracy

- Document implemented behavior, not planned behavior, as if it already works.
- Clearly label planned or unavailable functionality.
- Verify commands against the current Gradle configuration before claiming they work.
