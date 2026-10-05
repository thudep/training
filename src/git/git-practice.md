# git 实操练习

在开始下述内容之前，请确保已完成 [git 安装](./git.md#git-安装)与 [git 用户信息配置与 ssh 配置](./git.md#git-用户信息配置与-ssh-配置)。

## 本地操作

### 第一次提交

创建一个新文件夹并进入，使用如下命令初始化一个 git 仓库：
```bash
git init
```
创建 `.gitignore` 文件与 `README.md` ：
```bash
touch .gitignore
touch README.md  # 你可以写入一些基本信息
```
把这两个文件暂存并提交
```bash
git add .
git commit -m "Initial commit"
```

### 使用 `.gitignore` 取消对某些文件的追踪

创建一个（形式上的） python 虚拟环境：
```bash
# 如果你已经准备好了 python
python3 -m venv .venv
# 如果你还没有准备好 python , 可以创建一个形式上的
mkdir .venv
touch .venv/.gitkeep
```
在 `.gitignore` 中，添加以下内容：
```gitignore
.venv/
```
完成提交：
```bash
git add .
git commit -m "chore: 添加了 python 虚拟环境, 并被 .gitignore 忽略."
```

接下来，我们将体验一下如何取消对某些已追踪文件的追踪。

模拟两个文件，一个是确实有效的文件，一个是测试文件：
```bash
touch main.py
touch test.py
```
不小心忘记在 `.gitignore` 中添加 `test.py` ，直接提交
```bash
git add .
git commit -m "perf: 完成了主程序."
``` 
如果想取消对 `test.py` 的追踪，你需要先在 `.gitignore` 中添加以下内容
```gitignore
test.py
```
但你可以发现 `test.py` 仍然在仓库中被追踪，这时你需要在 git 缓存中删除它（如果你在本地也不需要保留，可以不加 `--cache` ）
```bash
git rm --cache test.py
``` 
然后完成提交
```bash
git add .
git commit -m "chore: 取消对 test.py 的追踪."
```

### 撤回提交

模拟创造一个有 bug 的文件并完成提交：
```bash
touch bug.py
git add .
git commit -m "feat: 添加一个绝妙的功能."
```
使用如下命令撤回本次提交
```bash
git revert HEAD
```

### 分支管理

创建一个分支并切换到该分支上：
```bash
git checkout -b new
# 你也可以使用以下命令的组合
git branch new
git checkout new
```
在该分支开发并提交：
```bash
touch new.py
git add .
git commit -m "feat: 添加一个新功能 new.py ."
```
回到主分支，进行开发并提交：
```bash
git checkout master  # 如果你的主分支是 main , 请修改
touch new_m.py
git add .
git commit -m "feat: 添加一个新功能 new_m.py ."
```
使用 `merge` 完成分支合并：
```bash
git merge new
```

接下来我们将手动创造一些冲突，并实现冲突处理：

在 `main.py` 中写入：
```python
print("hello world")

```
然后提交：
```bash
git add .
git commit -m "feat: 实现了 hello world ."
```
开一个新分支：
```bash
git checkout -b conflict
```
在该分支下，把 `main.py` 的内容修改为：
```python
print("Hello world")

```
然后提交：
```bash
git add .
git commit -m "fix: 把 hello world 的首字母大写."
```
之后回到主分支：
```bash
git checkout master  # 如果你的主分支是 main , 请修改
```
在该分支下，把 `main.py` 的内容修改为：
```python
print("hello world.")

```
然后提交：
```bash
git add .
git commit -m "fix: 在 hello world 后添加句点."
```
接着使用 `merge` 执行合并操作：
```bash
git merge conflict
```
会出现报错，例如：
```bash
Auto-merging main.py
CONFLICT (content): Merge conflict in main.py
Automatic merge failed; fix conflicts and then commit the result.
```
这是因为两个分支下都对 `main.py` 的同一行进行了修改， Git 不能自动选择该接受哪一个，这时你必须自行处理，打开 `main.py` ，把里面的内容改为（一个综合考虑两个分支修改的例子，你也可以选择采用某一分支的更改）：
```python
print("Hello world.")

```
然后手动进行合并操作：
```bash
git add .
git commit -m "Merge branch 'conflict'"
```

## 远程仓库

### 在 git 平台中添加 SSH Keys 并验证

首先进入[清华大学 GitLab](https://git.tsinghua.edu.cn/)，点击右上角的头像，点 `Edit Profile` ，在左侧点击 `SSH Keys` ，点 `Add new key` ，然后把你的**公钥**粘贴进去。

我们可以尝试 `clone` 一个仓库，来看一下你的 ssh key 能否正常使用，进入一个新的文件夹，输入：
```bash
git clone git@git.tsinghua.edu.cn:dep-training/git-example.git
```
如果能够 `clone` 下来，就说明你的 ssh key 可以正常使用；否则可能会报错，例如：
```bash
Cloning into 'git-example'...
git@git.tsinghua.edu.cn: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

### 建立自己的远程仓库并使用

点击中间正上方的 `Search or go to...` ，点 `Your work` ，在左侧点 `Projects` ，然后点击蓝色的 `New Project` 按钮，选择 `Create blank project` 。之后输入你的 Project name （自取），在 Project URL 的 `Pick a group or namespace` 中，选择 Users 下你的 id ，对于仓库的可见性 Visibility Level ，你可以自行选择， `Private` 意味着除本仓库的成员外其他人不可见， `Internal` 意味着只有能登陆清华大学 GitLab 的成员可见（相当于半公开），最后取消掉下面的 `Initialize repository with a README` （如果选择了，仓库会自动生成一个 `README.md` ，该 `README.md` 中一般只包含一个类似于教程的操作信息，为避免冲突，建议不勾选） ，点击 `Create project` ，即完成了你的仓库创建。

在你的仓库中，点击蓝色的 `Code` 按钮，点第一个复制按钮来复制 Clone with SSH 下的链接，下文称为 `<url>` 。

回到我们刚才一通操作的那个仓库，为它添加远程仓库：
```bash
git remote add origin <url>
```
然后把本地仓库发布到远程：
```bash
git push -u origin master
# 如果你想把 master 以外的分支也 push 上去
git push -u origin new
git push -u origin conflict
```
检查你的远程仓库，可以发现里面已经有你本地仓库的内容了。

接下来，我们将模拟多人协作的场景，来制造一些冲突并解决。

首先在远程仓库中，点击蓝色的 `Code` 按钮，点 `Web IDE` ，这将会打开一个网页端的 VSCode ，修改 `main.py` 为
```python
print("A: Hello world.")

```
接下来，你可以使用图形化界面，点击左侧的 git 图标，在框中写入 commit message ，例如 "`feat: 让 A 说 Hello world.`" ，然后点击 `Commit and push to 'master'` ，点 `Continue` 。回到你的远程仓库后，你可以发现完成了修改。但是你的本地仓库还没有同步，我们来手动制造一点冲突，在本地把 `main.py` 修改为：
```python
print("B: Hello world.")

```
然后提交：
```bash
git add .
git commit -m "feat: 让 B 说 Hello world."
```
接下来，我们来完成一下同步，输入：
```python
git pull origin master
```
这时由于本地和远程都有更新，会出现分叉的分支，所以会报错，例如：
```bash
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint: 
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint: 
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.
```
上述提示中给出了三种解决方案，分别是 `merge`, `rebase`, `fast-forward only` ，如果使用了它提供的 `git config` 命令，会给仓库设置默认的合并策略，我们先不设置，而是指定本次使用 `rebase` 策略进行 `pull` ：
```bash
git pull --rebase origin master
```
然后会发现，出现了和之前类似的冲突报错，例如：
```bash
Auto-merging main.py
CONFLICT (content): Merge conflict in main.py
error: could not apply 6e65887... feat: 让 B 说 Hello world.
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 6e65887... feat: 让 B 说 Hello world.
```
然后手动解决冲突，譬如选择接受让 B 说，然后输入：
```bash
git add .
git commit -m "feat: 让 B 说 Hello world."
git rebase --continue
```
然后推送到远程仓库：
```bash
git push origin master
```
