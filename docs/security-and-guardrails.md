# Security and Guardrails

## Safe default

The safest reusable pattern is:

- use an existing authenticated Slack session on the local Mac
- let Codex stage the message and attachment
- keep the final send as a human action

## Avoid these mistakes

- Do not store Slack cookies or tokens in source control.
- Do not publish local session extraction steps together with real token values.
- Do not assume desktop automation is stable enough for unattended posting.
- Do not let cross-project automation silently post into the wrong channel.

## Guardrails to keep

- Always name the target channel explicitly.
- Prefer staging over automatic send.
- Confirm the file path before upload.
- Use the currently signed-in user session rather than shared credentials in a repo.
- If Slack authentication is missing, stop and ask for sign-in rather than improvising.

## When to choose a real Slack integration instead

Use a Slack app, bot token, or webhook when:

- posts need to happen on schedules
- posts must run from CI/CD or servers
- multiple teammates need the same automation
- auditability matters more than convenience

## Good operator prompt

```text
Please post this file to Slack channel #channel-name using the authenticated Slack session on my Mac, and leave the post unsent so I can review it first.
```

## Better than direct send

Leaving the final click to the user reduces:

- wrong-channel mistakes
- wrong-file mistakes
- premature sends while an attachment is still uploading

It is a small friction cost for a big safety gain.
