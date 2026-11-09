# 🔁 GitHub Sync: Uninstall Protocol
*Stops automatic syncing; your memory and its GitHub history stay intact*

## Trigger Command
```
"uninstall github-sync"
```

## 4-Step Execution Process

### Step 1: Final sync
- [ ] Run `bash scripts/memory-sync.sh push` once so nothing is left unpushed. If it reports a conflict, resolve it first.

### Step 2: Remove the hooks
- [ ] In `.claude/settings.json`, delete the three entries that call `scripts/memory-sync.sh` (SessionStart, UserPromptSubmit, Stop). Leave every other hook in place.
- [ ] Remove `Bash(bash scripts/memory-sync.sh *)` from `permissions.allow`.

### Step 3: Remove the script and rules
- [ ] Delete `scripts/memory-sync.sh`.
- [ ] Remove the "Sync architecture" section from the boot file. Ask the user whether to keep the "one file per session" rule; it is still useful without sync.

### Step 4: Record
- [ ] Update `master-memory.md`: `GitHub Sync — removed [YYYY-MM-DD]`.
- [ ] Tell the user that the private repo still exists on GitHub and is theirs to keep or delete. **Never delete it yourself.**
