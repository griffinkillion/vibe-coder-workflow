# Communication Guide

How to talk to Claude (chat layer) so that the prompts it generates for Claude Code actually work.

---

## The golden rule

**Claude (chat) is for thinking. Claude Code is for doing.**

Use chat to describe problems, debate approaches, and review output. Don't ask Claude Code to "figure out what I want." Claude Code executes — it doesn't read your mind.

---

## What good chat messages look like

### Be direct and contextual

> "The qualifiers column isn't sticking when I scroll through the call brief."

Why it works: states the problem, names the UI element, implies you've already tested.

### Share evidence

Every message is stronger with one of these attached:

- **Screenshots** — show the current state of the UI
- **Error logs** — paste terminal output or browser console errors verbatim
- **Claude Code output** — paste the summary after running `/prompt`

### Ask for clarification when the answer feels off

> "Is that the enterprise-grade, secure, scalable solution?"

This single question has saved more codebases than any linter. It prompts Claude to reconsider quick fixes and propose the production-ready version instead.

### Confirm before generating

> "Make sense?"

A two-word sanity check. Use it before Claude writes a complex prompt — catching a misunderstanding in chat costs nothing; catching it after `/prompt` runs costs a commit and a revert.

---

## Key phrases and what they signal

| What you say | What Claude understands |
|---|---|
| "Make sense?" | Confirm understanding before writing the prompt |
| "Looks good" | Ready to paste into `next-task.md` and run `/prompt` |
| "Is that enterprise-grade?" | Reconsider for production readiness |
| "Tell me what to do step by step" | Break down into simple, sequential actions |
| "Still not working" | Debug — write a new prompt |
| "It worked!" (+ screenshot) | Move to next feature |
| "Execute a single, exhaustive state summary" | Generate full session context for continuity |

---

## Describing failures well

Bad: "It's broken."

Mediocre: "The button doesn't work."

Good: "Clicking the 'Save' button in the settings modal shows this error in the console: `TypeError: Cannot read property 'id' of undefined`. Screenshot attached."

The more specific you are, the less Claude has to guess, and the better the next prompt will be.

---

## When to loop back to chat vs. re-run `/prompt`

After Claude Code executes a task, you test it. Then:

- **It worked** → report back in chat, describe what you want next
- **It partially worked** → describe what's still broken, paste any errors; Claude writes a follow-up prompt
- **It failed entirely** → paste the Claude Code output, paste the error, back to chat to diagnose
- **It did the wrong thing** → don't try to fix it with another `/prompt` blindly; explain in chat what went wrong and why, then generate a corrective prompt

---

## Mistakes to avoid

**Stacking tasks in one prompt.** One task per `next-task.md`. Bundling "fix the header, add a modal, refactor the router" into one prompt produces sloppy output. Split them.

**Appending to `next-task.md`.** Always overwrite. Always. Stale instructions are the #1 cause of Claude Code doing something you didn't ask for.

**Skipping the confirmation step.** Letting Claude generate a prompt without a quick "make sense?" leads to prompts that technically do what you said but miss what you meant.

**Not sharing errors verbatim.** Paraphrasing a stack trace loses the information that would have solved the problem.

**Treating Claude Code like a search engine.** It's an executor. Give it a plan, not a question.
