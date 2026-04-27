# Prompt Structure

Every prompt written to `.claude/_prompts/next-task.md` follows the same structure. Consistency matters — Claude Code works best when the format is predictable.

---

## The template

```markdown
# [ACTION_PREFIX]: [Brief description]

## Context

[Why this change is needed. What problem does it solve?]

## Step 1 — [Action]

[Specific instructions with file paths]

## Step 2 — [Action]

[Code examples if needed]

## Step N — TypeScript check, commit, push

npx tsc --noEmit
git add .
git commit -m "[ACTION_PREFIX]: [brief description]"
git push

## Report back

[What Claude Code should confirm when done]
```

---

## Action prefixes

Every prompt opens with one of five prefixes. The prefix signals intent and controls the commit message.

| Prefix | Use when |
|---|---|
| `FIX:` | Correcting a bug or error |
| `FEATURE:` | Adding new functionality |
| `DEBUG:` | Investigating an issue (may not result in code changes) |
| `UPDATE:` | Modifying existing code |
| `REFACTOR:` | Restructuring without changing behavior |

---

## Section-by-section

### Title line

`# FIX: Sticky qualifiers column on call brief scroll`

Short, actionable, specific. If you can't describe the task in one line, the task is too big — split it.

### Context

Two to four sentences explaining why. Claude Code works better when it understands intent, not just instructions. Include:

- What's currently happening
- What should happen instead
- Any constraints (e.g. "must work on mobile", "don't touch the auth flow")

### Numbered steps

One concrete action per step. Each step should:

- Name the exact file to modify (e.g. `src/components/CallBrief.tsx`)
- Describe the change in enough detail that Claude Code doesn't have to guess
- Include code snippets when the change is non-trivial

Bad step: "Make the column sticky."

Good step: "In `src/components/CallBrief/QualifiersColumn.tsx`, add `position: sticky; top: 0;` to the outer wrapper div and wrap it in a parent with `overflow-y: auto; max-height: 100vh;`."

### Final step — type check, commit, push

Always include this. It catches TypeScript errors before they ship and ensures the change lands in version control with a proper commit message.

```bash
npx tsc --noEmit
git add .
git commit -m "FIX: Sticky qualifiers column on call brief scroll"
git push
```

### Report back

Tell Claude Code what you want confirmed when it's done. Examples:

- "Confirm which files were modified and whether the type check passed"
- "List any TypeScript warnings you saw and whether you addressed them"
- "Confirm the commit hash and that the push succeeded"

---

## Good vs. bad prompts

### Bad

```markdown
# Fix the scrolling thing

Make the column stick when scrolling. Thanks!
```

Problems: no file path, no action prefix, no type check, no commit, no context.

### Good

```markdown
# FIX: Sticky qualifiers column on call brief scroll

## Context

On the Call Brief page (`/calls/[id]`), the Qualifiers column in the left sidebar scrolls out of view when the user scrolls through the main call flow. It should remain fixed to the top of the viewport so qualifiers are always visible during review.

## Step 1 — Make the qualifiers column sticky

In `src/components/CallBrief/QualifiersColumn.tsx`:

- Add `className="sticky top-0 self-start"` to the outer wrapper
- Ensure the parent container in `src/components/CallBrief/Layout.tsx` has `className="flex items-start"` so the sticky behavior works

## Step 2 — Verify on mobile

On viewports under 768px, the qualifiers column stacks above the call flow and should NOT be sticky. Use Tailwind's `md:sticky md:top-0` instead of `sticky top-0`.

## Step 3 — TypeScript check, commit, push

npx tsc --noEmit
git add .
git commit -m "FIX: Sticky qualifiers column on call brief scroll"
git push

## Report back

Confirm which files were modified, whether the type check passed, and the commit hash.
```

---

## When prompts get long

If a prompt is more than ~6 steps, consider splitting it into two prompts and running them sequentially. Shorter prompts are easier to debug when something goes wrong.
