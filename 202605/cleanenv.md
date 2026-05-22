# 清除已经安装的ruyi，包括pip install 或者 pipx install 安装的
我来帮你系统性地排查并彻底清理所有 `ruyi` 安装，然后干净地重新安装。

## 1. 全面排查现有 ruyi 安装

在终端依次执行以下命令：

### 查找所有 ruyi 可执行文件
```bash
# 查找 PATH 中所有名为 ruyi 的可执行文件
which -a ruyi
/home/phebe/.local/bin/ruyi
/home/phebe/.local/bin/ruyi
/home/phebe/.local/bin/ruyi


# 更全面的查找
type -a ruyi
ruyi is /home/phebe/.local/bin/ruyi
ruyi is /home/phebe/.local/bin/ruyi
ruyi is /home/phebe/.local/bin/ruyi


# 查找所有位置的 ruyi 二进制/脚本
sudo find / -name "ruyi" -type f 2>/dev/null

/home/phebe/Downloads/ruyi-0.45.0/contrib/shell-completions/bash/ruyi
/home/phebe/.local/share/pipx/venvs/ruyi/bin/ruyi

```

### 检查 pip 安装的 ruyi
```bash
# 查看 pip 是否安装了 ruyi
pip show ruyi

Location: /home/phebe/.local/share/pipx/venvs/ruyi/lib/python3.10/site-packages


# 查看 pip 安装的 ruyi 位置
pip list | grep ruyi

ruyi                  0.47.0


# 用户级 pip 安装
pip3 show ruyi
Location: /home/phebe/.local/share/pipx/venvs/ruyi/lib/python3.10/site-packages

pip3 list | grep ruyi
ruyi                  0.47.0

```

### 检查 pipx 安装的 ruyi
```bash
# 列出 pipx 所有安装
pipx list

venvs are in /home/phebe/.local/share/pipx/venvs
apps are exposed on your $PATH at /home/phebe/.local/bin
manual pages are exposed at /home/phebe/.local/share/man
   package ruyi 0.47.0, installed using Python 3.10.12
    - ruyi

# 查看 ruyi 是否在 pipx 中
pipx list | grep ruyi

   package ruyi 0.47.0, installed using Python 3.10.12
    - ruyi

```

## 2. 彻底清理所有安装

根据上面的排查结果，按以下顺序清理：

### A. 清理 pipx 安装（如有）
```bash
pipx uninstall ruyi
uninstalled ruyi! ✨ 🌟 ✨
```

### B. 清理 pip 安装（如有）
```bash
# 用户级卸载
pip uninstall ruyi -y
WARNING: Skipping ruyi as it is not installed.


# 系统级卸载（如果用了 sudo）
sudo pip uninstall ruyi -y
WARNING: Skipping ruyi as it is not installed.

sudo pip3 uninstall ruyi -y
WARNING: Skipping ruyi as it is not installed.

```

### C. 清理直接下载的二进制
```bash
# 删除找到的所有 ruyi 二进制文件
# 根据 which ruyi 的结果，删除对应文件，例如：
sudo rm -f ~/.local/bin/ruyi

# 删除 Downloads 里的旧版本文件夹
sudo rm -rf /home/phebe/Downloads/ruyi-0.45.0/

```

### D. 清理 ruyi 数据和缓存
```bash
# 删除 ruyi 数据目录（会清除所有下载的工具链、包等）
sudo find / -name 'ruyi'
/home/phebe/Downloads/ruyi-0.45.0/contrib/shell-completions/bash/ruyi
/home/phebe/Downloads/ruyi-0.45.0/ruyi
/home/phebe/.cache/ruyi
/home/phebe/.local/share/ruyi
/home/phebe/.local/state/ruyi
/home/phebe/.config/ruyi

sudo rm -rf /home/phebe/Downloads/ruyi-0.45.0
sudo rm -rf /home/phebe/.cache/ruyi
sudo rm -rf /home/phebe/.local/share/ruyi
sudo rm -rf /home/phebe/.local/state/ruyi
sudo rm -rf /home/phebe/.config/ruyi


# 清理 shell 配置中的 ruyi 相关 PATH 设置
grep -n "ruyi" ~/.bashrc ~/.bash_profile ~/.profile ~/.zshrc 2>/dev/null
```

如果 `grep` 找到了相关行，编辑对应文件删除：
```bash
# 例如编辑 .bashrc
nano ~/.bashrc
# 删除类似 export PATH="$HOME/.local/bin:$PATH" 中涉及 ruyi 的行
```

---

## 3. 验证清理是否彻底

```bash
# 确认 ruyi 命令已不存在
which ruyi        # 应该无输出
type ruyi         # 应该无输出
command -v ruyi   # 应该无输出

# 确认没有残留数据
ls -la ~/.ruyi 2>/dev/null        # 应该无此目录
ls -la ~/.cache/ruyi 2>/dev/null  # 应该无此目录

# 刷新 shell 环境
source ~/.bashrc
hash -r
```



## 清除vscode插件和工作空间

## 清除eclipse插件和工作空间
