Read and execute the instructions in `.claude/_prompts/next-task.md`. Save output to .claude/output.md.

Follow the execution pattern defined in `CLAUDE.md`:

1. Investigate before implementing — read the referenced files first
2. Make the changes described
3. Run the project's TypeScript check (`npx tsc --noEmit` or equivalent)
4. Ask user if they want to manually test before committing and pushing.
5. Commit with a message using the same action prefix as the prompt. 
6. Push to the current branch
7. Report back with a summary of what changed and any decisions made
