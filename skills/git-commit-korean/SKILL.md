---
name: git-commit-korean
description: Inspect LostHub backend history and diffs, then draft or create small Korean commits when the user explicitly requests a commit.
---

# Korean Git Commit

Use this skill only when the user asks to draft a commit message or create a commit.

## Rules

- Inspect the current branch, recent history, dirty paths, staged diff, and unstaged diff before proposing a commit.
- Preserve unrelated user changes and never stage them without explicit scope.
- Split unrelated changes into small logical commits when practical.
- Use a short type prefix and a concise Korean subject.
- Do not commit, push, rewrite history, open a pull request, or merge unless the user explicitly requests that action.
- Never include secrets, generated build output, IDE-local files, or production credentials.

## Suggested Types

- `feat`: new behavior.
- `fix`: bug fix.
- `refactor`: behavior-preserving restructuring.
- `test`: test-only change.
- `docs`: documentation or Javadoc change.
- `chore`: build, CI, harness, or repository maintenance.

Use the form `<type>: <concise Korean subject>` without mixing unrelated changes into one commit.
