# 📌 My Personal Tech Cheat Sheets

Welcome! This repository serves as a **quick reference guide** and centralized hub for various tech commands, snippets, and configurations. It is designed to be highly accessible and tailored for **everyday, self-use development workflows**.

⚠️ **Work in Progress (WIP):** This repo is continuously expanding as I encounter new tools and optimization patterns.

---

## 🚀 Quick Navigation

Click on any topic below to jump directly to its dedicated, full-length cheat sheet:

* 🐳 [Docker & Containers](./docker.md "Docker Cheat Sheet") — Container lifecycle, multi-stage builds, Docker Compose essentials, and disk cleanup.
* 🛡️ [Linux & Bash](./linux.md "Linux & Bash Cheat Sheet") — Files, permissions, text processing, services, networking, and SSH.
* 🛠️ [Git & GitHub](./git.md "Git & GitHub Cheat Sheet") — Config templates, branching, interactive rebases, and undo/recovery.
* 🌐 [Kubernetes & DevOps](./k8s.md "Kubernetes & DevOps Cheat Sheet") — Pod debugging, context switching, rollouts, and Helm quickstarts.

---

## 🗺️ Roadmap (Coming Soon)

Topics queued up for future sheets:

- [ ] 🗄️ **SQL & PostgreSQL** — Common queries, joins, indexes, `psql` meta-commands, backups with `pg_dump`.
- [ ] 🐍 **Python** — venv/`uv`/pip, one-liners, comprehensions, `pytest` flags.
- [ ] 🟨 **Node.js & npm** — npm/pnpm scripts, `nvm`, version management, publishing.
- [ ] 🧾 **Regex** — Syntax, anchors, lookarounds, and ready-made patterns.
- [ ] 🐚 **Bash Scripting** — Variables, loops, conditionals, `set -euo pipefail`, argument parsing.
- [ ] ✍️ **Vim / Neovim** — Motions, text objects, macros, and search/replace.
- [ ] 🪟 **tmux** — Sessions, windows, panes, and copy mode.
- [ ] 🔄 **CI/CD & GitHub Actions** — Workflow syntax, caching, matrices, secrets, and the `gh` CLI.
- [ ] 🏗️ **Terraform** — `init`/`plan`/`apply`, state management, workspaces, modules.
- [ ] ☁️ **Cloud CLIs (AWS / Azure / GCP)** — Auth, profiles, and the everyday commands.
- [ ] 🔏 **Networking & Security** — Ports, DNS, TLS/`openssl`, firewalls (`ufw`), `nmap`.
- [ ] 🪟 **PowerShell & Windows** — Equivalents of the Linux sheet, `winget`, WSL tips.
- [ ] 📐 **JSON & YAML tooling** — `jq` and `yq` filters.

---

## 💡 How I Use This Repo

To get the most utility out of these cheat sheets locally, you can use basic terminal utilities to query your `.md` files without leaving your IDE:

```bash
# Clone this repository to your local machine
git clone https://github.com/MHDMAM/Cheat-Sheets.git && cd Cheat-Sheets

# Quickly search for a specific command across all cheat sheets
grep -i "prune" *.md

# Show matching table rows only (handy for long sheets)
grep -ih "^| \`.*stash" *.md
```

---

## 🎨 Markdown Syntax Refresher (For My Future Self)

A quick layout guide for keeping future sheets clean and unified.

### Sheet Template
Every sheet follows the same skeleton:

```markdown
# <emoji> <Topic> Cheat Sheet

[← Back to Main Index](./README.md "Main Index")

One-line description of what the sheet covers.

---

## <emoji> <Section>

| Command | Action |
| :--- | :--- |
| `command <arg>` | What it does, in one sentence. |
```

### Code Snippets with Copy Support
```bash
# Always use specific syntax highlighting tags next to the backticks
docker system prune -a --volumes
```

### Pipes Inside Tables
A raw `|` breaks a table row — escape it as `\|`, even inside inline code:

```markdown
| `ps aux \| grep <name>` | Find a process by name. |
```

### Collapsible Deep Dives
If a section gets too complex, wrap it in a details block (leave a blank line after `</summary>` so the Markdown inside renders):

<details>
<summary>🔍 Click to expand niche commands...</summary>

```bash
# Complex piping example
history | awk '{print $2}' | sort | uniq -c | sort -rn | head -10
```
</details>

---

## 📝 Contributions & Personal Notes
Since this is a self-use sandbox, I focus purely on density and execution speed rather than detailed explanations. If you happen to stumble upon this and find a typo, feel free to fork it!
