# Git 基础操作和命令

> **环境说明：** WSL2 (Ubuntu 24.04) + Positron IDE

---

## 基本工作流

```bash
# 1. 查看当前状态
git status

# 2. 暂存文件
git add <file>        # 添加单个文件
git add .             # 添加所有改动

# 3. 提交
git commit -m "描述改动"

# 4. 推送到远程
git push origin <branch-name>

# 5. 拉取更新
git pull origin <branch-name>
```

---

## 初始化配置（只需做一次）

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"

# WSL2 换行符设置（避免 Windows/Linux 冲突）
git config --global core.autocrlf input

# 查看所有配置
git config --list
```

---

## 从零开始：本地仓库连接 GitHub

### 1. 初始化本地仓库

```bash
cd 你的项目目录
git init
git add .
git commit -m "Initial commit"
```

### 2. 在 GitHub 创建新仓库

1. 登录 [github.com](https://github.com)
2. 右上角 `+` → **New repository**
3. 填写仓库名
4. **不要勾选** "Initialize with README"（本地已有文件）
5. 点 **Create repository**，复制页面上的仓库 URL

### 3. 连接并推送

```bash
git remote add origin https://github.com/用户名/仓库名.git
git branch -M main
git push -u origin main
```

### 4. 后续日常更新

```bash
git add .
git commit -m "你的提交信息"
git push
```

---

## 远程操作

```bash
# 克隆远程仓库
git clone https://github.com/用户名/仓库名.git

# 查看远程仓库
git remote -v

# 添加远程仓库
git remote add origin https://github.com/用户名/仓库名.git

# 修改远程仓库地址
git remote set-url origin https://github.com/用户名/新仓库名.git

# 获取远程更新（不自动合并）
git fetch origin

# 拉取并合并
git pull origin main

# 推送
git push origin <branch-name>
git push -u origin main          # -u 设置上游，之后直接 git push 即可
```

---

## 分支管理

```bash
# 创建并切换分支
git switch -c <branch-name>      # 推荐新语法
git checkout -b <branch-name>    # 旧语法

# 切换分支
git switch <branch-name>

# 查看所有分支（含远程）
git branch -a

# 合并分支（先切换到目标分支）
git switch main
git merge <branch-name>

# 删除分支
git branch -d <branch-name>      # 安全删除（已合并）
git branch -D <branch-name>      # 强制删除
```

---

## 查看历史

```bash
# 查看提交日志
git log                           # 完整日志
git log --oneline                 # 简洁版
git log --oneline --graph --all   # 图形化展示所有分支

# 查看具体改动
git diff                          # 未暂存的改动
git diff --cached                 # 已暂存的改动
git diff HEAD~1                   # 和上一次提交比较
git diff <branch1> <branch2>      # 两个分支对比
git show <commit-hash>            # 查看某次提交详情

# 查看某个文件的历史
git log --oneline -- <file>
git log -p -- <file>              # 含每次的具体改动
```

---

## 撤销改动 / 回滚

### 改动还没 commit

```bash
# 撤销某个文件的改动
git restore <file>

# 撤销所有文件的改动
git restore .
```

### 已经 add，想取消暂存

```bash
# 取消暂存，改动回到工作区
git restore --staged <file>

# 取消暂存所有文件
git restore --staged .
```

### 已经 commit，想回滚

```bash
# 撤销上一次 commit，改动保留在暂存区（安全，常用）
git reset --soft HEAD~1

# 撤销上一次 commit，改动回到工作区（默认）
git reset --mixed HEAD~1

# 完全删除改动，回到上一次提交（危险！不可恢复）
git reset --hard HEAD~1

# 回滚到某个具体的 commit
git log --oneline            # 查看历史，找到目标 commit hash
git reset --hard <commit-hash>
```

### 已经 push 到 GitHub，想回滚

```bash
# 查看提交历史
git log --oneline

# 回滚到某个提交
git reset --hard <commit-hash>

# 强制推送到远程（谨慎！会覆盖远程历史）
git push -f origin main
```

**建议：** 已 push 到远程的提交通常不建议 force push，除非确实需要。更安全的做法是用新的 commit 来修复问题。

---

## 临时保存改动

```bash
# 临时保存未提交的改动（切换任务时常用）
git stash                         # 暂存
git stash list                    # 查看所有 stash
git stash pop                     # 恢复最新的 stash
git stash drop                    # 删除最新的 stash
```

---

## 标签管理

```bash
# 创建标签（常用于版本节点）
git tag v1.0
git tag -a v1.0 -m "第一个正式版本"   # 附注标签（推荐）

# 查看标签
git tag

# 推送标签到远程
git push origin v1.0
git push origin --tags             # 推送所有标签
```

---

## 实用技巧

```bash
# cherry-pick：把某次提交应用到当前分支
git cherry-pick <commit-hash>

# 出问题快速回滚到某次提交
git log --oneline                 # 找到目标 commit hash
git reset --hard <commit-hash>    # 回滚（危险！会丢失此后的改动）

# 文件权限频繁被标记为改动时
git config core.fileMode false
```

---

## 日常工作流总结

```bash
# 修改代码后的标准流程
git add .
git commit -m "描述你的改动"
git push

# 如果改错了，还没 commit
git restore .

# 如果 commit 了，想撤销（改动保留）
git reset --soft HEAD~1

# 如果 commit 了，想完全删除
git reset --hard HEAD~1
```

---

## 在 Positron 里用 Git

Positron 左侧栏有 **Source Control** 面板（图标像分叉的树），可以：
- 可视化查看文件改动（diff）
- 直接输入 commit message 并提交
- 切换 / 创建分支

命令行和 GUI 面板可以混用，效果一样。
