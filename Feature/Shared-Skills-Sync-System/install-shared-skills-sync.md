# 🧰 Shared Skills Sync: Installation Protocol
*One private skill marketplace, loaded by every session*

## Trigger Command
```
"Load shared-skills-sync"
```

## Prerequisites
- Claude Code with plugin support (`claude plugin --help` works).
- Git and a GitHub account. The GitHub CLI (`gh`) makes step 2 one command.
- Recommended: GitHub Sync System, so the project settings travel with your memory.

## 6-Step Execution Process

### Step 1: Gather Configuration
- [ ] Ask the user: **"What should your skills marketplace be called?"** → `[MARKET]` (e.g. `my-skills`, `<name>-skills`).
- [ ] Ask: **"Do you already have skills in `~/.claude/skills` or in other chats you'd like to bring along?"** List what you find.

### Step 2: Create the marketplace repo (private)
- [ ] Create a folder `[MARKET]` **outside** the MemoryCore folder, then `git init -b main`.
- [ ] Create `.claude-plugin/marketplace.json`:
```json
{
  "name": "[MARKET]",
  "owner": { "name": "<user name>" },
  "plugins": []
}
```
- [ ] Create `UPSTREAM.md` (a table: plugin, upstream repo, paths copied, commit, licence) and `PATCHES.md` (changes made to vendored files).
- [ ] Create the GitHub repo **private**: `gh repo create [MARKET] --private --source . --remote origin`, then push.

### Step 3: Add the first plugin
- [ ] For each skill to add, make `plugins/<name>/.claude-plugin/plugin.json` (name, version, description, licence, repository) and put the skill folder(s) in `plugins/<name>/skills/`.
- [ ] For third-party skills, follow the review loop in the README: licence → scan scripts → copy with LICENSE → record in `UPSTREAM.md`/`PATCHES.md`.
- [ ] Add each plugin to `marketplace.json`'s `plugins` array: `{ "name": "<name>", "source": "./plugins/<name>", "description": "...", "version": "..." }`.
- [ ] Commit and push.

### Step 4: Register the marketplace in the MemoryCore project
- [ ] Run, from the MemoryCore folder:
  - `claude plugin marketplace add <owner>/[MARKET]`
  - `claude plugin install <name>@[MARKET] --scope project` for each plugin.
- [ ] Confirm `.claude/settings.json` now has `extraKnownMarketplaces.[MARKET]` (a github source) and `enabledPlugins` entries. Add `extraKnownMarketplaces` by hand if it's missing, so other machines and cloud sessions can find the marketplace.

### Step 5: Teach the rule
Add to the boot file:
```markdown
## Skills and tools
- Shared skills live in the private marketplace <owner>/[MARKET]. This project's .claude/settings.json enables them, so every session gets the same set. New sessions load them at start.
- To add a skill: clone it to a scratch folder, check the licence, scan its scripts (network, telemetry, hooks), vendor it with its LICENSE, record it in UPSTREAM.md and PATCHES.md, push, then `claude plugin install <name>@[MARKET] --scope project`.
- Software (apps, models, CLIs) stays on the machine it's installed on; list it in technical-profile memory.
```

### Step 6: Verify and record
- [ ] Start a **new** session (skills load at session start) and check the skills appear.
- [ ] Record in `master-memory.md`: `Shared Skills Sync — installed [YYYY-MM-DD], marketplace <owner>/[MARKET]`.

## Updating a vendored skill later
Compare upstream against the pinned commit, read the diff for anything new (network, hooks, telemetry), copy the update, bump the commit in `UPSTREAM.md`, then push. Sessions pick it up after `claude plugin marketplace update [MARKET]` and a restart.
