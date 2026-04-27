# Vibe Coder Workflow

A two-layer system for building production-grade software with Claude — using Claude (chat) for strategy and Claude Code for execution.

Built for "vibe coders" who want to move fast **and** ship enterprise-grade code. No quick fixes, no technical debt, no shortcuts.

---

## Quickstart (5 minutes)

1. **Install Claude Code**: Follow the [official install guide](https://docs.claude.com/en/docs/claude-code/overview)
2. **Copy `.claude/` into your project root**
3. **Copy `CLAUDE.md` into your project root**
4. **Start a session** — describe what you want to build in Claude (chat)
5. **Paste the generated prompt** into `.claude/_prompts/next-task.md`
6. **Run `/prompt`** in your Claude Code terminal

That's the loop. Keep going until it ships.

---

## Why this works

Most AI coding workflows collapse into one of two failure modes:

- **Too loose** — you chat with Claude, get code, paste it around, lose context, end up with a frankenstein codebase
- **Too rigid** — you write detailed specs for every change, lose the speed advantage that made you reach for AI in the first place

This workflow splits the work into two layers, each optimized for what it does best.

### Layer 1 — Chat (strategy)

You talk to Claude in natural language. Share screenshots. Paste error logs. Ask "is that enterprise-grade?" when something feels off. This is where decisions get made.

### Layer 2 — Claude Code (execution)

Claude generates a structured prompt. You paste it into `_prompts/next-task.md`. You run `/prompt`. Claude Code executes against your actual codebase, with file paths, type checks, and git commits baked in.

The handoff between the two layers is the single file `.claude/_prompts/next-task.md`. Claude writes it, you paste it, Claude Code runs it. One task queue, always current, always overwritten.

---

## The 6-step loop

1. **Describe** the feature or bug in chat ("the qualifiers column needs to stick while scrolling")
2. **Confirm** — Claude asks clarifying questions, proposes an approach
3. **Generate** — Claude writes a structured prompt for `next-task.md`
4. **Paste & run** — overwrite `next-task.md`, run `/prompt` in Claude Code
5. **Test** — verify the change works in your browser / terminal / wherever
6. **Report back** — "it worked!" or "still broken, here's the error" → loop to step 1

---

## What's in this repo

```
vibe-coder-workflow/
├── README.md                          ← you are here
├── CLAUDE.md                          ← auto-loaded rules for Claude Code
├── .claude/
│   ├── commands/
│   │   └── prompt.md                  ← the /prompt slash command
│   └── _prompts/
│       ├── next-task.md               ← single task queue (overwrite, never append)
│       └── state-summary.md           ← session handoff prompt
└── docs/
    ├── communication-guide.md         ← how to talk to Claude effectively
    ├── prompt-structure.md            ← format for next-task.md prompts
    └── examples/
        └── sample-session.md          ← real end-to-end example
```

---

## Core principles

**Enterprise-grade by default.** Every prompt Claude generates assumes server-side validation, API routes for sensitive operations, proper auth checks, audit trails, error handling, and TypeScript safety. If a feature needs a more complex implementation to be production-ready, that's what gets recommended — no quick fixes that would need to be refactored later.

**Always overwrite `next-task.md`.** Never append. The file is a single task queue, not a log. Stale instructions cause Claude Code to execute the wrong thing.

**Test incrementally.** Verify each change works before moving to the next. Screenshots and error logs are your best friends.

**Confirm before generating.** When in doubt, ask "make sense?" before Claude writes the prompt. Catching misunderstandings in chat is free. Catching them after `/prompt` runs costs a commit.

---

## When to use which tool

| Situation | Use |
|---|---|
| Exploring ideas, debugging logic, making decisions | Claude (chat) |
| Writing/editing code, running tests, committing changes | Claude Code |
| Long session handoff / context dump | `_prompts/state-summary.md` |
| New feature or bug fix | `_prompts/next-task.md` |

---

## Contributing

Got a prompt pattern that works well? A tweak to the workflow? Open a PR. This is meant to evolve.

## License

MIT — use it, fork it, remix it.
