# Codeport plugins

The Codeport plugin for **Codex** and **Claude Code**: estimate an idea before you code, find where to host
your app and what it costs, check it for leaked secrets before you push, follow your organization's
guidelines, and deploy to your own Google Cloud or AWS account.

Needs Node.js 22 or newer: the plugin starts the Codeport MCP server with `npx`.

## Install

**Codex** (CLI, IDE extension, ChatGPT desktop app):

```sh
codex plugin marketplace add codeport-diy/codeport-plugins
```

Then open `/plugins` in Codex and install **Codeport**.

**Claude Code:**

```
/plugin marketplace add codeport-diy/codeport-plugins
/plugin install codeport@codeport
```

## First steps

Ask in the chat:

- _"Sign in to Codeport with me@company.com"_, then paste the code from the email.
- _"Where should I host this project, and what will it cost?"_
- _"Check this project for leaked secrets before I push."_ (works without an account)
- _"Estimate my app idea before I start building."_

## Skills

| Skill                   | What it does                                                                  |
| ----------------------- | ----------------------------------------------------------------------------- |
| `codeport-sign-in`      | Sign in or create an account with a code sent to your work email              |
| `estimate-idea`         | Stack, AI tokens and plan, hours and hosting cost of an idea before coding    |
| `where-to-host`         | Detected stack, monthly cost on nine platforms and the best fit with reasons  |
| `security-before-push`  | Leaked secrets in files and history, vulnerable dependencies, and their fixes |
| `follow-org-guidelines` | Your organization's stack, code standards, security and deployment rules      |
| `deploy-with-codeport`  | Deploy, see what is live, roll back (admins)                                  |

Your code stays on your machine: analysis runs locally and only the stack profile is sent to Codeport.
Secret values and your session are never shown to the AI.

This plugin is published from Codeport's main repository.

Questions: info@codeport.diy · https://codeport.diy
