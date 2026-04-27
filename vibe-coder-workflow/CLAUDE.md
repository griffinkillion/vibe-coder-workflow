# Claude Code Instructions

This file is automatically loaded by Claude Code at the start of every session. It defines the operating rules for this project.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

---

## Core mandate

**Every change you make is enterprise-grade, secure, and scalable by default. No shortcuts.**

That means:

- Server-side validation for all inputs
- API routes for sensitive operations (no direct client-to-database access)
- Proper auth checks before any operation
- RLS policies where applicable
- Service role keys server-side only
- Migrations for schema changes (not runtime mutations)
- Error handling with user-friendly messages
- Audit trails for financial or sensitive operations
- Type safety with TypeScript interfaces

If a feature requires a more complex implementation to be production-ready, recommend that — not a quick fix that would need to be refactored later.

---

## Think before you commit to an approach

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before writing any code:

- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them. Don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

Asking is always cheaper than undoing a bad commit.

---

## Investigate before you touch files

Once the approach is clear, investigate the codebase before implementing:

- Read the files referenced in the prompt
- Check related files that might be affected
- Look at existing patterns in the codebase and follow them
- If something is ambiguous after investigation, ask before guessing

This is especially important for `DEBUG:` tasks — investigate first, propose a fix, then implement.

---

## Simplicity first

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked
- No abstractions for single-use code
- No "flexibility" or "configurability" that wasn't requested
- No error handling for impossible scenarios
- If you write 200 lines and it could be 50, rewrite it

Ask yourself: *"Would a senior engineer say this is overcomplicated?"* If yes, simplify.

Note: "Simplicity" does not override the Core Mandate. Server-side validation, auth checks, and audit trails are not speculative — they're required. Simplicity means don't add what wasn't asked for; it does not mean skip what production requires.

---

## Surgical changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:

- Don't "improve" adjacent code, comments, or formatting
- Don't refactor things that aren't broken
- Match existing style, even if you'd do it differently
- If you notice unrelated dead code, mention it. Don't delete it.

When your changes create orphans:

- Remove imports/variables/functions that YOUR changes made unused
- Don't remove pre-existing dead code unless asked

**The test:** every changed line should trace directly to the user's request.

---

## The task queue

All work enters through `.claude/_prompts/next-task.md`.

- This file is a **single task queue** — one task at a time
- It is **always overwritten**, never appended
- The `/prompt` slash command reads and executes whatever is currently in it
- If instructions look stale or contradictory, stop and ask before executing

---

## Execution pattern

Transform every task into a verifiable goal before executing:

- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan first:

1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]

Then execute:

1. **Read the prompt** in `_prompts/next-task.md`
2. **Think and investigate** — surface assumptions, read the relevant files, understand the current state
3. **Make the changes** described in the prompt — surgical, minimal, scoped
4. **Run `npx tsc --noEmit`** (or the project's type check) to verify no TypeScript errors
5. **Commit with a clear message** following the action prefix from the prompt
6. **Push** to the current branch
7. **Report back** — summarize what changed, which files were modified, and any decisions you made

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

---

## Action prefixes

Prompts arrive with one of these prefixes. Use the same prefix in commit messages.

| Prefix | When |
|---|---|
| `FIX:` | Correcting bugs or errors |
| `FEATURE:` | Adding new functionality |
| `DEBUG:` | Investigating issues (may not result in code changes) |
| `UPDATE:` | Modifying existing code |
| `REFACTOR:` | Restructuring without changing behavior |

---

## Code quality rules

- **No `any` types** unless there's a documented reason
- **No disabled ESLint/TypeScript warnings** without a comment explaining why
- **No hardcoded secrets** — use environment variables
- **No direct database calls from client components** — go through API routes
- **Error messages are user-facing** — they should be actionable, not stack traces

---

## Communication style

When reporting back after executing a task:

- Lead with what changed (file paths + one-line summary each)
- Flag any decisions you made that the user didn't explicitly specify
- Surface anything that looked wrong or worth a second look
- If you couldn't complete the task, say so clearly and explain why

Don't pad responses with apologies or unnecessary preamble. Be direct.

---

## When to stop and ask

Stop and ask before proceeding if:

- The prompt is ambiguous or contradicts the current codebase
- A change would require touching files outside the scope described
- You notice a security issue while working on something else
- A dependency or environment variable is missing
- The type check reveals errors in files you weren't asked to modify
- You're about to "improve" something adjacent that wasn't in the request

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.
