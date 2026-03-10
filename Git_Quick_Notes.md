# BIOL3207 Git 快速笔记

## 1. Git 是干什么的

Git 用来记录文件的历史变化。你可以把它理解成：

- `工作区`：你正在直接修改的文件
- `暂存区`：准备进入下一次提交的文件
- `提交`：一次正式保存的版本
- `远程仓库`：GitHub 上的仓库

最常见的流程就是：

```bash
修改文件 -> git add -> git commit -> git push
```

## 2. 今天遇到的几个典型报错

### 2.1 `fatal: not a git repository`

意思是当前目录不是 Git 仓库，没有 `.git/`。

解决：

```bash
git init
```

### 2.2 `does not have a commit checked out`

意思通常是某个子目录里也有 `.git/`，形成了“仓库套仓库”。

这次你的 `Workshop_1`、`Workshop_2`、`test` 目录里都各自有 `.git/`，所以外层 `git add .` 失败了。

排查：

```bash
find . -maxdepth 3 -name .git -print
```

### 2.3 `src refspec main does not match any`

意思通常是本地还没有成功提交，虽然分支名叫 `main`，但它还没有任何 commit。

解决：

```bash
git add .
git commit -m "Initial commit"
git push -u origin main
```

### 2.4 `could not read Username for 'https://github.com'`

意思是 GitHub 远程地址已经配置了，但推送时认证失败。

如果远程是 HTTPS：

- 用户名填 GitHub 用户名
- 密码位置填 GitHub PAT

## 3. 最基本的 Git 命令

### 3.1 初始化仓库

```bash
cd /home/lichi/ANU_26s1/BIOL3207
git init
```

### 3.2 查看状态

```bash
git status
git status -sb
```

说明：

- `git status`：完整状态
- `git status -sb`：更简洁，适合平时频繁查看

### 3.3 添加文件到暂存区

```bash
git add .
```

意思是把当前目录下所有变化加入暂存区。

如果你只想提交部分文件，更推荐明确指定：

```bash
git add Git_Quick_Notes.md
git add Workshop_1
git add Workshop_1/test.qmd
```

### 3.4 提交

```bash
git commit -m "Initial commit"
```

提交信息要尽量描述清楚这次改了什么。

例子：

```bash
git commit -m "Add Git quick notes"
git commit -m "Update workshop 1 notes"
git commit -m "Fix typo in README"
```

### 3.5 连接 GitHub 仓库

```bash
git remote add origin https://github.com/LijiayuDeng/BIOL3207.git
```

检查远程仓库：

```bash
git remote -v
```

### 3.6 重命名分支为 `main`

```bash
git branch -m main
```

### 3.7 推送到 GitHub

```bash
git push -u origin main
```

第一次推送用 `-u`，之后通常直接：

```bash
git push
```

## 4. 分支、fork、clone、pull 分别是什么

这四个概念很容易混，但用途不一样：

- `branch`：同一个仓库里开一条新的开发线
- `fork`：把别人的 GitHub 仓库复制到你自己的 GitHub 账号下
- `clone`：把远程仓库下载到本地
- `pull`：把远程仓库的最新更新拉到本地

### 4.1 如何创建分支

分支适合你想试改动，但不想直接改 `main`。

查看分支：

```bash
git branch
```

创建新分支：

```bash
git branch notes-update
```

创建并切换到新分支：

```bash
git switch -c notes-update
```

如果你的 Git 版本较旧，也可以用：

```bash
git checkout -b notes-update
```

切回主分支：

```bash
git switch main
```

把新分支推到 GitHub：

```bash
git push -u origin notes-update
```

### 4.2 fork 是什么

`fork` 不是本地 Git 命令，它是在 GitHub 网页上操作的。

适合场景：

- 你想修改别人的仓库
- 你没有原仓库的写权限
- 你想在自己账号下保留一份副本

操作方式：

1. 打开对方的 GitHub 仓库页面
2. 点击右上角 `Fork`
3. GitHub 会在你的账号下复制一个仓库

然后你再把你自己的 fork 拉到本地：

```bash
git clone 你的fork地址
```

### 4.3 如何从远程把仓库拉到本地

如果你本地还没有这个仓库，用：

```bash
git clone 仓库地址
```

例如：

```bash
git clone https://github.com/LijiayuDeng/BIOL3207.git
```

这会在当前目录下创建一个 `BIOL3207/` 文件夹。

然后进入：

```bash
cd BIOL3207
```

### 4.4 如何把远程最新内容拉下来

如果本地已经有这个仓库，想同步远程更新，用：

```bash
git pull
```

或者写得更明确一点：

```bash
git pull origin main
```

### 4.5 什么时候用哪一个

- 想在自己仓库里尝试新改动：`branch`
- 想复制别人的 GitHub 仓库到自己账号：`fork`
- 想第一次下载远程仓库到本地：`clone`
- 想更新本地仓库：`pull`

## 5. 如果仓库不想公开

GitHub 仓库可以设成 `Private`，这样只有你和你授权的人能看到。

如果你新建仓库时就不想公开：

1. 在 GitHub 点击 `New repository`
2. 填写仓库名
3. 在可见性里选择 `Private`
4. 创建仓库

如果仓库已经建好了，也可以改成私有：

1. 打开仓库页面
2. 进入 `Settings`
3. 拉到页面底部的 `Danger Zone`
4. 点击 `Change repository visibility`
5. 选择 `Make private`

