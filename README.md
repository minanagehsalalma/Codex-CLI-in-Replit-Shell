# 🤖 Codex CLI in Replit Shell — Complete Setup Guide

> **OpenAI's Codex CLI** is a powerful terminal-based coding agent that reads your codebase, writes and edits files, and runs commands — all from a shell prompt. This guide walks you through running it inside **Replit's Shell**.

---

## 📋 Table of Contents

1. [Prerequisites](#prerequisites)
2. [Step 1 — Open the Replit Shell](#step-1--open-the-replit-shell)
3. [Step 2 — Install Codex CLI](#step-2--install-codex-cli)
4. [Step 3 — Authenticate](#step-3--authenticate)
   - [Option A: API Key (Recommended for Replit)](#option-a-api-key-recommended-for-replit)
   - [Option B: ChatGPT Subscription (Localhost Workaround)](#option-b-chatgpt-subscription-localhost-workaround)
5. [Step 4 — Launch Codex](#step-4--launch-codex)
6. [Step 5 — Choose an Approval Mode](#step-5--choose-an-approval-mode)
7. [Step 6 — Start Coding with Codex](#step-6--start-coding-with-codex)
8. [Useful Commands & Tips](#useful-commands--tips)
9. [Troubleshooting](#troubleshooting)

---

## Prerequisites

Before starting, make sure you have:

- ✅ A **Replit** account (free or paid)
- ✅ Either an **OpenAI API key** (from [platform.openai.com](https://platform.openai.com/api-keys)) **or** a **ChatGPT Plus/Pro subscription**
- ✅ A Repl to work in (any language)

> 💡 Using an **API key** is the easiest path in Replit — the ChatGPT login flow tries to open `localhost`, which doesn't work in a browser-based IDE without a workaround (covered below).

---

## Step 1 — Open the Replit Shell

1. Open your Repl on [replit.com](https://replit.com)
2. In the bottom panel, click the **Shell** tab

![Replit Shell Tab](https://i.imgur.com/placeholder-shell.png)

> The shell is a full Linux terminal running inside your Repl. You'll run all Codex commands here.

---

## Step 2 — Install Codex CLI

In the Shell, run:

```bash
npm install -g @openai/codex
```

![npm install codex](https://i.imgur.com/placeholder-install.png)

Wait for the installation to complete. You should see a success message with the installed version.

Verify the install worked:

```bash
codex --version
```

---

## Step 3 — Authenticate

Codex supports two authentication methods. **Choose one:**

---

### Option A: API Key (Recommended for Replit)

This is the simplest method — no browser redirects needed.

**1. Set your API key as an environment variable:**

In the Replit Shell:

```bash
export OPENAI_API_KEY=sk-your-key-here
```

Or, for persistence across sessions, add it to Replit's **Secrets**:

1. Click the **🔒 Secrets** tab in the left sidebar
2. Add a new secret:
   - **Key:** `OPENAI_API_KEY`
   - **Value:** `sk-your-key-here`
3. Click **Add Secret**

![Replit Secrets Panel](https://i.imgur.com/placeholder-secrets.png)

> Secrets are automatically loaded into your shell environment — you won't need to `export` them every session.

**2. Run Codex:**

```bash
codex
```

When prompted to choose a login method, **select option 2** (API Key). Codex will confirm it found your key and proceed.

---

### Option B: ChatGPT Subscription (Localhost Workaround)

Replit's shell doesn't support opening `localhost` in a browser — so you need a workaround to complete the OAuth flow.

**1. Start Codex:**

```bash
codex
```

**2. Select "Sign in with ChatGPT"** and proceed through the prompts until you hit the **localhost error page** in your browser.

**3. Copy the full localhost URL** from your browser's address bar. It will look like:
```
http://127.0.0.1:PORT/callback?code=xxxxx&state=xxxxx
```

**4. Leave that Shell tab open. Open a second Shell tab.**

In the new shell, run these commands — pasting your copied URL:

```bash
URL='http://127.0.0.1:PORT/callback?code=xxxxx&state=xxxxx'
curl -i "$URL"
```

![Two Shell Tabs Workaround](https://i.imgur.com/placeholder-curl.png)

**5. Go back to the first shell tab.** Codex should now be authenticated and launch into the interactive TUI.

---

## Step 4 — Launch Codex

Once authenticated, start Codex from your project directory:

```bash
codex
```

You'll see the **Codex TUI (Terminal UI)** — an interactive chat interface right in your shell:

![Codex TUI Interactive Shell](https://canada1.discourse-cdn.com/flex010/uploads/replit/original/2X/b/bdc41050d3cce46f8ce4ad778432797e64a16a53.png)

> Codex automatically reads the files in your current directory and uses them as context for every prompt.

---

## Step 5 — Choose an Approval Mode

When Codex starts (or when you run it), it asks how much autonomy to give it:

| Mode | What It Means | Best For |
|------|--------------|----------|
| **Read Only** (default) | Codex can read files and suggest changes, but asks before editing anything | Safe exploration, code review |
| **Auto Edit** (`--full-auto`) | Codex edits files automatically, asks before running shell commands | Daily development |
| **Full Auto** (`--dangerously-bypass-approvals-and-sandbox`) | Codex does everything without asking | CI/CD, isolated environments only |

For most Replit usage, **Auto Edit** is the sweet spot:

```bash
codex --full-auto "your task here"
```

---

## Step 6 — Start Coding with Codex

Type your task in plain English inside the Codex TUI. Here are some examples to get you going:

```
> Explain what this project does

> Add input validation to the login form

> Write unit tests for the auth module

> Fix the bug where users can't log out

> Refactor index.js to use async/await instead of callbacks

> Create a README for this project
```

Codex will:
1. **Read** the relevant files in your repo
2. **Plan** the changes
3. **Stream** the response back to your terminal
4. **Ask for approval** (in auto-edit mode) before modifying files or running commands

---

## Useful Commands & Tips

### While Codex is Running

| Action | Key |
|--------|-----|
| Inject a new instruction mid-task | `Enter` |
| Queue a follow-up for the next turn | `Tab` |
| Run a shell command directly | Prefix with `!` (e.g. `!ls`) |
| Edit your previous message | `Esc` twice, then `Enter` |
| Switch models | `/model` |
| Open code review mode | `/review` |

### Useful Flags

```bash
# Start in a specific directory without cd-ing first
codex --cd /path/to/project

# Enable network access inside the sandbox
codex -c 'sandbox_workspace_write.network_access=true' "npm install and run tests"

# Non-interactive mode (pipe results to stdout)
codex exec "Update the CHANGELOG"
```

### Pro Tips for Replit

- 🔑 **Always use Secrets** for your API key — never hardcode it in your shell history
- 📁 **Navigate to your project folder first** — Codex reads context from the current directory
- 🔄 **Use `/clear` between tasks** — keeps your context window clean and tokens low
- 💾 **Commit your work before big changes** — Codex can edit multiple files at once; a git checkpoint lets you revert if needed:
  ```bash
  git add -A && git commit -m "checkpoint before codex session"
  ```
- 🚀 **Use `--full-auto` for repetitive tasks** like running tests, updating docs, or reformatting code

---

## Troubleshooting

### ❌ `localhost` error when logging in
**→ Use Option A (API Key)** or follow the two-shell curl workaround in Step 3B.

### ❌ `command not found: codex` after install
**→** The global npm bin path may not be in your `$PATH`. Run:
```bash
export PATH="$PATH:$(npm bin -g)"
```
Or re-run: `npm install -g @openai/codex`

### ❌ Codex keeps asking for approval even with `--full-auto`
**→** Check your settings inside the TUI with `/status`. Restart with explicit flags:
```bash
codex -a never -s workspace-write "your task"
```

### ❌ Network errors (npm install fails inside Codex)
**→** Enable network access in the sandbox:
```bash
codex -a never -s workspace-write \
  -c 'sandbox_workspace_write.network_access=true' \
  "npm install"
```

### ❌ Sandbox errors on Replit (Landlock/seccomp unsupported)
**→** Run with the danger bypass flag (safe inside Replit's already-sandboxed environment):
```bash
codex --dangerously-bypass-approvals-and-sandbox "your task"
```

---

## 📚 Resources

- [Codex CLI Docs](https://developers.openai.com/codex/cli/)
- [Codex CLI GitHub](https://github.com/openai/codex)
- [OpenAI API Keys](https://platform.openai.com/api-keys)
- [Replit Community Forum — Codex Discussion](https://replit.discourse.group/t/using-codex-with-replit/7214)

---

*Made with ❤️ for Replit builders. Happy vibe-coding!*
