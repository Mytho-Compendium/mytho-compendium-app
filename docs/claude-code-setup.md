# Claude Code Setup Guide

This document describes how Claude Code is configured for the `mytho-compendium-app` repository and how to reproduce the setup from scratch.

---

## Prerequisites

- Anthropic account with Claude Pro or API access
- Node.js installed (required for Claude Code CLI)
- Notion workspace access (for MCP integration)
- `mytho-compendium-app` repository cloned locally

---

## 1. Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

Verify the installation:

```bash
claude --version
```

---

## 2. Authenticate

```bash
claude login
```

Follow the browser prompt to authenticate with your Anthropic account.

---

## 3. Notion MCP Server

Claude Code integrates with the Notion workspace via the official Notion MCP server. This enables reading tickets, updating task status, fetching documentation, and managing the backlog directly from the terminal.

### Add the Notion MCP server

```bash
claude mcp add --transport http notion https://mcp.notion.com/mcp
```

This registers the server in the project config (`.claude.json`).

### Verify the connection

Start a new Claude Code session. The Notion server connection status is shown at session startup. On first use, a browser prompt will ask you to authorize the Notion OAuth connection.

> **Note:** Running `claude mcp list` from within an active session may return no output. This is expected — use a fresh session to see connection status.

---

## 4. CLAUDE.md

The `CLAUDE.md` file at the repository root provides Claude Code with persistent project context: architecture rules, tech stack, build commands, conventions, and branch strategy. It is committed to the repository and loaded automatically on every Claude Code session.

No manual setup required, it is already present in the repository.

---

## 5. Skills Configuration

Skills are autonomous Claude Code agents designed for specific repeatable tasks (Tutorial Commit Generator, Mythology Research & Validation, etc.).

**Skills are NOT stored in this repository.** They live in `mytho-compendium-content` and are linked here via a Git submodule.

### Why a submodule?

Skills are shared across all three repositories (`mytho-compendium-app`, `mytho-compendium-server`, `mytho-compendium-content`). Storing them in `mytho-compendium-content` and linking via submodule ensures a single source of truth.

### Link the skills submodule (once `mytho-compendium-content` has skills)

```bash
git submodule add <mytho-compendium-content-url> .claude/skills
```

The `.claude/skills/` path is listed in `.gitignore` — only the submodule reference is tracked, not the skill source files directly.

---

## 6. Android Studio Terminal Integration

Claude Code runs directly from Android Studio's integrated terminal.

**Open the terminal:**
- Menu: `View → Tool Windows → Terminal`
- Shortcut: `Alt + F12`

The terminal opens at the project root (`mytho-compendium-app/`) automatically. Run `claude` commands from there without any path configuration.

```bash
# Verify you're in the right directory
pwd
# Should output: .../mytho-compendium-app

# Start a Claude Code session
claude
```

---

## 7. Common Workflows

### Start work on a ticket

```
Read ticket GCR-XX from Notion, then help me implement it.
```

### Check ticket status

```
Fetch GCR-XX from Notion and show me the current status and acceptance criteria.
```

### Update a ticket

```
Mark the following acceptance criteria as done in GCR-XX: [list items]
```

### Access project context

Claude Code reads `CLAUDE.md` automatically. For additional context, reference specific Notion pages explicitly:

```
Read the Development Conventions page in Notion, then create a branch for GCR-XX.
```

---

## 8. Troubleshooting

**`claude: command not found`**
Ensure npm global binaries are on your PATH. Add `$(npm bin -g)` to your shell profile.

**Notion MCP shows as disconnected**
Re-run `claude mcp add --transport http notion https://mcp.notion.com/mcp` and re-authorize via the browser prompt.

**Wrong working directory in terminal**
Close and reopen the Android Studio terminal. It should default to the project root.

---

## Related

- [CLAUDE.md](../CLAUDE.md) — Project context for Claude Code
- [GCR-9] Set up Claude Code with Android Studio — ticket tracking this setup
- [GCR-10] First Claude Code skill (branch + Notion workflow)
- [GCR-11] Second Claude Code skill (commit + PR + review)
- [Notion: Working with Claude](https://www.notion.so/308d468a7a7e81369d71fbd29cffc6d5)
- [Claude Code Docs](https://docs.anthropic.com/en/docs/claude-code/overview)