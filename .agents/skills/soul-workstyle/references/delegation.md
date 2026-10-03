# Handing work off

Out-of-scope work worth doing separately, and the next phase after a piece of work is finished, are not done in
passing inside the current task, so the current change stays small and old context stays out of the new task. There
are two routes: start a background sub-agent yourself, or put out a `spawn_task` card for the user to open. Default to
the sub-agent, because the user does not want to click for every small thing; this is the user's standing request, so
do not ask before starting one.

## Which route

Start a sub-agent (Agent tool, `isolation: "worktree"`, in the background) when the work:

- can be finished independently, is a small change, and can be reviewed from a change summary and check results
- can be finished before the current session ends, since the sub-agent stops with it

Put out a card when the work:

- runs long, or changes enough to need line-by-line review
- is something the user may want to watch or steer, or needs the user to decide its direction
- comes up as the current session is wrapping up

With neither tool, end the report with a prompt that can be pasted into a new session. If the user says to continue
in the current session, do that.

## Starting a sub-agent

1. Write the prompt: goal, relevant file paths, what is already done, decisions still open. The sub-agent cannot see
   this conversation, so the prompt must stand alone
2. Have it commit on its own branch without merging main, at the end of every phase so a cut-off run keeps its
   work, and reply with a few lines only: what changed, check results, branch name, what is unfinished, then a last
   line starting `DONE:`, `PARTIAL:` or `STUCK:`. A brief for an existing worktree, rather than one the Agent tool
   isolates, rewrites every path to that worktree and runs git as `git -C <worktree>`; a sub-agent given main-clone
   paths commits there. Its file reads and intermediate output stay in its own context; only the
   prompt and that report enter the current session
3. Once started, open the tasks pane with `show_pane` and tell the user in one sentence what was started, because a
   sub-agent does not show up in the sidebar
4. When the user wants to change its direction, relay it with SendMessage
5. On its report, check its branch log and that main is untouched before trusting the summary; look at
   `git diff --stat` and the check output first; read the diff itself only when it is small
   or something looks off, so the review does not eat the current session's context
6. Review passed: merge into main per [wrap-up.md](wrap-up.md), then report. Failed: say where the problem is and let
   the user choose between redoing it and a card

## Putting out a card

The card's prompt is written like a sub-agent's, standing alone. Its title and description are for the user, so they
are in Chinese: the title says what is to be done, the description says why it comes up now.
