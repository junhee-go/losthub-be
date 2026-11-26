---
name: javadoc-api-manual
description: Write, update, generate, and verify Korean Javadoc that serves as the LostHub backend assignment API manual.
---

# Javadoc API Manual

Use this skill when a public or protected Java API changes or when preparing the assignment API manual.

## Documentation Language

- Write all Javadoc descriptions in Korean.
- Keep Java identifiers, type names, method names, package names, annotations, and Javadoc tags in English.
- Write meaningful Korean descriptions for `@param`, `@return`, and `@throws`.
- Do not translate identifiers or invent Korean replacements for established Java terminology.
- Repository Markdown, including this skill file, remains English.

## Documentation Standard

- Document each public class, interface, enum, constructor, and method that belongs in the assignment API manual.
- Start with a concise purpose sentence and then explain non-obvious observable behavior or constraints.
- Describe authentication and authorization requirements, validation rules, state transitions, persistence ordering, nullability, and concurrency behavior when applicable.
- Use `@param`, `@return`, and `@throws` whenever applicable. Do not add empty tags or exceptions that cannot occur.
- Add `package-info.java` when a package overview materially improves the manual.
- Keep descriptions aligned with the implementation and current requirements.
- Do not use Javadoc as a replacement for an external HTTP or WebSocket contract.

## Verification

Run:

```powershell
.\gradlew.bat javadoc
```

Inspect `build/docs/javadoc/index.html` and representative class pages. Verify navigation, Korean text, signatures, parameter and return descriptions, warnings, and broken links.

Generated HTML under `build/docs/javadoc` is the source of truth for the assignment API manual.
