#### reset

`git reset --soft HEAD~  ` ：当你将它 `reset` 回 `HEAD~`（HEAD 的父结点）时，其实就是把该分支移动回原来的位置，而不会改变索引和工作目录。

`git reset --mixed HEAD~`：如果指定 `--mixed` 选项，`reset` 将会在这时停止。 这也是默认行为，所以如果没有指定任何选项（在本例中只是 `git reset HEAD~`），这就是命令将会停止的地方。它依然会撤销一上次 `提交`，但还会 *取消暂存* 所有的东西。 于是，我们回滚到了所有 `git add` 和 `git commit` 的命令执行之前。

`git reset --hard HEAD~`：撤销了最后的提交、`git add` 和 `git commit` 命令 **以及** 工作目录中的所有工作。

可通过 `reflog` 找回撤销的 commit 。

