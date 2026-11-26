---
name: codeport-sign-in
description: Sign the user in to Codeport, or create their account, inside the chat with a code sent to their work email. Use when a Codeport tool answers that nobody is signed in, or when the user asks to sign in, sign up, switch accounts or sign out of Codeport.
---

# Sign in to Codeport

Sign-in happens in the chat. There is no browser step and no password.

1. Call `whoami`. If an account is already signed in and it is the one the user wants, stop here and say who is
   signed in and in which organization.
2. Ask for the user's **work email**, unless they already gave it. Do not guess it from git config or files.
3. Call `auth_start` with that email. Tell the user that Codeport has emailed them a 6-digit code and ask them to
   paste it. A new email creates a new account, so sign-up and sign-in are the same step.
4. Call `auth_verify` with the code. If it was started in another session, pass the email too.
5. Call `whoami` and confirm the account and organization in one line.

Rules:

- Never repeat the code back, write it to a file or keep it for later. Use it once, in `auth_verify`.
- The session is stored in the operating system's keychain by the Codeport server. You never see it; never ask
  for it or look for it.
- If the code is wrong or expired, say so and offer to send a new one (`auth_start` again). Do not retry the same
  code.
- To switch accounts or sign out, call `sign_out`, then start again from step 2 if the user wants another account.
- After signing in, go back to what the user was doing before sign-in was needed.
