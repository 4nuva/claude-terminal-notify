# terminal-notify

> **From the makers of [Agent Terminal](https://4nuva.dev/agent-terminal/).**
> In Agent Terminal these notifications also reach your paired phone, where you
> can open the session and reply. [Get Agent Terminal →](https://4nuva.dev/agent-terminal/)

A Claude Code plugin that raises a desktop notification through your terminal when Claude:

- finishes a turn: `Task complete: Fixed the failing test. All 42 tests pass.`
- needs a permission: `Permission needed: Bash — npm run deploy`
- asks you a question: `Question: Which database should I use?`

The title names the project folder (`Claude Code · my-app`), so you can tell sessions apart.

## Install

At the prompt of a Claude Code session:

```
/plugin install terminal-notify --marketplace 4nuva/claude-terminal-notify
```

Answer `y` to add the marketplace, then pick a scope. From a shell:

```
claude plugin install terminal-notify --marketplace 4nuva/claude-terminal-notify
```

Requires [Node.js](https://nodejs.org/) 18 or later on your `PATH`.

## Terminals

The plugin hands Claude Code a notification escape sequence through the hook's `terminalSequence` output, and picks the sequence for the terminal it detects:

| Terminal | Sequence |
| --- | --- |
| [Agent Terminal](https://4nuva.dev/agent-terminal/) (desktop and phone) | OSC 777 |
| Ghostty, Warp, urxvt | OSC 777 |
| Windows Terminal, iTerm2, WezTerm, ConEmu | OSC 9 |
| kitty | OSC 99 |
| macOS Terminal, VS Code | bell |
| anything else | OSC 9 |

Claude Code has notifications of its own, on by default in Ghostty, kitty and iTerm2. If you see two notifications, set **Notifications** to `notifications_disabled` in `/config` (the `preferredNotifChannel` setting) to keep only this plugin's.

Some terminals only show notifications while their window is in the background, and some need notifications turned on in their own settings (iTerm2: Profiles → Terminal → Notifications). Inside tmux, sequences reach the outer terminal only with `allow-passthrough on`.

## Options

Set these when you install, or later in `/plugin` → terminal-notify → Configure:

| Option | Default | What it does |
| --- | --- | --- |
| `format` | `auto` | `auto`, `osc777`, `osc9`, `osc99`, `all`, `bell` or `off`. `all` sends all three sequences, for a terminal `auto` does not know; a terminal that reads more than one (Ghostty reads 9 and 777) shows more than one. |
| `details` | on | Include the start of the reply, the command or file a permission is for, and the question. Off shows only the event. |
| `idle_reminder` | off | Notify again when the prompt has waited 60 seconds for a reply |

## Privacy

Nothing leaves your machine except through your terminal. Before any text goes into a notification, known key and token formats (API keys, GitHub and Slack tokens, JWTs, bearer tokens, `password=...`, credentials in URLs, long random strings) are replaced with `***`. Turn `details` off to keep reply text and commands out of notifications entirely, for example when notifications show on a lock screen.

Subagents' turns do not notify; only the main session's do.

## Development

```
node --test test/notify.test.js
claude --plugin-dir .
```

## License

MIT

---

**[Agent Terminal](https://4nuva.dev/agent-terminal/)** is a terminal for running coding agents, on your desktop and your phone. It shows these notifications on the desktop and forwards them to your paired phone.
