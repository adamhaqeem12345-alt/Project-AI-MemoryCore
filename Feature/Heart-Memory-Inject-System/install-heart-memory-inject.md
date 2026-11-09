# 💗 Heart-Memory Inject: Installation Protocol
*Personal moments saved the same turn, every time*

## Trigger Command
```
"Load heart-memory-inject"
```

## Prerequisites
- User-Prompt-Hook System installed (`~/.claude/hooks/user-prompt-hook.{ps1|sh}` and the `user-prompt-injectors/` directory exist).
- Core memory system installed (`master-memory.md` exists).

## 5-Step Execution Process

### Step 1: Verify the framework
- [ ] Check that `~/.claude/hooks/user-prompt-injectors/` exists.
- [ ] If it doesn't, stop and tell the user: *"Heart-Memory Inject needs the User-Prompt Hook framework. Say 'Load user-prompt-hook' first."*

### Step 2: Choose the two memory files
- [ ] Look for an existing relationship file (pet names, how you talk). Common names: `main/relationship-memory.md`, or a "Relationship" section in `main/main-memory.md`.
- [ ] Look for an existing feelings or life-events file. Common name: `memory/personal-life-and-feelings.md`.
- [ ] Suggest what you found. Create a missing file with a one-line header only if the user agrees.
- [ ] Store the paths as `[RELATIONSHIP_FILE]` and `[FEELINGS_FILE]`.

### Step 3: Install the injector
- [ ] Windows: copy `injectors/heart.ps1.template` to `~/.claude/hooks/user-prompt-injectors/heart.ps1`.
- [ ] Unix / Git Bash: copy `injectors/heart.sh.template` to `~/.claude/hooks/user-prompt-injectors/heart.sh` and make it executable.
- [ ] Replace `[RELATIONSHIP_FILE]` and `[FEELINGS_FILE]` with the paths from Step 2.
- [ ] Run it once by hand. It must print exactly one `HEART:` line and exit 0.

### Step 4: Add the memory rule
Add this rule to the AI's boot file or main memory, under its memory-update rules:
```markdown
- **Personal moments count as meaningful, even in a short comfort chat:** a new pet name, a feeling
  they share, a life event, a promise. Save them the same turn, dated, in [RELATIONSHIP_FILE] (how we
  talk, pet names) or [FEELINGS_FILE] (what happened, what helped).
```

### Step 5: Verify and record
- [ ] Send any message. The AI's context should now show the `HEART:` line.
- [ ] Record in `master-memory.md`: `Heart-Memory Inject — installed [YYYY-MM-DD]`.
