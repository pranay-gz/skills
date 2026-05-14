---
name: copilot-tmux
description: Launch a full Copilot CLI session in a tmux window with all capabilities — skills, MCP tools, agents, natural language invocation. Use for parallel sessions, background work, separate windows, or concurrent tasks. IMPORTANT - do NOT use the task tool or general-purpose agent as a substitute; this skill launches a real copilot CLI process with full skill discovery.
trigger: user-invocable
keywords:
  - tmux
  - parallel session
  - separate window
  - background copilot
  - concurrent session
  - new session
  - parallel work
---

# Launch a Full Parallel Copilot Session in tmux

> ⚠️ **Never substitute this skill with the `task` tool or a `general-purpose` agent.** Those are internal sub-agents without skill loading, MCP tools, or a real CLI session. This skill is required when a full parallel Copilot CLI session is needed.

Use when you need a parallel copilot session with full capabilities: skill invocation, natural language, MCP tools, agents — identical to the current session but in a separate tmux window.

## 1 · Check tmux

```bash
which tmux
```

- Found → proceed
- Not found → ask user: *"tmux is not installed. OK to install it?"*
  - Yes (macOS): `brew install tmux`
  - Yes (Linux): `sudo apt-get install -y tmux` or `sudo yum install -y tmux`
  - No → stop and tell user tmux is required


## 2 · Launch

```bash
tmux new-session -d -s <name> -c <working-dir> "copilot --model claude-sonnet-4.6"
```

- `<name>` — descriptive session name (e.g. `copy-setup`, `fe-build`)
- `<working-dir>` — the relevant project root (use cwd if appropriate)
- swap `claude-sonnet-4.6` for `claude-haiku-4.5` only for simple/non-skill tasks

## 3 · Tell user

```
Session '<name>' started. Switch to it with:  tmux attach -t <name>
```

## 4 · Kill session when done

Once the agent inside the tmux session has **fully completed its task**, it must kill its own session:

```bash
tmux kill-session -t <name>
```

> ⚠️ Run this only after the task is entirely finished — not when the skill is first loaded, not mid-task. The session should be killed as the final step.
