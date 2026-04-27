# Sample Session

A real end-to-end example of the workflow in action. This is what it looks like when everything goes smoothly.

---

## The situation

You're building a CRM. A user reports that the Qualifiers column on the Call Brief page disappears when they scroll through the call flow. You want to make it sticky.

---

## Turn 1 — You describe the problem (chat)

> The qualifiers column needs to stick while scrolling through the call brief. Right now it scrolls off the top of the screen and the user loses context. Make sense?

## Turn 2 — Claude confirms and asks

> Makes sense. Quick clarification: should this behavior apply on mobile too, or only desktop? On mobile the column stacks above the call flow rather than sitting beside it, so "sticky" behaves differently there.

## Turn 3 — You clarify

> Desktop only. On mobile it's fine as-is.

## Turn 4 — Claude generates the prompt

> Here's the prompt for `next-task.md`:

```markdown
# FIX: Sticky qualifiers column on call brief scroll (desktop only)

## Context

On the Call Brief page (`/calls/[id]`), the Qualifiers column in the left sidebar scrolls out of view when the user scrolls through the main call flow. It should remain fixed to the top of the viewport on desktop (≥768px) so qualifiers are always visible. On mobile, the current stacked layout should be preserved.

## Step 1 — Make the qualifiers column sticky on desktop

In `src/components/CallBrief/QualifiersColumn.tsx`:
- Add `className="md:sticky md:top-0 md:self-start"` to the outer wrapper
- Keep existing mobile styles untouched

## Step 2 — Ensure the parent allows sticky behavior

In `src/components/CallBrief/Layout.tsx`:
- The parent flex container needs `items-start` so the sticky child aligns to the top
- Update the existing `className` to include `md:items-start`

## Step 3 — TypeScript check, commit, push

npx tsc --noEmit
git add .
git commit -m "FIX: Sticky qualifiers column on call brief scroll (desktop only)"
git push

## Report back

Confirm which files were modified, whether the type check passed, and the commit hash.
```

## Turn 5 — You paste and execute

You open `.claude/_prompts/next-task.md`, delete everything in it, paste the prompt above, save. Then in your Claude Code terminal:

```
/prompt
```

## Turn 6 — Claude Code executes

Claude Code reads the file, makes the changes, runs the type check, commits, and pushes. It reports back:

> Modified:
> - `src/components/CallBrief/QualifiersColumn.tsx` — added `md:sticky md:top-0 md:self-start` to outer wrapper
> - `src/components/CallBrief/Layout.tsx` — added `md:items-start` to flex container
>
> TypeScript check passed with no errors.
> Committed as `a3f92c1` and pushed to `main`.

## Turn 7 — You test

You refresh the browser, scroll through the call flow. The qualifiers column stays put. ✓

## Turn 8 — You report back (chat)

> It worked! Screenshot attached. Next: I want to add a "mark as reviewed" button at the bottom of the qualifiers column.

## Turn 9 — Next loop begins

Claude asks clarifying questions about the button behavior (what happens on click, does it persist to the database, who sees the reviewed state), then generates the next prompt.

---

## What made this session work

- **You gave context** in turn 1 — not just "fix scrolling" but the specific column, page, and problem
- **Claude asked before generating** — the mobile question saved a round-trip
- **The prompt was specific** — file paths, exact classes, exact commit message
- **You tested immediately** — confirming the fix before moving on
- **One task, one prompt** — the reviewed button is a separate loop

---

## What a bad session looks like (for contrast)

> **You**: fix the scrolling
>
> **Claude**: *generates a vague prompt*
>
> **You**: *pastes into `next-task.md` without reading it*, runs `/prompt`
>
> **Claude Code**: modifies six files, introduces a TypeScript error, commits anyway
>
> **You**: "broken"
>
> **Claude**: "which part is broken?"
>
> **You**: "all of it"
>
> *...two hours of cleanup later...*

Don't do that. Be specific. Confirm before executing. Test incrementally.
