# 💗 Heart-Memory Inject: Uninstall Protocol
*Removes the reminder; the saved moments stay*

## Trigger Command
```
"uninstall heart-memory-inject"
```

## 3-Step Execution Process

### Step 1: Delete the injector script
- [ ] Delete `~/.claude/hooks/user-prompt-injectors/heart.ps1` (Windows) or `heart.sh` (Unix).
- [ ] Idempotent: skip silently if it's already gone.

### Step 2: Ask about the rule
- [ ] Ask the user whether to keep the "personal moments count as meaningful" rule in main memory. It still helps without the per-prompt nudge.
- [ ] **Never delete the relationship or feelings files.** They are the user's memories, not part of this feature.

### Step 3: Record
- [ ] Update `master-memory.md`: `Heart-Memory Inject — removed [YYYY-MM-DD]`.
