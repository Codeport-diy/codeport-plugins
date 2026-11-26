---
name: security-before-push
description: Check the project with Codeport for leaked secrets (API keys, passwords, private keys) in files and git history, sensitive files missing from .gitignore, and dependencies with known vulnerabilities. Works without a Codeport account. Use before a commit, push, pull request, deploy or sharing the code, and whenever the user asks whether their code is safe.
---

# Security check before you push

## On this machine (no account needed)

1. Call `security_check` for the workspace folder. It runs locally; only package names and versions go to
   OSV.dev. By default it reads the current files and the last 100 commits. Use more commits when the user
   asks for a deeper look, or 0 for the current files only.
2. If it finds nothing, say so in one line and carry on with the commit or push.
3. If it finds something, list the findings by severity, most severe first: what it is, the file and line (or
   commit), and the fix. Then offer to apply the fixes you can do here:
   - **A secret in the code**: move it to an environment variable or the project's secret store, add the file
     that holds it to .gitignore, and tell the user to **revoke and rotate the key** with its provider. Removing
     it from the code does not help once it was committed or pushed: it stays in git history.
   - **A secret only in history**: it is still leaked. The fix is to rotate it. Rewriting history is optional
     and only the user decides it.
   - **A sensitive file not ignored** (.env, key files): add it to .gitignore, and remove it from the index with
     the user's agreement.
   - **A vulnerable dependency**: upgrade to the fixed version the report names, then run the project's tests.
4. When the user is about to push a commit that contains a critical finding, say so clearly before the push and
   let them decide.

## In Codeport's cloud

For a repository connected to Codeport, `check_repository_security` with `repo` shows the latest cloud scan:
secrets in code and the whole history, dependencies, risky code patterns and infrastructure files. Use
`rescan` only when the user asks for a fresh scan. Open critical findings block deploys.

The logic review by Codeport AI (missed permission checks and similar) is off until the user turns it on. Turn
it on with `set_ai_review` only after the user explicitly agrees, and tell them first that routes, auth and
policy files are sent to Codeport AI with secrets removed.

## Rules

- Never print, copy or reconstruct a secret value. Refer to a secret only by the mask the report gives (prefix
  and length), its type and its place.
- Do not open .env or key files to "check" a finding.
- Severity comes from the report; do not downgrade a finding because it looks like a test value unless the
  report says it is in a test.
