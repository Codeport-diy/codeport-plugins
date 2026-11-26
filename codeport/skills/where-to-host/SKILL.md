---
name: where-to-host
description: Analyze the project with Codeport and recommend where to host it - detected stack, monthly cost on AWS, Google Cloud, Azure, Vercel, Render, Railway, Fly.io, Netlify and Cloudflare, and the best fit with reasons. Use when the user asks where to deploy or host, what hosting will cost, or compares platforms for the code in this workspace or in a connected GitHub repository.
---

# Where to host this project, and what it costs

## Code on this machine

1. Call `analyze_project` for the workspace folder (or the folder the user names). It detects the stack locally;
   only the stack profile is sent to Codeport, never the code or secret values. The project is saved in the
   organization's catalog.
2. Ask about the expected load only if the user cares about precision. Otherwise use the default scenario. The
   scenarios are team (~50 users), department (~500) and company (~5,000); the user may give their own users,
   requests per day and data size. Ask for Europe or US pricing only when it matters to them.
3. Call `recommend_target` with the same load. Present:
   - the best fit and the two or three reasons that decide it;
   - the runner-up, and what would make it better;
   - the monthly cost of the top options.
4. When the user wants the bill behind a number, call `estimate_cost` with `platform` for the itemized bill
   with unit prices and their sources.

## Code in GitHub

If the code is not on this machine, or the user asks about a repository, call `analyze_repository` with `repo`.
If no repository is connected, call `connect_github` and give the user the link it returns; the Codeport GitHub
App is read-only and sees only the repositories they pick. Then run `analyze_repository` again.

## Rules

- Be honest about the fit. The best platform may be one Codeport cannot deploy to yet: recommend it anyway, and
  name the best one Codeport can deploy to, as the tool answer does. Do not hide it.
- Show each cost as one average with "~" ("~$18/month"), never a "from-to" range.
- Mention what is not priced (a service a platform does not offer) when it changes the choice.
- If the organization's guidelines limit clouds or regions, the answer marks which platforms are allowed. Lead
  with the best allowed one and say why the others are out.
- If a tool answers that nobody is signed in, run the `codeport-sign-in` skill and then continue.
- When the user wants to go live after this, use the `deploy-with-codeport` skill.
