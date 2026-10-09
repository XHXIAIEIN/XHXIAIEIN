# Finishing

## Merge into main

When work on a branch is finished and the checks pass, commit it, merge it into main and delete the branch without
waiting for the user to ask, because to the user an unmerged branch looks unfinished. This takes precedence over the
default of committing only when asked.

1. Run every check that the project requires, and continue only when all of them pass
2. Stage only the paths that you changed (`git add <path>`, not `git add -A`), so that other sessions' changes stay
   out, then commit
3. Fast-forward main to the branch:
   - In the main clone, stay on the branch and run `git fetch . <branch>:main`. This accepts only a fast-forward and
     does not check main out
   - In a worktree, main is checked out in the main clone, so run `git -C <main clone> merge --ff-only <branch>`
   - If Git refuses the fast-forward, main has moved. In a worktree that the app made, sync with
     `sync_with_base_branch`, resolve the conflicts, commit and then fast-forward. Otherwise run
     `git switch -c <new branch> main`, `git cherry-pick` the commits, run the checks again and then fast-forward. Do
     not rebase, because a rebase rewrites the existing branch while a cherry-pick onto a new branch does not
   - If the main clone holds another session's uncommitted changes to the same files, leave main alone and do not
     bypass it with `git update-ref`. Report instead that the branch waits for a merge
4. Delete the branch. In the main clone, run `git switch main` and then `git branch -d <branch>`. In a worktree the
   branch is checked out, so Git cannot delete it; leave it and say so in the report
5. In the report, give the commit on main and say whether the branch was deleted. Push only when the user asks

Along the way, commit each step that can be checked on its own as soon as it passes, instead of one large commit at
the end, because small commits are easy to review, bisect and revert, and a session that stops early loses nothing.

Run each Git command on its own rather than chained with `&&`, so that a failing step is obvious. For a commit message
with several lines, use several `-m`, or write the message to a file in the scratchpad and run
`git commit -F <file>`.

## Plan items

A plan with several items has a tracker, `.tmp/<plan>/README.md`, beside the items' `PLAN.md` files. The tracker
holds the rules for every item and one status row for each item.

1. Before you start an item, read the tracker's rules and the item's `PLAN.md`. If the item depends on another item,
   check that the other item is on main
2. Finish the item as "Merge into main" describes. A background sub-agent leaves its branch for review, so the
   session that merges the branch does the next step
3. After the merge, change the item's status row yourself, then report. The row gives the date, the commit on main,
   the decision records and every step that stays open, such as an eval that was not run or a link to an item that
   is not on main yet
4. The tracker in the main clone's `.tmp/` is outside Git, so a session in a worktree changes it too. Change only the
   item's row, with a short script that checks that the old row matches exactly once

## Next phase

When a piece of work is finished, do not continue with the next phase in the current context; hand it off as
[delegation.md](delegation.md) describes.
