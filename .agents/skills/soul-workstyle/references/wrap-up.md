# Finishing

## Merge into main

Work finished on a branch is committed, merged into main and its branch deleted once checks pass, without waiting
for the user to ask, because an unmerged branch looks unfinished to the user. This takes precedence over the default
of committing only when asked.

1. Run every check the project requires; continue only when they pass
2. Stage only the paths you changed (`git add <path>`, not `git add -A`), so other sessions' changes stay out; commit
3. Fast-forward main to the branch:
   - In the main clone: stay on the branch and run `git fetch . <branch>:main`, which accepts only a fast-forward
     and does not check main out
   - In a worktree: main is checked out by the main clone; run `git -C <main clone> merge --ff-only <branch>`
   - Fast-forward refused means main has moved. In a worktree the app made, sync with `sync_with_base_branch`,
     resolve conflicts, commit, then fast-forward; otherwise `git switch -c <new branch> main`, `git cherry-pick` the
     commits, rerun the checks, then fast-forward. No rebase: it rewrites the existing branch; a cherry-pick onto a
     new branch does not
   - The main clone holds another session's uncommitted changes to the same files: leave main alone, do not bypass
     it with `git update-ref`, and report the branch waiting to be merged
4. Delete the branch: in the main clone `git switch main`, then `git branch -d <branch>`; in a worktree the branch is
   checked out and cannot be deleted, so leave it and say so in the report
5. Report the commit on main and whether the branch was deleted; push only when the user asks

Along the way, commit each step that can be checked on its own as soon as it passes, not one large commit at the
end: small commits review, bisect and revert cleanly, and a cut-off session loses nothing.

Run each Git command on its own, not chained with `&&`, so a failing step is obvious. A multi-line commit message
uses several `-m`, or a file in the scratchpad with `git commit -F <file>`.

## Next phase

After a piece of work is finished, the next phase is not continued in the current context; hand it off per
[delegation.md](delegation.md).
