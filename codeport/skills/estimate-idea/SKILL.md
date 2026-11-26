---
name: estimate-idea
description: Estimate an app idea with Codeport before any code exists - the stack it needs, the AI tokens and which AI coding plan to buy, how many hours and days it takes at the user's pace, and what hosting it will cost. Use when the user describes an app they want to build, or asks how long, how much or which plan before starting.
---

# Estimate an idea before building it

Use this when there is an idea and no code yet. For code that exists, use the `where-to-host` skill instead.

1. Make sure you have a description of the idea: what the app does, who uses it, roughly how many users, and
   anything special (payments, files, AI calls, real-time, sign-in). If it is one vague line, ask one question
   to fill the biggest gap, then go on.
2. Ask how many hours a day the user can spend on it and whether they are new to building apps or experienced,
   unless they already said. Skip the question if they want a quick answer.
3. Call `estimate_idea` with `idea`, and `hoursPerDay` and `pace` when known. The description is sent to
   Codeport AI; tell the user before you send it if it holds anything they may not want to share, and never
   put secrets, keys or customer data in it.
4. Present the answer briefly:
   - the proposed stack, in one line;
   - effort: hours and calendar days at their pace;
   - which AI coding plan to buy and why;
   - hosting: the recommended platform and its monthly cost.
5. To show an earlier estimate again, call `estimate_idea` with `ideaId`. For "what if I had 2 hours a day",
   call it again with the same `ideaId` and other `hoursPerDay` or `pace`: the stored advice does not change,
   the answer adds the what-if.

Rules:

- Show every price and duration as one average with "~" (for example "~$24/month", "~6 days"), never as a
  "from-to" range.
- AI features belong to Codeport: call them "Codeport AI".
- If a tool answers that nobody is signed in, run the `codeport-sign-in` skill and then continue.
- If the organization has guidelines (stack.md), the estimate already follows them. Mention it when the proposed
  stack differs from what the user asked for because of them.