注意：

- 改成私有不会影响本地 Git 命令
- 你还是照常用 `git add`、`git commit`、`git push`
- 区别只是别人不能随便访问这个仓库

如果你需要和别人协作，但又不想公开，可以把对方加为 collaborator。

## 6. 哪些文件通常不需要提交

不是所有文件都应该进 Git。一般不建议提交这些内容：

- 临时文件
- 编辑器自动生成的配置缓存
- 编译/渲染产生的中间文件
- 大型缓存目录
- 含隐私信息的文件
- 本地运行环境文件

在你这个项目里，常见不建议提交的内容包括：

- `.Rproj.user/`
- `*.html`（如果只是从 `.Rmd` 或 `.qmd` 渲染出来的结果）
- `*_files/`
- `renv/library/` 这类本地环境缓存
- `.DS_Store`
- `*.log`
- `*.tmp`

但也要看用途：

- 如果 HTML 是你想提交给老师或同学直接查看的成果，可以提交
- 如果 `renv.lock` 是为了记录 R 包环境，通常应该提交
- 如果只是本地缓存或自动生成结果，通常不提交

## 7. 不想提交某些文件，用什么命令

### 5.1 用 `.gitignore` 永久忽略

如果某类文件以后都不想提交，最标准的方法是写进 `.gitignore`。

先创建文件：

```bash
touch .gitignore
```

然后编辑它，把要忽略的规则写进去。

例如可以用编辑器打开：

```bash
nano .gitignore
```

或者：

```bash
code .gitignore
```

例如：

```gitignore
.Rproj.user/
.DS_Store
*.log
*.tmp
*_files/
```

作用：

- 以后这些文件不会出现在 `git status` 的未跟踪列表里
- 适合忽略长期不需要版本控制的内容

写完以后，把 `.gitignore` 本身提交到 Git：

```bash
git add .gitignore
git commit -m "Add gitignore"
```

### 5.2 如果文件已经被 Git 跟踪了

`.gitignore` 对“已经被跟踪的文件”不生效。  
这时要先把它从 Git 跟踪中移除，但保留本地文件：

```bash
git rm --cached 文件名
```

例如：

```bash
git rm --cached Workshop_1/test.html
git rm --cached -r Workshop_1/.Rproj.user
```

然后再提交：

```bash
git commit -m "Stop tracking generated files"
```

### 5.3 只是不想这次提交它

如果文件还没 `git add`，那最简单：什么都不用做，不加它就行。

比如你只提交一个文件：

```bash
git add Git_Quick_Notes.md
git commit -m "Add Git notes"
```

这样别的未跟踪文件不会进这次提交。

## 8. 如果加错了，怎么撤回

### 6.1 文件已经 `git add`，但还没 commit

把文件从暂存区拿出来：

```bash
git restore --staged 文件名
```

例如：

```bash
git restore --staged Workshop_1/test.html
```

如果想把所有刚刚 `add` 的东西都撤回：

```bash
git restore --staged .
```

注意：这不会删除文件，只是把它从暂存区拿掉。

### 6.2 文件改坏了，想恢复到上一次提交的版本

```bash
git restore 文件名
```

例如：

```bash
git restore Git_Quick_Notes.md
```

注意：这会丢掉该文件在工作区里的未提交修改，要谨慎。

## 9. 每次提交前建议做什么

推荐你每次都按这个顺序：

```bash
git status -sb
git add 想提交的文件
git status -sb
git commit -m "写清楚这次改了什么"
git push
```

这样可以避免：

- 把不该交的缓存文件一起提交
- 把自动生成的文件混进去
- 一次提交内容太乱

## 10. 查看历史和远程状态

### 8.1 看提交历史

```bash
git log --oneline -n 10
```

### 8.2 看当前分支

```bash
git branch
```

### 8.3 看远程仓库地址

```bash
git remote -v
```

### 8.4 看本地和远程是否同步

```bash
git status -sb
```

如果显示类似：

```bash
## main...origin/main
```

通常表示本地和远程都已经关联。

## 11. 适合你现在项目的做法

1. 提交前先看 `git status -sb`
2. 尽量不要上来就 `git add .`，除非你非常确定目录干净
3. 更稳妥的做法是只 `git add` 你确定要交的文件
4. 把 `.Rproj.user/`、日志、缓存、自动生成文件写入 `.gitignore`
5. 保持 `BIOL3207` 只有一个主仓库，不要再让子目录里出现新的 `.git/`

## 12. 常用命令速查

```bash
# 初始化仓库
git init

# 查看状态
git status
git status -sb

# 添加全部变化
git add .

# 添加指定文件
git add 文件名

# 提交
git commit -m "提交说明"

# 查看历史
git log --oneline -n 10

# 添加远程仓库
git remote add origin 仓库地址

# 查看远程仓库
git remote -v

# 改分支名为 main
git branch -m main

# 创建并切换分支
git switch -c 分支名

# 切换分支
git switch 分支名

# 推送
git push -u origin main

# 克隆远程仓库
git clone 仓库地址

# 拉取远程更新
git pull
git pull origin main

# 取消暂存
git restore --staged 文件名

# 恢复文件到上次提交状态
git restore 文件名

# 停止跟踪但保留本地文件
git rm --cached 文件名
```
