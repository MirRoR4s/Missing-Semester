# Missing-semester

记录《计算机教育中缺失的一课》中提出的问题与解决方案。

## 6. 版本控制

### 可视化版本历史

一个版本对应一次提交，所以可视化版本历史就是可视化提交（commit）历史。

通过 `git log --graph --all --oneline --decorate` 可视化提交历史（以图形方式展示提交历史和分支结构。）。

- --all：显示所有分支的提交历史，包括远程分支和分离头指针。
- --graph：以图形方式显示提交历史和分支结构。
- --oneline：以一行显示每个提交的简化信息。
- --decorate：显示每个提交的引用，包括分支、标签等

#### 参考资料

- [Pro git 第二版 git log](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2#log_options)
- [Git 如何理解git log –all –graph –oneline –decorate的图形输出](https://geek-docs.com/git/git-questions/740_git_how_to_understand_the_graph_output_of_git_log_all_graph_oneline_decorate.htmlhttps://www.git-scm.com/docs/git-log/zh_HANS-CN)

### 找出 README.md 的最后修改者

可以通过 `git log` 和 `git blame` 来做，采用 log 做法的命令是：`git log -1 README.md`

- -\<n\>：只显示最近的 n 次提交

#### 参考资料

[git 查看文件修改记录](https://geek-docs.com/git/git-questions/41_tk_1705454228.html)

### 清除提交历史中的敏感信息

最近的做法是利用 `git filter-repo` 这款工具

- [从 commit 历史移除敏感文件](https://blog.csdn.net/yuanlaijike/article/details/89879525)

#### 参考资料

- <https://debugtalk.com/post/clean-sensitive-data-from-git-history-commits/>

- [git stash 相关](https://www.cnblogs.com/lifan-fineDay/p/16960584.html)
- [gitconfig 配置别名](https://www.liaoxuefeng.com/wiki/896043488029600/898732837407424)
- [gitignore 与 core_exlucdesfile] (<https://git-scm.com/docs/gitignore/zh_HANS-CN>)
