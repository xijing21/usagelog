## 环境准备
- 测试环境：vmware + ubuntu 24.04.4
- https://fast-mirror.isrc.ac.cn/ruyisdk/ruyi/releases/0.49.0/ruyi-0.49.0.amd64

从0.48开始，需要python 3.10以上的版本：
- python：
```
python3 --version
Python 3.12.3
```

- 计划使用pipx安装ruyi：
# 更新软件源并安装 pipx
sudo apt update
sudo apt install -y pipx
pipx ensurepath
source ~/.bashrc


安装 vscode 

<img width="1258" height="1266" alt="image" src="https://github.com/user-attachments/assets/12a0c4cc-0630-45bb-bc87-e8e2de145ebb" />

<img width="1332" height="786" alt="image" src="https://github.com/user-attachments/assets/0b0a2510-c927-476e-8908-4e62d7682ee8" />


```
ruyi update
信息：正在同步仓库 'ruyisdk'
信息：正在更新软件包仓库
Traceback (most recent call last):
  File "/home/phebe/.local/bin/ruyi", line 6, in <module>
    sys.exit(entrypoint())
             ^^^^^^^^^^^^
  File "/home/phebe/.local/share/pipx/venvs/ruyi/lib/python3.12/site-packages/ruyi/__main__.py", line 110, in entrypoint
    sys.exit(main(gm, gc, sys.argv))
             ^^^^^^^^^^^^^^^^^^^^^^
  File "/home/phebe/.local/share/pipx/venvs/ruyi/lib/python3.12/site-packages/ruyi/cli/main.py", line 185, in main
    return func(gc, args)
           ^^^^^^^^^^^^^^
  File "/home/phebe/.local/share/pipx/venvs/ruyi/lib/python3.12/site-packages/ruyi/ruyipkg/update_cli.py", line 46, in main
    mr.sync_all()
  File "/home/phebe/.local/share/pipx/venvs/ruyi/lib/python3.12/site-packages/ruyi/ruyipkg/composite_repo.py", line 88, in sync_all
    repo.sync()
  File "/home/phebe/.local/share/pipx/venvs/ruyi/lib/python3.12/site-packages/ruyi/ruyipkg/repo.py", line 460, in sync
    pull_ff_or_die(
  File "/home/phebe/.local/share/pipx/venvs/ruyi/lib/python3.12/site-packages/ruyi/utils/git.py", line 167, in pull_ff_or_die
    repo.checkout_tree(tgt)
  File "/home/phebe/.local/share/pipx/venvs/ruyi/lib/python3.12/site-packages/pygit2/repository.py", line 442, in checkout_tree
    payload.check_error(err)
  File "/home/phebe/.local/share/pipx/venvs/ruyi/lib/python3.12/site-packages/pygit2/callbacks.py", line 111, in check_error
    check_error(error_code)
  File "/home/phebe/.local/share/pipx/venvs/ruyi/lib/python3.12/site-packages/pygit2/errors.py", line 67, in check_error
    raise GitError(message)
_pygit2.GitError: 629 conflicts prevent checkout
```

问题原因及解决办法：

这个错误是由于 RuyiSDK 的本地软件包仓库缓存（基于 Git 管理）在同步时，与上游远程仓库产生了代码冲突（conflicts prevent checkout）。通常是因为本地缓存文件被意外修改、损坏，或者远程仓库强制推送了更新。 [1, 2, 3, 4] 
因为这只是元数据和软件包信息的缓存，直接将其删除并让 Ruyi 重新下载是最快速、最彻底的解决方案。请按照以下步骤操作：
## 核心解决方法：清理缓存法（推荐）

   1. 彻底删除本地的仓库缓存
   打开终端，运行以下命令删除 ruyisdk 的 Git 缓存目录：
  
   ruyi self clean --all  ( ruyi self clean --repo  最小影响执行这个命令)
   ruyi update
   
<img width="970" height="1076" alt="image" src="https://github.com/user-attachments/assets/93996bbb-c28d-48b0-b819-504047fe4725" />

回顾报错：
_pygit2.GitError: 629 conflicts prevent checkout

* 报错核心：Ruyi 内部使用 pygit2（一个 Git 库）来管理软件包仓库的元数据（也就是 manifests/ruyisdk 目录下的清单文件）。这个报错代表本地的 Git 仓库状态混乱了。
* 各清理选项的影响：
* --repo：（精准解药） 专门用来移除 Ruyi 管理的本地仓库缓存。执行它会直接把那个死锁、冲突的 Git 目录删掉。由于导致报错的根源就是这个 Git 仓库，清除它后执行 ruyi update 就会重新 git clone 一份干净的仓库，从而完美解决问题。
   * --progcache：清除的是 Ruyi 的程序运行缓存（例如网络请求缓存、临时解析数据等），这部分不包含 Git 仓库本身，所以单用它无法解决 Git 冲突。

## 总结：如何用最小影响应对各种故障？
下次如果还遇到类似问题，可以对照这个策略：

   1. 更新报错 / Git 冲突（如你本次遇到的问题）
   * 最小影响命令：ruyi self clean --repo
      * 作用：只删软件包清单（几 MB 大小），完全不影响已经安装的编译器、工具链和下载的安装包。
   2. 磁盘空间不够 / 想清理下载的安装包
   * 最小影响命令：ruyi self clean --distfiles
      * 作用：只删除下载好的 .tar.gz 等压缩包安装源，腾出空间，但不卸载已安装的工具。
   3. 彻底重置环境（你想推倒重来）
   * 命令：ruyi self clean --all
      * 作用：清除一切。这会导致不仅仓库要重新同步，你之前通过 ruyi install 安装的所有软件包（--installed-pkgs）也全都会被卸载删除。
   

