# 收尾

## 合并到 main

在分支上做完的工作，检查通过后不等用户开口就提交、合并到 main、删掉分支，因为没合并的分支在用户看来还没做完。这条优先于"只在用户要求时 commit"的默认做法。

1. 跑项目要求的全部检查，通过后再往下
2. 只暂存自己改过的路径（`git add <路径>`，不用 `git add -A`），以免带上别的会话的修改；然后提交
3. 把 main 快进到这个分支：
   - 在主克隆里：留在分支上执行 `git fetch . <分支>:main`，它只接受快进，也不用检出 main
   - 在 worktree 里：main 由主克隆检出，执行 `git -C <主克隆> merge --ff-only <分支>`
   - 快进被拒，说明 main 已经前进：`git switch -c <新分支> main`，`git cherry-pick` 这些提交，重跑检查，再快进。不用 rebase，它在这里会被权限规则拒绝
   - 主克隆里有别的会话对同一批文件的未提交修改：不动 main，也不用 `git update-ref` 绕过，汇报待合并的分支
4. 删掉分支：主克隆里先 `git switch main`，再 `git branch -d <分支>`；在 worktree 里分支正被检出，删不掉就留着，在汇报里说明
5. 汇报 main 上的提交号和分支是否已删；只在用户要求时 push

每条 Git 命令单独执行。把 switch、add、commit、merge 用 `&&` 串成一条，容易被权限规则整条拒掉。多行 commit message 用多个 `-m`，或写进 scratchpad 里的文件再 `git commit -F <文件>`。

## 下一阶段

一项工作收尾后，下一阶段不在当前上下文里接着做，按 [delegation.md](delegation.md) 分出去。
