# 🔁 GitHub Sync: Installation Protocol
*Make a private GitHub repository the source of truth for your AI's memory*

## Purpose
Executed when the "Load github-sync" command is used. It connects the MemoryCore folder to a private GitHub repo, installs the sync script, and wires three Claude Code hooks.

## Trigger Command
```
"Load github-sync"
```

## Prerequisites
- Core memory system installed (`master-memory.md` exists).
- Git installed (`git --version`); on Windows, Git for Windows (includes Git Bash).
- A GitHub account. The GitHub CLI (`gh`) is optional but makes step 2 one command.

## 6-Step Execution Process

### Step 1: Gather Configuration
- [ ] Ask the user: **"What is your AI's name?"** → `[AI_NAME]` (used in the sync messages).
- [ ] Ask: **"What should I call you when something needs your attention?"** → `[USER_NAME]`.
- [ ] Ask: **"Which name and email should memory commits use?"** → `[GIT_NAME]`, `[GIT_EMAIL]`. This is only a fallback for machines with no git identity, such as cloud sessions.

### Step 2: Connect a PRIVATE GitHub repository
- [ ] If the folder isn't a git repo yet: `git init -b main`.
- [ ] Check `.gitignore` covers secrets: `.env`, `*.key`, `credentials*`, and any local-only folders (for example `attachments/`). Add them if missing.
- [ ] Ask the user for the repo name, then create it **private**:
  - with gh: `gh repo create <name> --private --source . --remote origin`
  - without gh: the user creates it on github.com (Private), then `git remote add origin <url>`.
- [ ] Confirm with the user that it says **Private** before the first push. Memory must never be public.
- [ ] First commit and push: `git add -A && git commit -m "First memory" && git push -u origin main`.

### Step 3: Install the sync script
- [ ] Copy `scripts/memory-sync.sh.template` to `scripts/memory-sync.sh` in the MemoryCore root.
- [ ] Replace `[AI_NAME]`, `[USER_NAME]`, `[GIT_NAME]` and `[GIT_EMAIL]`.
- [ ] Test it: `bash scripts/memory-sync.sh pull` should print "... memory synced from GitHub main (...)".

### Step 4: Wire the hooks
Merge this into the project's `.claude/settings.json`. Keep any existing hooks and permissions:
```json
{
  "permissions": { "allow": ["Bash(git *)", "Bash(bash scripts/memory-sync.sh *)"] },
  "hooks": {
    "SessionStart": [{ "hooks": [{ "type": "command", "command": "bash \"$CLAUDE_PROJECT_DIR/scripts/memory-sync.sh\" pull", "timeout": 60, "statusMessage": "Syncing memory from GitHub" }] }],
    "UserPromptSubmit": [{ "hooks": [{ "type": "command", "command": "bash \"$CLAUDE_PROJECT_DIR/scripts/memory-sync.sh\" prompt", "timeout": 30 }] }],
    "Stop": [{ "hooks": [{ "type": "command", "command": "bash \"$CLAUDE_PROJECT_DIR/scripts/memory-sync.sh\" push", "timeout": 90, "statusMessage": "Saving memory to GitHub" }] }]
  }
}
```
If the **User-Prompt Hook** framework is already installed, keep its master hook too. Claude Code runs both `UserPromptSubmit` entries.

### Step 5: Teach the memory rules
Add a short section to the AI's boot file (for example `CLAUDE.md` or `main/identity-core.md`):
```markdown
## Sync architecture
- GitHub (private repo) is the source of truth; local copies are working copies.
- Hooks pull at session start and before each prompt, and push after every turn. Don't run them by hand unless a hook reports a problem.
- If a hook reports a conflict: tell [USER_NAME] first, then `git pull --rebase`, keep both sides' facts (the newest dated statement wins), continue, push.
- Session details go in their own file, main/sessions/YYYY-MM-DD-<topic>.md, with one pointer line in main/current-session.md, so parallel sessions never edit the same lines.
```

### Step 6: Verify and record
- [ ] Start a new session. The SessionStart message should say memory synced.
- [ ] Make a tiny memory edit and end the turn. Check that `git log origin/main -1` shows the auto-commit.
- [ ] Record in `master-memory.md`: `GitHub Sync — installed [YYYY-MM-DD], repo <owner/name> (private)`.

## Notes
- On Windows, paths containing an apostrophe break `git -C`; the script uses `cd` for that reason.
- Cloud sessions sometimes may not push to `main`. The script then pushes to the session's branch and tells you to merge it.
