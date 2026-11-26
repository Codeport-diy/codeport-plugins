---
name: deploy-with-codeport
description: Deploy an app with Codeport to the organization's own Google Cloud or AWS account, check where it runs and its latest deploys, and roll back. Admins only. Use when the user asks to deploy, ship, publish or release the app, asks what is live or why a deploy failed or was blocked, or asks to roll back.
---

# Deploy with Codeport

Codeport deploys a repository connected to it (GitHub), to a cloud account the organization connected in the
Codeport web app. Only admins can deploy and roll back.

## Before the deploy

1. Call `list_deployments` with `repo` (and `app`, the folder or service, when the repository has several apps).
   It shows where the app deploys, what is live and the latest deploys.
2. Call `check_repository_security` with `repo`. A deploy takes the commit the latest finished scan read, so
   tell the user which commit will go live. If they pushed since, the new commit goes after its scan finishes.
3. If the repository is not connected, use `connect_github` first. If no cloud is connected, tell the user an
   admin connects one in the Codeport web app, Settings → Clouds; it cannot be done from here.

## Deploy

1. Say what will happen in one line (app, commit, cloud and project, region) and ask the user to confirm,
   unless they already asked for exactly this deploy.
2. Call `deploy_app` with `repo` (and `app` when needed).
3. Follow it with `list_deployments` until it is live, failed or blocked. Do not poll in a tight loop: wait
   between checks and tell the user they can carry on meanwhile.
4. When it is live, give the address. When it failed, show the reason and the log lines the answer gives,
   and suggest the fix in the code if it is clear.

## Blocked deploys

A deploy is blocked by open critical security findings, a missing or failed scan, a cloud that is not active,
or a cloud or region that the organization's stack.md does not allow. Explain which one and how to fix it
(the `security-before-push` skill helps with findings). **Only an admin in the Codeport web app can unblock
findings, with a reason. It cannot be done from here; never try to work around a block.**

## Roll back

1. Call `list_deployments` and show the earlier deploys.
2. Ask the user to confirm which one, then call `rollback_deployment` with its `deploymentId`. Nothing is
   rebuilt: the traffic moves back to that version.

## Rules

- Only deploy or roll back when the user asked for it. Never deploy as a side effect of another task.
- If the tool answers that the user is not an admin, say that an admin of their organization has to do it.
- Settings such as variables, databases, domains and auto-deploy are changed in the Codeport web app.
- If a tool answers that nobody is signed in, run the `codeport-sign-in` skill and then continue.
