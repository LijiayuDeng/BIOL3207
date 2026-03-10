# Ubuntu 常用命令速查

> **环境说明：** WSL2 (Ubuntu 24.04) + Positron IDE

---

## 文件和目录操作

```bash
# 查看当前目录
pwd

# 列出文件
ls                    # 基本列表
ls -la                # 详细信息（含隐藏文件）
ls -lh                # 人类可读的文件大小

# 切换目录
cd 目录名
cd ..                 # 上一级
cd ~                  # 回到主目录
cd -                  # 回到上一个目录

# 创建目录
mkdir 目录名
mkdir -p a/b/c        # 递归创建多级目录

# 创建文件
touch 文件名
nano 文件名           # 创建并编辑

# 复制
cp 源文件 目标文件
cp -r 源目录 目标目录  # 递归复制目录

# 移动 / 重命名
mv 源 目标

# 删除
rm 文件名
rm -r 目录名          # 递归删除目录（谨慎！）
rm -rf 目录名         # 强制删除（非常谨慎！）

# 查看文件内容
cat 文件名
less 文件名           # 分页查看（q 退出）
head -n 20 文件名     # 查看前 20 行
tail -n 20 文件名     # 查看后 20 行
tail -f 文件名        # 实时监听文件末尾（看日志常用）
```

---

## 搜索

```bash
# 搜索文件
find . -name "*.py"                   # 在当前目录搜索 .py 文件
find . -type f -name "*.csv"          # 只搜索文件
find . -type d -name "data"           # 只搜索目录

# 搜索文件内容
grep "关键词" 文件名
grep -r "关键词" 目录名              # 递归搜索
grep -n "关键词" 文件名              # 显示行号
grep -i "关键词" 文件名              # 忽略大小写

# 组合用法
find . -name "*.py" | xargs grep "import pandas"
```

---

## 权限管理

```bash
# 查看权限
ls -la

# 修改权限
chmod +x 文件名       # 添加执行权限
chmod 755 文件名      # rwxr-xr-x
chmod 644 文件名      # rw-r--r--

# 修改所有者
sudo chown 用户名:组名 文件名
```

---

## 包管理（apt）

```bash
# 更新包列表
sudo apt update

# 升级所有包
sudo apt upgrade

# 安装包
sudo apt install 包名

# 卸载包
sudo apt remove 包名
sudo apt purge 包名        # 同时删除配置文件

# 搜索包
apt search 关键词

# 查看已安装的包
dpkg -l | grep 包名
```

---

## 进程管理

```bash
# 查看运行中的进程
ps aux
ps aux | grep python      # 过滤 python 进程

# 动态查看进程（类似任务管理器）
top
htop                      # 更友好（需安装：sudo apt install htop）

# 杀死进程
kill <PID>
kill -9 <PID>             # 强制杀死

# 后台运行
命令 &
nohup 命令 &              # 退出终端后继续运行

# 查看端口占用
lsof -i :8080
```

---

## 磁盘和内存

```bash
# 磁盘使用情况
df -h                     # 各分区使用情况
du -sh 目录名             # 某目录占用大小
du -sh *                  # 当前目录下每个文件/目录大小

# 内存使用情况
free -h
```

---

## 网络

```bash
# 测试网络连通性
ping google.com

# 下载文件
wget URL
curl -O URL

# 查看网络接口
ip addr
ifconfig                  # 需安装：sudo apt install net-tools
```

---

## 压缩和解压

```bash
# tar.gz
tar -czvf 压缩包.tar.gz 目录名      # 压缩
tar -xzvf 压缩包.tar.gz             # 解压
tar -xzvf 压缩包.tar.gz -C 目标目录  # 解压到指定目录

# zip
zip -r 压缩包.zip 目录名            # 压缩
unzip 压缩包.zip                    # 解压
unzip 压缩包.zip -d 目标目录        # 解压到指定目录
```

---

## 环境变量

```bash
# 查看所有环境变量
env
printenv

# 查看单个变量
echo $PATH
echo $HOME

# 临时设置（当前终端有效）
export MY_VAR="value"

# 永久设置（写入 ~/.bashrc）
echo 'export MY_VAR="value"' >> ~/.bashrc
source ~/.bashrc          # 立即生效
```

---

## Conda 环境管理

```bash
# 创建新环境
conda create -n 环境名 python=3.11

# 激活 / 退出环境
conda activate 环境名
conda deactivate

# 列出所有环境
conda env list

# 安装包
conda install 包名
pip install 包名

# 导出环境
conda env export > environment.yml

# 从文件还原环境
conda env create -f environment.yml

# 删除环境
conda env remove -n 环境名
```

---

## 在 WSL2 中访问 Windows 文件

```bash
# Windows 的 C 盘在 WSL2 里挂载在
cd /mnt/c/Users/你的Windows用户名/

# 从 Windows 资源管理器打开当前 WSL2 目录
explorer.exe .
```

---

## 常用快捷键

| 快捷键 | 作用 |
|--------|------|
| `Ctrl + C` | 中止当前命令 |
| `Ctrl + Z` | 挂起当前命令（后台） |
| `Ctrl + D` | 退出当前 shell |
| `Ctrl + L` | 清屏 |
| `Ctrl + A` | 光标移到行首 |
| `Ctrl + E` | 光标移到行尾 |
| `↑ / ↓` | 浏览历史命令 |
| `Tab` | 自动补全 |
| `!!` | 重复上一条命令 |
| `sudo !!` | 用 sudo 重复上一条命令 |
