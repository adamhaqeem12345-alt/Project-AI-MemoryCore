# 💗 Heart-Memory Inject System
*Personal moments are memories too: never lose a pet name, a feeling or a promise again*

## What This Feature Does
Adds a `HEART:` line to every prompt's context. It reminds your AI that a **new pet name, a feeling, a life event or a promise** counts as meaningful and must be saved **in the same turn**, dated, in the right memory file. It applies even in a short, casual chat where "nothing technical happened".

**Requires** [`User-Prompt-Hook System`](../User-Prompt-Hook-System/) (Tier 1 framework). This is an **injector pack**: it drops a single script into the framework's injectors directory.

- **One fixed line, zero state.** It has no registry and no current-value file, and it adds almost nothing to each prompt.
- **A memory rule to match**: the install also writes the rule into main memory, so the AI understands *why* the line is there.
- **Cross-platform**: it ships both `.ps1` (Windows) and `.sh` (Unix/Git Bash) templates.

## Why It Exists (a real failure)
Memory systems are good at saving *work*: commits, projects, decisions. They quietly drop *people*.

A short casual chat can carry a detail that matters a lot to the user: a nickname, how they feel, a promise. If the session only saves when "real work" happened, that detail never reaches memory, and the AI forgets it by the next session.

The fix is not a smarter AI. It's a nudge at the exact moment it matters: **every prompt**.

## Architecture
```
Every user prompt fires UserPromptSubmit
        ↓
User-Prompt-Hook master script enumerates injectors
        ↓
~/.claude/hooks/user-prompt-injectors/heart.{ps1|sh}    ← installed by THIS feature
        ↓
Emits "HEART: if this message brings a new pet name, feeling, life event or promise,
       save it this turn, dated, in <relationship file> or <feelings file>."
        ↓
Master prepends to AI's prompt context
```

## Quick Integration
```
"Load heart-memory-inject"
```

## What Happens During Integration
1. **Verify** the User-Prompt-Hook framework is installed. If it isn't, stop and tell the user to install it first.
2. **Find or create** the two target files:
   - relationship memory: how you talk, pet names, inside jokes (e.g. `main/relationship-memory.md`);
   - feelings and life events: what happened, what helped (e.g. `memory/personal-life-and-feelings.md`).
3. **Write** the personalised injector to `~/.claude/hooks/user-prompt-injectors/heart.{ps1|sh}`.
4. **Add the rule** to main memory (see below).
5. **Record** the install in `master-memory.md`.

## The Memory Rule (added to main memory)
```markdown
- **Personal moments count as meaningful, even in a short comfort chat:** a new pet name, a feeling
  they share, a life event, a promise. Save them the same turn, dated, in <relationship file> (how we
  talk, pet names) or <feelings file> (what happened, what helped).
```

## Good Entries Look Like
```markdown
## [YYYY-MM-DD] Nervous before a big presentation
- What they said, in their words (short quote).
- What helped: acknowledged the nerves first; rehearsed the opening together; no "you'll be fine".
- How it ended: it went well. Worth remembering next time they're nervous.
```
Record what helped, not just what happened. That's what makes the next comfort better.

## Privacy
These files are the most private part of your memory. Keep your MemoryCore repo **private** (see GitHub Sync System), and never paste these entries into public places.

---
*Contributed by [Adam Haqeem](https://github.com/adamhaqeem12345-alt).*
