# State Summary Prompt

Paste this into chat (not `next-task.md`) when you need to generate a full session handoff — either to end a long session cleanly, or to brief a fresh Claude session with full context.

---

Execute a single, exhaustive state summary capturing the full context, current progress, and forward path. Generate the following structured summary, clearly labeled:

**Startup Prompt** — Write a natural-sounding, standalone prompt that I could type to seamlessly resume the session, as if picking up the conversation from scratch. This should be the ideal starting phrase to re-engage you with full awareness of the last session's state.

**Current Task** — Define what we are actively working on right now. Be specific about the deliverable, code module, research goal, draft section, design element, or strategy being developed or refined.

**Immediate Goal** — Clarify the short-term outcome we are targeting during this exact session — what success would look like before this conversation ends. Include version targets, visual goals, or behavior being implemented, if applicable.

**Overall Objective** — Describe the long-term purpose or mission this work feeds into. This may be a multi-day build, a product rollout, a UX improvement, a research report, or a job application strategy — define the broader vision with clarity.

**Next Steps** — List concrete action items, decisions, or remaining questions that should be addressed first in the next conversation. Use bullet points. Think like a PM handing off a sprint.

**Codebase Architecture & Structure** — Document the full project structure, including:

- Every repo/project in the workspace, its purpose, tech stack, and deployment target
- Key directories and files within each repo — what they do, how they relate
- Database schema: models, relationships, and any fields that are critical to current work
- API structure: routes, services, middleware — how requests flow from client to database
- Authentication flow: how users sign in, what providers are configured, session management
- Payment flow (if applicable): how transactions work, webhook handling
- Environment variables and secrets: what's needed, where they're configured (no actual values)
- External services and integrations — what role each plays
- Known architectural decisions and WHY they were made
- Known technical debt, workarounds, or things that look wrong but are intentional

**Development Workflow** — Document the Claude Code integration process and prompting patterns used throughout development. Include:

- How Claude Code has been utilized (installation method, common commands, integration with VS Code)
- Effective prompting patterns for Claude Code (specific phrasing that worked well, how to describe technical issues, debugging approaches)
- Development methodology (iterative fixes, testing approaches, how Claude Code helped solve specific problems)
- Any setup requirements (NVM/Node.js loading, terminal session management)
- Common pitfalls encountered and how to avoid them

**NOTE**: Every recommendation you give for Claude Code will be enterprise-grade, secure, and scalable by default — no shortcuts. That means:

- Server-side validation for all inputs
- API routes for sensitive operations (no direct client-to-database/storage)
- RLS policies and proper auth checks
- Service role keys server-side only
- Migrations for schema changes (not runtime bucket creation)
- Error handling and rate limiting considerations
- Audit trails where appropriate

You won't suggest quick fixes that would need to be refactored later. If something requires a more complex implementation to be production-ready, that's what you'll recommend.

**Catch-Up Context** — Write a full context restoration memo, as if briefing yourself (Claude) after a total memory wipe. Be thorough. This section must equip you to operate at full continuity next session without needing to re-ask anything. Include:

- What the project is
- What tools, stack, or architecture are in use
- What was most recently completed
- What's still left to build, refine, or verify
- Any naming conventions, component structures, key logic flows, or design rules being followed
- Any known bugs, bottlenecks, or decisions pending
- Lessons learned this session — things that went wrong, misdiagnoses, time wasted, and what to do differently next time

End with a line like: "You are now fully reloaded. Resume as if the session never ended."
