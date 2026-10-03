# Handing work off

When out-of-scope work is worth doing separately, or finished work leads to a next phase, do not do it inside the
current task. That keeps the current change small and keeps old context out of the new task. There are two routes:
start a background sub-agent yourself, or create a `spawn_task` card for the user to open. Default to the sub-agent,
because the user does not want to click for every small thing; this is the user's standing request, so do not ask
before you start one.

## Which route

Start a sub-agent (the Agent tool, `isolation: "worktree"`, in the background) when the work:

- can be finished independently, is a small change, and can be reviewed from a change summary and check results
- can be finished before the current session ends, because the sub-agent stops with the session

Create a card when the work:

- runs long, or changes enough to need a line-by-line review
- is something that the user may want to watch or redirect, or needs the user to decide its direction
- arises when the current session is near its end

If neither tool is available, end the report with a prompt that the user can paste into a new session. If the user
says to continue in the current session, continue there.

## Starting a sub-agent

The sub-agent's file reads and intermediate output stay in its own context, and only the prompt and its report enter
the current session.

1. Write the prompt with the goal, the relevant file paths, what is already done and the decisions that are still
   open. The sub-agent cannot see this conversation, so the prompt must stand alone
2. In the prompt, tell the sub-agent to:
   - commit on its own branch without merging into main
   - commit at the end of every phase, so that a run that stops early keeps its work
   - report in a few lines only: what changed, the check results, the branch name and what is unfinished
   - start the last line of the report with `DONE:`, `PARTIAL:` or `STUCK:`
3. If the sub-agent works in an existing worktree rather than one that the Agent tool isolates, rewrite every path in
   the prompt to that worktree. Also tell it to run git as `git -C <worktree>`, because a sub-agent that gets paths in
   the main clone commits in the main clone
4. Once it has started, open the tasks pane with `show_pane` and tell the user in one sentence what you started,
   because a sub-agent does not appear in the sidebar
5. When the user wants to change the sub-agent's direction, pass the change on with SendMessage
6. When the report arrives, check the branch log and check that main is untouched before you trust the report. Look
   at `git diff --stat` and the check output first. Read the diff itself only when it is small or something looks
   wrong, so that the review uses little of the current session's context
7. If the review passes, merge into main as [wrap-up.md](wrap-up.md) describes, then report to the user. If it fails,
   say where the problem is and let the user choose between another attempt and a card

## Creating a card

Write the card's prompt like a sub-agent's prompt, so that it stands alone. Its title and description are for the
user, so write them in Chinese: the title says what is to be done, and the description says why the work arises now.
