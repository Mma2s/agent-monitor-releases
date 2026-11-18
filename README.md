# Agent Monitor

A native macOS menu-bar app with a small floating panel that shows every Claude Code agent running on your Mac,
what each one is doing right now, and everything it did before. You can answer its permission requests, questions
and plan approvals straight from the panel while you work in other apps.

**[Download the latest version](https://github.com/Mma2s/agent-monitor-releases/releases/latest)**

## Why Agent Monitor

The more agents you run, the harder it gets to keep track of what they're all doing. Some run in your terminal,
spread over several tabs or windows, others in the Claude desktop app or your IDE. When an agent needs a permission, an answer
or a plan approval, it stops and waits, and nothing tells you unless you happen to look at the right tab. So you come
back from an hour-long meeting and find that an agent asked for your approval five minutes after you left, and has
been waiting ever since.

Agent Monitor puts all your agents in one small panel that you can pin on top of your other windows. When an agent
needs you, its card pops up in the panel, and the menu-bar icon shows how many requests are waiting. A glance is
enough, even in a meeting or while you're busy with something else, and you can answer right there. Your agents keep
building while you work on something else, instead of sitting idle until you're back.

## Features

### See your agents

- **All agents at a glance**, grouped by project. Each one shows its Claude Code session name, what it is doing right
  now and how long it has been running, with its own colour.
- **Task progress.** When an agent keeps a task list, its row shows a progress bar ("3 of 7 tasks"). Expand the row
  to see the tasks as a checklist.
- **Subagents on their own rows**, each with its status, progress and activity. The main conversation's row is
  called **General**.
- **Activity history.** Click an agent to see everything it did: prompts, commands, file reads and edits, searches,
  subagents, permission decisions and finished turns.
- **Finished agents disappear** when their session ends or their terminal is closed. You can also swipe a finished
  agent away, with Undo.

### Act on them

- **Answer from the panel:**
  - **permission requests:** Allow, Always Allow, Deny, or Deny with a reason;
  - **Claude's questions**, including a free-text answer;
  - **plans:** approve, approve with notes, or keep planning with feedback.
- **Terminal and panel stay in sync.** Answer in either place: whichever you answer first wins.
- **Jump to the agent.** Click an agent's name to bring its app to the front: Terminal, iTerm, VS Code, Cursor or the
  Claude desktop app.
- **Doesn't interrupt you.** The panel pops up silently, without taking keyboard focus, so you never type into it by
  accident. Pin it on top, hide it, or open it again from the menu-bar icon.
- **Works with** Claude Code in any terminal, local sessions in the Claude desktop app, and background and
  `claude -p` sessions.

## Requirements

- macOS 26 Tahoe or later, on Apple silicon or Intel.
- Claude Code 2.1.311 or later.

## Install

1. Download `Agent-Monitor-<version>.dmg` from the
   [latest release](https://github.com/Mma2s/agent-monitor-releases/releases/latest).
2. Open the DMG and drag **Agent Monitor** onto **Applications**. Open it from Applications, not from the disk image.
3. **First launch only.** Agent Monitor isn't signed with an Apple Developer ID yet, so macOS blocks it the first
   time ("Agent Monitor" Not Opened). Click **Done**, then:
   - open **System Settings › Privacy & Security**;
   - scroll down to the message about Agent Monitor and click **Open Anyway**;
   - confirm with your password or Touch ID, then click **Open Anyway** in the last dialog.

   From then on it opens normally. (Right-click › Open no longer skips this step on macOS 26.)
4. Click the sparkle icon in the menu bar, open the panel's **⋯** menu and choose **Connect to Claude Code…**.

Sessions that are already running appear after their next step.

To check the download, compare its SHA-256 with the `.sha256` file on the release page:

```sh
shasum -a 256 -c Agent-Monitor-<version>.dmg.sha256
```

## How it works

**Connect to Claude Code** adds a few hook entries to `~/.claude/settings.json` that point to a small helper app.
Your own settings and hooks are kept exactly as they are, and a backup of the file is saved first.

When Claude Code runs a hook, the helper writes a short record for the app and exits in a few milliseconds, so it
doesn't slow Claude down. For a permission request, question or plan, the helper asks the app over a local connection
that only your user account can reach. If the app isn't running, Claude Code simply asks in the terminal as usual.

**Everything stays on your Mac.** Agent Monitor sends nothing over the network.

## Optional: task lists for progress bars

Progress bars need the agent to keep a task list, which Claude Code doesn't always do by default. Choose
**⋯ › Let Claude Keep Task Lists** to turn it on. This adds one line to your Claude Code settings (a backup is saved
first). It applies to sessions you start afterwards, and Claude will use slightly more of your usage to keep the
list up to date. Turning it off removes the line again.

## Update to a new version

There's no built-in updater yet. To be told about new versions, choose **Watch › Custom › Releases** at the top of
this page. To update:

1. Quit Agent Monitor: right-click the sparkle in the menu bar › **Quit Agent Monitor**.
2. Open the new DMG, drag **Agent Monitor** onto **Applications** and choose **Replace**.
3. Open Agent Monitor from Applications. If macOS blocks it, use **Open Anyway** again.

You don't need to connect again. Your connection and history are kept.

## Disconnect or uninstall

- **Disconnect from Claude Code…** (⋯ menu) removes Agent Monitor's hooks from your Claude Code settings and keeps the app.
- **Uninstall Agent Monitor…** (⋯ menu) removes the hooks, deletes Agent Monitor's activity history and logs, and
  quits. Then move the app to the Trash. Backups of your settings file are kept unless you tick
  **Also delete settings backups**. Those backups can contain API keys if your settings file has them.

If you moved the app to the Trash without uninstalling, run this once in Terminal to remove the hooks:

```sh
"$HOME/Library/Application Support/Agent Monitor/bin/agent-monitor-hook" uninstall
```

It shows what it will remove and asks you to confirm. Afterwards you can delete the
`~/Library/Application Support/Agent Monitor` folder.

## Known limitations

- Desktop-app sessions that run in a cloud or VM sandbox aren't monitored, only local ones.
- Clicking an agent's name brings its app to the front, not the exact terminal tab or window.
- Progress bars appear only for agents that keep a task list.
- If your Claude Code permission mode is `auto`, Claude approves most actions itself, so you'll see fewer requests.
- The first launch on each Mac needs the **Open Anyway** step, until the app is signed with a Developer ID.

## License

Copyright © 2026 Marco dos Santos. All rights reserved. See [LICENSE](LICENSE).

The source code isn't public. This repository only hosts the downloads.
