# 开发环境速查手册

> **环境：** WSL2 (Ubuntu 24.04) + Conda + Positron IDE  
> **R 版本：** 4.5.2 | **Python 版本：** 3.11

---

## 文件索引

| 文件 | 内容 |
|------|------|
| [`git_basics.md`](./git_basics.md) | Git 常用命令、GitHub 连接与新建仓库 |
| [`ubuntu_basics.md`](./ubuntu_basics.md) | Ubuntu / WSL2 常用命令、文件操作、Conda 环境管理 |
| [`r_basics.md`](./r_basics.md) | R 常用命令、tidyverse、ggplot2、统计分析 |
| [`python_pytorch.md`](./python_pytorch.md) | Python、NumPy、Pandas、PyTorch 建模、Scikit-learn |

---

## 快速上手

### 激活开发环境

```bash
conda activate r_workshop
```

### 日常 Git 工作流

```bash
git add .
git commit -m "描述改动"
git push
```

### 连接 GitHub（首次）

```bash
git remote add origin https://github.com/用户名/仓库名.git
git branch -M main
git push -u origin main
```
