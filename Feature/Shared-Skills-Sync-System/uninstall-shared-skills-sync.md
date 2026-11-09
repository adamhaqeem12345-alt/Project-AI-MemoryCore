# 🧰 Shared Skills Sync: Uninstall Protocol
*Stops loading the shared skills in this project; the marketplace repo stays yours*

## Trigger Command
```
"uninstall shared-skills-sync"
```

## 3-Step Execution Process

### Step 1: Disable the plugins
- [ ] For each enabled plugin: `claude plugin uninstall <name>@[MARKET] --scope project`.
- [ ] Remove `extraKnownMarketplaces.[MARKET]` and any leftover `enabledPlugins` entries from `.claude/settings.json`.

### Step 2: Remove the rule
- [ ] Remove the "Skills and tools" section from the boot file, or ask the user whether to keep it as documentation.

### Step 3: Record
- [ ] Update `master-memory.md`: `Shared Skills Sync — removed [YYYY-MM-DD]`.
- [ ] Tell the user the marketplace repo still exists on GitHub. **Never delete it yourself.**
