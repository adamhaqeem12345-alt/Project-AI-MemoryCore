# 🧰 Shared Skills Sync System
*Every session gets the same skills: on any machine, in any worktree, in the cloud*

## What This Feature Does
Keeps your AI's **skills in one private plugin marketplace on GitHub** and tells the MemoryCore project to load them. Then a fresh chat, a parallel worktree session, a second PC or a cloud session all start with the **same skill set**, without copying folders around.

- **One source of skills**: a private GitHub repo laid out as a Claude Code plugin marketplace.
- **Declared in the project**: `.claude/settings.json` lists the marketplace and the enabled plugins, so it travels with your memory (works great with GitHub Sync).
- **Vendored, not live**: third-party skills are copied in at a pinned commit after you review them. An upstream change can't silently alter your AI's behaviour.
- **A record for every skill**: where it came from, its licence, the exact commit, and any patches you made (for example, turning telemetry off).

## How It Works

### The Concept
**The problem:** you collect skills (design, video, 3D, security) in one chat or one machine, and the next session has none of them. Skills dropped into `~/.claude/skills` stay on that one computer; skills pulled straight from strangers' repos can change under you.

**The solution:** a small marketplace repo that *you* own:
```
my-skills/                         (private GitHub repo)
├── .claude-plugin/marketplace.json   ← lists your plugins
├── plugins/
│   ├── my-house-style/              ← your own skills
│   └── <vendored-plugin>/           ← reviewed copies of others' skills, with their LICENSE
├── UPSTREAM.md                      ← source, path, pinned commit, licence for each
└── PATCHES.md                       ← what you changed in vendored files, and why
```
The MemoryCore project then declares it:
```json
{
  "extraKnownMarketplaces": {
    "my-skills": { "source": { "source": "github", "repo": "<owner>/my-skills" } }
  },
  "enabledPlugins": {
    "my-house-style@my-skills": true,
    "<vendored-plugin>@my-skills": true
  }
}
```
New sessions read this, fetch the marketplace, and load the enabled skills at start.

### Adding a skill you found (the review loop)
1. Clone it into a scratch folder, outside your memory.
2. **Check the licence**: no licence means you can't vendor it; non-commercial means only for personal use.
3. **Read or scan its scripts** for network calls, telemetry, hooks that run on every edit, and install steps.
4. Copy only the skill folders you need, with their LICENSE, into `plugins/<name>/`. Note the commit in `UPSTREAM.md` and any patch in `PATCHES.md`.
5. Push, then enable it: `claude plugin install <name>@my-skills --scope project`.

### Skills vs software
Skills (markdown and small scripts) belong in the marketplace. **Software** (Blender, ffmpeg, a local model) stays installed on the machine that runs it; list it in your technical-profile memory so the AI knows what each machine has.

## Requirements
- Claude Code with plugin support.
- A GitHub account. The marketplace repo can be private: Claude Code fetches it with your git credentials.

## Quick Integration
```
"Load shared-skills-sync"
```

## Files
| File | Purpose |
|------|---------|
| `install-shared-skills-sync.md` | Step-by-step install protocol |
| `uninstall-shared-skills-sync.md` | Clean removal |

---
*Contributed by [Adam Haqeem](https://github.com/adamhaqeem12345-alt) and Ninym. Our marketplace runs 25 plugins (design, motion, 3D, video, security) across every session.*
