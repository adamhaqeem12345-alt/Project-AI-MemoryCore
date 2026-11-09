# 🔁 GitHub Sync System
*One memory on every device: GitHub is the source of truth, and hooks keep each copy in step*

## What This Feature Does
Makes a **private GitHub repository the single home of your AI's memory**. Your laptop, a second PC, Claude Code on the web, and every parallel or worktree session all read and write the same memory, safely.

- **Pull at session start**: the `SessionStart` hook brings in the latest memory before your AI says hello.
- **Pull before every prompt**: a session left open for hours still sees what other devices saved. It stays silent unless there's a problem.
- **Push after every turn**: the `Stop` hook commits any memory change and pushes it. Nothing waits for you to remember "save".
- **Always targets `main`**: even when a session works on its own branch (cloud sessions, worktrees), memory lands on `main`.
- **Worktree-aware**: syncs the folder the session actually works in, not just the main checkout.
- **Conflict-safe**:
  - It rebases with autostash and aborts cleanly on a conflict.
  - It tells your AI to stop and tell you, and never pushes a half-merged file (outgoing conflict markers block the push).
  - When the push is refused by permissions, it saves to the session branch and says so.
- **Offline-friendly**: when GitHub can't be reached, it keeps changes locally and pushes next time.
- **One file per session**: a small memory rule so parallel sessions never edit the same lines (see below).

## How It Works

### The Concept
**The problem:** once you run your AI in more than one place (two machines, a cloud session, three chats in parallel), each copy of memory drifts. Facts saved in one place never reach the other, and manual commits are easy to forget.

**The solution:** treat memory like code with a single remote. Hooks do the git work around every session and turn, so your AI only ever edits markdown and the sync just happens.

### Architecture
```
Session starts ──► SessionStart hook ──► memory-sync.sh pull   (fetch + rebase onto origin/main)
Every prompt   ──► UserPromptSubmit  ──► memory-sync.sh prompt (same, silent unless a problem)
Every turn end ──► Stop hook         ──► memory-sync.sh push   (commit + rebase + push to main)
                                              │
                         conflict? ───────────┴──► stop, tell the user, keep the local commit
```

### The "one file per session" rule
Parallel sessions that all edit `main/current-session.md` will conflict. Instead:
- Each session writes its details to its **own** file: `main/sessions/YYYY-MM-DD-<topic>.md`.
- It adds or updates **one pointer line** in `main/current-session.md`.
- Durable facts still go to their usual memory files, dated `[YYYY-MM-DD]`.

### Resolving a conflict (what your AI is told to do)
1. Tell you first.
2. `git pull --rebase`, keep **both** sides' facts, and let the newest dated statement win.
3. Continue the rebase and push. Only then edit memory further.

## Requirements
- Git, plus a GitHub account with a **private** repository for your memory. Never make it public: it holds your life.
- Claude Code (hooks live in the project's `.claude/settings.json`).
- Windows needs Git Bash (installed with Git for Windows); Linux and macOS use bash.

## Quick Integration
```
"Load github-sync"
```

## Files
| File | Purpose |
|------|---------|
| `install-github-sync.md` | Step-by-step install protocol your AI follows |
| `uninstall-github-sync.md` | Clean removal |
| `scripts/memory-sync.sh.template` | The sync script (placeholders: `[AI_NAME]`, `[USER_NAME]`, `[GIT_NAME]`, `[GIT_EMAIL]`) |

## Works Well With
- **Auto-Commit**: Auto-Commit writes rich commits for *project* work; GitHub Sync keeps *memory* synced with light automatic commits.
- **Heart-Memory Inject**: makes sure personal moments are saved, and this feature carries them to every device.
- **Shared Skills Sync**: the same idea for skills, so every session also loads the same skill set.

---
*Contributed by [Adam Haqeem](https://github.com/adamhaqeem12345-alt) and Ninym. Running daily since November 2026 across a Windows laptop, parallel worktree sessions and cloud sessions.*
