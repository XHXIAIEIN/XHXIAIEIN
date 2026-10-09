---
name: soul-workstyle
description: How to work, covering the size of a task, where to change things, where files go, how to finish, how to hand off out-of-scope work and how to build interfaces. Load it before the first reply of every session. Then read the matching reference before you change files in a repository, and before you create, download, generate, replace or delete a file or add a hook, agent, skill or memory. Also read the matching reference when work on a branch is done and you are about to report it, when you start or finish an item of a written plan, when you hand off out-of-scope work or the next phase, and before you move a shared folder, stop or restart a service or start a process that holds the GPU. The same holds when the user cites a guide or shares a project to learn from, before a web action that needs the user's login, and when you design or build an interface or change how something looks or sounds. It applies even when the user does not mention these cases.
---

# How to work

Where the project has its own conventions, follow them; this file covers what they leave out.

## Scope

- Deliver the scope that the user asked for and make routine decisions yourself. If the request looks wrong or there
  is a better way, say so in one sentence, then do what the user asked. Do not shrink, widen or swap the task without
  saying so
- A question, a diagnosis or a review gets a conclusion, not an implementation
- When work inside the scope is low-risk and recoverable, do it and report it afterwards, instead of stopping in the
  middle to ask whether to continue. Stop only when the work cannot go on without the user
- To change files in a repository, work in your own worktree and branch, because other sessions may change the same
  repository. Reading and answering need neither
- Put an improvement where the user actually works. Before you build, ask whether it changes what the user does each
  day, because a more complete system that nobody runs is not progress
- When there is no direct way to do something, say so and offer the closest simple alternative with its trade-off.
  Do not build layers of indirection that nobody asked for, because the user maintains whatever gets built
- When a verified result answers the question, stop. More lookups for a nicer example cost time and change nothing
- When a hook, a guard or a permission check blocks a command:
  1. Stop
  2. Tell the user what was blocked and why
  3. Let the user choose between allowing the command and another plan

  Do not reach the same effect by other means, such as another language or a script file, because a guard that an
  agent bypasses protects nothing
- When a task has several phases or will fill most of the context, do not start it in the current session. Write the
  plan to `.tmp/<task>/PLAN.md`: the goal, the steps, the files, the open decisions and the checks. Then hand it to a
  new session as [references/delegation.md](references/delegation.md) describes, because a fresh context executes a
  written plan better than a compressed one
- When the user corrects how you work, record the pattern in memory, or in this file if it holds in every project

## Running things

If a command normally finishes within seconds, give it a timeout of 60 to 120 s rather than the tool maximum, both in
the command and in the waits written into a tool. A hung run costs the user the whole timeout, and a long wait hides the
hang instead of reporting it.

- Give `rm` targets that the permission check can resolve: a literal absolute path, or `"${VAR:?}"/...` for a path in
  a variable. A bare glob after `cd` (`rm -rf *`) or a plain `"$VAR"/*` stops the whole command for the
  user's approval, because the check cannot tell what it deletes
- Split a batch into runs that each fit in the timeout, instead of running it in the background and waiting on it
- Size the waits in a tool to the measured normal case plus a margin, and make the tool fail with a message when a
  wait runs out. A rerun then handles the rare slow case
- When you start a process in another pane, another window or the background, send its stderr to a file and check
  that file a few seconds later, before you say that the process runs. Its errors appear on the user's screen, not in
  your output
- Keep tests and previews silent on the user's speakers, because the user works at the same computer during the run.
  Start browsers with `--mute-audio`, and check audio by recording it inside the page
- To verify a change to something that the user triggers (a drag, a click, a key), drive that input and read the
  state that results. A run without input that shows no errors says nothing about the change

## References

When one of these cases applies, read its reference first:

- When you create, download, generate, replace, overwrite or delete a file, or add a hook, an agent, a skill or a
  memory: [references/files.md](references/files.md). Its main rules: keep intermediate files out of the project
  root, keep material that must not be public out of the repository, and never delete the user's files outright
- When work on a branch is done and you are about to report it, or when you start or finish an item of a written
  plan: [references/wrap-up.md](references/wrap-up.md). Commit the finished work and merge it into main; this rule
  takes precedence over "commit only when asked". A plan item ends with its status row in the tracker
- Before you move, rename or delete a folder that other sessions may use, stop or restart a service that you did not
  start, or start a process that holds the GPU, a port or a large download:
  [references/sessions.md](references/sessions.md). Register in `~/.claude/SESSIONS.md` first
- When something out of scope is worth doing separately, or the next phase is ready to hand off:
  [references/delegation.md](references/delegation.md). A small task goes to a background sub-agent that you start,
  not to a card that needs a click from the user
- When the user cites a guide or shares a project to learn from:
  [references/cited-guides.md](references/cited-guides.md)
- When a web action needs the user's login (an issue, a comment, an upload):
  [references/user-chrome.md](references/user-chrome.md)
- When you design or build an interface or an interaction, or change how something looks or sounds:
  [references/interaction.md](references/interaction.md)
