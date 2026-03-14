# Codex Slack Posting Guide

This repo documents a reusable method for having Codex help post files and announcements into Slack channels using your existing authenticated Slack session on macOS.

## What this is for

Use this playbook when:

- you want Codex to help draft and stage a Slack post
- the target Slack workspace is already signed in on your Mac
- you want to attach a local file such as a PDF, image, or release note
- you are comfortable doing the final review and send yourself

## What worked in practice

In the NovaLXP dashboard workflow, Codex was able to stage a Slack post by combining:

- an already authenticated Slack session in Google Chrome
- macOS automation permissions for Codex/Terminal
- browser tab discovery so Codex could switch to the correct Slack channel
- clipboard-based message pasting
- Finder-based file selection and paste/upload into Slack

The most reliable pattern was:

1. Confirm the Slack channel is already open in Chrome.
2. Put the message text on the clipboard.
3. Activate Chrome and paste into the Slack composer.
4. Select the local file in Finder.
5. Copy the file in Finder and paste it into the Slack tab.
6. Leave the post unsent so the user can review and click send.

## Why this approach is useful

- It avoids hard-coding Slack bot tokens into project repos.
- It works across projects as long as the same Mac is signed into Slack.
- It keeps a human approval step before the final send.
- It is often faster than building a dedicated Slack integration for one-off announcements.

## Important limitations

- This depends on a live desktop session.
- Slack must already be authenticated in the browser.
- macOS Accessibility and Automation permissions must be enabled.
- Browser UI automation can be brittle if Slack changes its layout.
- File upload by paste may not work in every browser state, so a manual drag-and-drop fallback is still useful.

## Security guidance

- Do not commit extracted Slack tokens, cookies, or local session data.
- Prefer browser-session and UI-assisted workflows over storing long-lived secrets in repos.
- Keep Codex focused on staging and drafting unless you explicitly want it to send automatically.
- Treat any local browser session data as sensitive and ephemeral.

## Repo contents

- `docs/slack-posting-playbook.md` - end-to-end workflow and prerequisites
- `docs/security-and-guardrails.md` - what to avoid and how to use this safely
- `docs/request-template.md` - a reusable prompt you can give Codex in future projects

## Recommended usage pattern

Ask Codex for a staged post like this:

```text
Please post this file to my Slack channel #channel-name with a short announcement.
Use the authenticated Slack session on my Mac if available, and let me press send myself.
```

That prompt encourages a safe default:

- Codex checks for an authenticated Slack session
- Codex stages the message and attachment
- you do the final review and send

## Next enhancement ideas

- add a tiny local helper to verify macOS permissions before attempting a post
- add a browser choice guide for Chrome vs Slack desktop app
- add optional Slack API guidance for teams that prefer bot-based posting
