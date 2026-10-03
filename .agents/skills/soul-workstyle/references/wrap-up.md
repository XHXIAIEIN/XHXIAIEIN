# Finishing

## Merge into main

When work on a branch is finished and the checks pass, commit it, merge it into main and delete the branch. Do not
wait for the user to ask, because to the user an unmerged branch looks unfinished. This rule takes precedence over the
default "commit only when asked".

1. Run every check that the project requires. Continue only when all checks pass
2. Stage only the paths that you changed (`git add <path>`, not `git add -A`), so the changes of other sessions stay
   out. Commit
3. Fast-forward main to the branch:
   - In the main clone: stay on the branch and run `git fetch . <branch>:main`. This command accepts only a
     fast-forward and does not check out main
   - In a worktree: the main clone has main checked out. Run `git -C <main clone> merge --ff-only <branch>`
   - If Git refuses the fast-forward, main has moved. In a worktree that the app made, sync with
     `sync_with_base_branch`, resolve the conflicts, commit, then fast-forward. Otherwise, run
     `git switch -c <new branch> main`, `git cherry-pick` the commits, run the checks again, then fast-forward. Do not
     rebase, because a rebase rewrites the existing branch; a cherry-pick onto a new branch does not
   - If the main clone holds uncommitted changes of another session to the same files, leave main as it is. Do not
     bypass it with `git update-ref`. Report that the branch waits for a merge
4. Delete the branch. In the main clone, run `git switch main`, then `git branch -d <branch>`. In a worktree, the
   branch is checked out, so Git cannot delete it. Leave it and say so in the report
5. In the report, give the commit on main and say whether the branch was deleted. Push only when the user asks

When a step that can be checked on its own passes, commit it at once. Do not make one large commit at the end. Small
commits are easy to review, bisect and revert, and a session that stops early loses nothing.

Run each Git command on its own, not chained with `&&`, so that a failed step is obvious. For a commit message with
more than one line, use several `-m`, or a file in the scratchpad with `git commit -F <file>`.

## Next phase

When a piece of work is finished, do not continue the next phase in the current context. Hand it off as
[delegation.md](delegation.md) says.
