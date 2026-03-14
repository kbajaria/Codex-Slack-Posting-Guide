# Slack Posting Playbook

## Goal

Stage a Slack post from Codex on macOS using an already authenticated user session, ideally leaving the final send to the human operator.

## Preconditions

Before Codex can do this reliably, the machine should have:

- Google Chrome running
- an authenticated Slack workspace in Chrome
- the target channel already open, or at least reachable from an authenticated Slack tab
- macOS Accessibility permission granted for the shell/app Codex is using
- macOS Automation permission granted where needed
- the file to upload already saved locally

## Workflow that worked

### 1. Detect whether Slack is already available

Codex can check for:

- running Chrome process
- existing Slack browser tabs
- known Slack workspace/channel URLs

Example signals:

- browser process list
- Chrome tab titles and URLs via AppleScript
- bookmarks or prior Slack tabs

### 2. Activate the correct Slack channel tab

If the target channel is already open, Codex can switch Chrome to that tab using AppleScript.

This is much safer than trying to navigate an unknown Slack UI from scratch.

### 3. Stage the announcement text

The most reliable message-entry path is:

1. place the final message text on the clipboard
2. bring Chrome forward
3. paste into the Slack composer with `Cmd+V`

This avoids brittle character-by-character typing.

### 4. Stage the file upload

For a local document:

1. reveal the file in Finder
2. select it
3. copy it in Finder
4. switch back to Slack
5. paste into the channel

Slack often interprets this as a file upload.

### 5. Stop before send

Best practice is to leave:

- the message visible in the composer
- the attachment staged or uploaded
- the final send to the user

This keeps a deliberate approval step in the loop.

## Fallback paths

If the fully automated route is blocked, use one of these:

### Fallback A: manual send with prepared assets

Codex provides:

- the final message copy
- the exact local file path
- the target channel URL

The user manually pastes/uploads and clicks send.

### Fallback B: browser-session API route

This is more advanced and should be used carefully.

In some cases Codex can inspect the already authenticated browser session to confirm workspace state or support troubleshooting. This should be treated as sensitive and temporary session material, not as a reusable integration strategy.

### Fallback C: proper Slack app integration

For repeatable production use across many environments, a Slack app or webhook may be better than desktop-session automation.

Use that when:

- posting should happen unattended
- multiple users or machines need the same capability
- channel posting needs auditability and central administration

## Common blockers

### Chrome JavaScript from Apple Events disabled

Chrome may block JavaScript execution via AppleScript. If so, avoid DOM scripting and use clipboard/UI workflows instead.

### macOS Accessibility not enabled

Without Accessibility permission, keystrokes and UI interactions will fail.

### Slack file paste not accepted

If Slack does not accept the pasted file:

- leave the message draft in place
- open Finder with the file already selected
- ask the user to drag the file in or use the attach button

### No authenticated Slack session

If there is no signed-in Slack session:

- stop
- do not attempt to invent credentials
- ask the user to sign in first or provide a sanctioned Slack integration

## Recommended operator workflow

1. Open Slack in Chrome and navigate to the target channel.
2. Save the asset locally first.
3. Ask Codex to stage the post and let you press send.
4. Review the staged message and attachment.
5. Click send yourself.

This gives a good balance between speed and control.
