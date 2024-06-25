# Missing Semester 知识点总结

## 1. 课程概览与 shell

- shell 会从环境变量 `$PATH` 中搜索执行程序的路径（基于程序的名称搜索）
- `which` 用于确定某个程序名代表的是哪个具体的程序
- 为进入某个文件夹，需要具备该文件夹以及其父文件夹的“**搜索**”权限（以“可执行” x 权限表示）。

---

## 2. shell 工具与脚本

> 以下都针对 bash shell 而言

- 为变量赋值： `foo=bar`
- 访问变量值： `$foo`

> 注意 `foo = bar` （使用空格隔开）无法正确工作。因为解释器会调用程序 `foo` 并将 `=` 和 `bar` 作为参数。在 shell 脚本中使用空格会起到分割参数的作用，有时可能会造成混淆，请务必多加检查。

- `$0` - 脚本名
- `$1` 到 `$9` - 脚本的参数。 `$1` 是第一个参数，依此类推。
- `$@` - 所有参数
- `$#` - 参数个数
- `$?` - 前一个命令的返回值
- `$$` - 当前脚本的进程识别码
- `!!` - 完整的上一条命令，包括参数。常见应用：当你因为权限不足执行命令失败时，可以使用 `sudo !!` 再尝试一次。
- `$_` - 上一条命令的最后一个参数。如果你正在使用的是交互式 shell，你可以通过按下 Esc 之后键入 . 来获取这个值。

- 命令返回 0 表示正常执行，返回非 0 表示有错误发生。

- `true` 程序的返回码永远是 0，`false` 程序的返回码永远是 1。

- 通过命令替换（command substitution）以变量的形式获取一个命令的输出。比如 `$( CMD )` 会被替换为 `CMD` 命令的输出

- 进程替换（process substitution）， `<( CMD )` 会执行 `CMD` 并将结果输出到一个临时文件中，并将 `<( CMD )` 替换成**临时文件名**。这在我们希望返回值通过文件而不是 STDIN 传递时很有用。例如， `diff <(ls foo) <(ls bar)` 会显示文件夹 foo 和 bar 中文件的区别。

- shell 通配（globbing）：基于文件扩展名展开表达式
- 花括号 `{}`：当你有一系列的指令，其中包含一段公共子串时，可以用花括号来自动展开这些命令。这在批量移动或转换文件时非常方便。

```bash
convert image.{png,jpg}
# 会展开为
convert image.png image.jpg
```

- `find` 命令：递归地搜索符合条件的文件

```bash
# 查找所有名称为src的文件夹
find . -name src -type d
# 查找所有文件夹路径中包含test的python文件
find . -path '*/test/*.py' -type f
# 查找前一天修改的所有文件
find . -mtime -1
# 查找所有大小在500k至10M的tar.gz文件
find . -size +500k -size -10M -name '*.tar.gz'
```

除了列出所寻找的文件之外，find 还能对所有查找到的文件进行操作，这能极大地简化一些单调的任务。

```bash
# 删除全部扩展名为.tmp 的文件
find . -name '*.tmp' -exec rm {} \;
# 查找全部的 PNG 文件并将其转换为 JPG
find . -name '*.png' -exec convert {} {}.jpg \;
```

- grep 命令：对输入文本进行匹配

grep 有很多选项：

- `-C` ：获取查找结果的上下文（Context）；
- `-v`：对结果进行反选（Invert）即输出不匹配的结果。

`grep -C 5`：输出匹配结果前后五行。

- `-R` ：递归地进入子目录并搜索所有的文本文件。

- `Ctrl+R` 对命令历史记录进行回溯搜索

---

## 3. vim 编辑器

- `:e {文件名}`：打开要编辑的文件
- `:help {标题}`：打开帮助文档，比如 `:help :w` 打开 `:w` 命令的帮助文档。
- `~`：改变字符大小写
- `da'` 删除一个单引号字符串，包括周围的单引号
- 编辑 `~/.vimrc` 配置文件来自定义 vim
- 8.0 后的 vim 有内置的插件管理器，为了扩展 vim 只需创建一个 `~/.vim/pack/vendor/start` 文件夹并将插件放入其中。
- `:s`：替换命令。`:s /test/Test/g` 在整个文件中将 test 全局替换成 Test
- `:sp`/`:vsp`：分割窗口

---

## 4. 数据整理

- `sed`：基于 `ed` 的流编辑器
- `.`：匹配除换行符之外的任意单个字符
- 在 `*` 或 `+` 后跟一个 `?` 可使表达式变成**非贪婪**模式。（sed 不支持）
- sed 的 -E 选项可启用扩展的正则表达式
- sed 的替换命令：s/REGEX/SUBSTITUTION/
- `sort`：排序输入数据
- `uniq -c`：将连续出现的行折叠为一行并用出现次数作为前缀
- `sort -n`：按数字顺序排序输入（默认为 ASCII）

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
