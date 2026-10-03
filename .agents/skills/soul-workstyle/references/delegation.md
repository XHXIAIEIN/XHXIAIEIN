# Handing work off

If out-of-scope work is worth doing separately, or if the next phase follows finished work, do not do it inside the
current task. The current change then stays small, and old context stays out of the new task. There are two routes:

- Start a background sub-agent yourself
- Create a `spawn_task` card that the user opens

Use the sub-agent by default, because the user does not want to click for every small thing. This is a standing
request of the user, so start the sub-agent without asking first.

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

The file reads and intermediate output of the sub-agent stay in its own context. Only the prompt and the report enter
the current session.

1. Write the prompt: the goal, the relevant file paths, what is already done, and the decisions that are still open.
   The sub-agent cannot see this conversation, so the prompt must stand alone
2. Tell the sub-agent to commit on its own branch and not to merge into main. Tell it to commit at the end of every
   phase, so a run that stops early keeps its work. Tell it to report in a few lines only: what changed, the check
   results, the branch name and what is unfinished. Tell it to start the last line with `DONE:`, `PARTIAL:` or
   `STUCK:`
3. If the sub-agent works in an existing worktree, not in one that the Agent tool isolates, change every path in the
   prompt to that worktree. Tell the sub-agent to run git as `git -C <worktree>`. A sub-agent that gets paths in the
   main clone commits in the main clone
4. After the start, open the tasks pane with `show_pane`. Tell the user in one sentence what you started, because a
   sub-agent does not appear in the sidebar
5. If the user wants to change the direction of the sub-agent, send the change with SendMessage
6. When the report comes, check the branch log, and check that main is unchanged, before you trust the report. Look
   at `git diff --stat` and the check output first. Read the diff itself only when it is small or something looks
   wrong, so the review uses little of the context of the current session
7. If the review passes, merge into main as [wrap-up.md](wrap-up.md) says, then report. If it fails, say where the
   problem is, and let the user choose between a new attempt and a card

## Creating a card

Write the prompt of the card like the prompt of a sub-agent: it must stand alone. The title and the description are
for the user, so write them in Chinese. The title says what to do. The description says why the work arises now.
