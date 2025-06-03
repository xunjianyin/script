# 我的自定义 Bash 环境配置

本文档概述了用于增强 Bash Shell 体验的自定义别名和函数设置。所有自定义命令均存储在 `~/.bash_aliases` 文件中，以便更好地组织和管理。

## 目的

此设置的主要目标是：
* 使用更短、易记的别名简化常见的命令行任务。
* 使用 Bash 函数实现更复杂的自定义命令。
* 通过将个人自定义集中到 `~/.bash_aliases`，保持主 `~/.bashrc` 文件的整洁。
* **显著增强安全性**：用一个将文件移至回收站的版本替换标准的 `rm` 命令。

## 文件结构

此设置涉及两个主要文件：

1.  **`~/.bashrc`**：
    *标准的 Bash 运行控制脚本，在交互式非登录 shell 启动时执行。
    *它被配置为加载（source）自定义命令文件。

2.  **`~/.bash_aliases`**：
    *此文件包含所有用户定义的别名和函数。
    * **注意**：尽管名为 `.bash_aliases`，在此设置中，该文件用于存储**别名和函数两者**。

## 工作原理

`~/.bashrc` 文件包含一段代码片段，用于检查 `~/.bash_aliases` 文件是否存在。如果文件存在，`~/.bashrc` 将会“source”它，这意味着它会在当前 shell 会话中读取并执行 `~/.bash_aliases` 中的命令。这使得所有定义的别名和函数都可以在终端中使用。

`~/.bashrc` 中的相关代码行如下：

```bash
# 如果自定义命令文件存在，则加载它
if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases  # 也可以使用 'source ~/.bash_aliases'
fi
````

## 🌟 重要：安全的 `rm` (移至回收站) 🌟

标准的 `rm` 命令已被一个更安全的版本 (`safe_rm_to_trash`) **覆盖**。这个新的 `rm` 命令不会永久删除文件和目录，而是将它们移动到位于 `~/.Trash` 的回收站目录中。

  * **行为特性**：
      * `rm file.txt` 会将 `file.txt` 移动到 `~/.Trash/file.txt.<timestamp>`。
      * `rm -r my_directory` 会将 `my_directory` 移动到 `~/.Trash/my_directory.<timestamp>`。
      * 它会尝试支持 `-f` (强制，忽略不存在的文件) 和 `-v` (详细输出) 标志。
      * 此自定义 `rm` **不支持** `-i` (交互式) 标志。
  * **回收站管理**：
      * 提供了一个新命令 `emptytrash` (也别名为 `cleartrash`)，用于在提示确认后永久删除 `~/.Trash` 目录中的所有项目。
  * **安全性**：这显著降低了意外永久丢失数据的风险。

## `~/.bash_aliases` 中的自定义命令详情

以下是可用的别名和函数的详细列表：

### 通用别名

| 别名       | 描述                                                                 | 原始命令 (示例)      |
| :--------- | :------------------------------------------------------------------- | :------------------------------ |
| `..`       | 返回上一级目录。                                                       | `cd ..`                         |
| `...`      | 返回上两级目录。                                                       | `cd ../..`                      |
| `....`     | 返回上三级目录。                                                       | `cd ../../..`                   |
| `ls`       | 带颜色列出目录内容。                                                   | `ls --color=auto`               |
| `ll`       |以长格式列出所有文件，带类型指示符和颜色。                               | `ls -alF --color=auto`          |
| `la`       | 列出所有文件（包括隐藏文件），带颜色。                                 | `ls -A --color=auto`            |
| `l`        | 以列格式列出文件，带类型指示符和颜色。                                 | `ls -CF --color=auto`           |
| `lsg`      | 列出文件（长格式）并进行 grep (不区分大小写)。                           | `ll \| grep --color=auto -i`    |
| `cp`       | 复制文件，覆盖前提示，并显示详细输出。                                 | `cp -iv`                        |
| `mv`       | 移动/重命名文件，覆盖前提示，并显示详细输出。                             | `mv -iv`                        |
| `mkdir`    | 创建目录（包括父目录），并显示详细输出。                               | `mkdir -pv`                     |
| `dfh`      | 以人类可读格式显示磁盘可用空间。                                       | `df -h`                         |
| `duh`      | 以人类可读格式显示当前文件夹的磁盘使用情况。                             | `du -sh`                        |
| `dufh`     | 以人类可读格式显示当前目录中各项的磁盘使用情况，并按大小排序。             | `du -sh * \| sort -rh`          |
| `freeh`    | 以人类可读格式显示可用内存。                                           | `free -h`                       |
| `psef`     | 在进程列表中 grep (不区分大小写)。                                     | `ps -ef \| grep --color=auto -i`|
| `myip`     | 获取您的公网 IP 地址。                                                 | `curl -s ipinfo.io/ip \|\| curl -s ifconfig.me` |
| `update`   | 更新系统软件包 (Debian/Ubuntu 示例)。                                 | `sudo apt update && sudo apt upgrade -y` |
| `cls`, `c` | 清除终端屏幕。                                                         | `clear`                         |
| `pingg`    | Ping https://www.google.com/url?sa=E\&source=gmail\&q=google.com。                                                      | `ping google.com`               |
| `sln`      | 创建符号链接 (`TARGET LINK_NAME`)。                                    | `ln -s`                         |
| `slnh`     | 创建符号链接，将 `LINK_NAME` 视为普通文件 (`TARGET LINK_NAME`)。       | `ln -s -T`                      |

-----

### GPU 别名 (NVIDIA)

| 别名       | 描述                                                                 | 原始命令 (示例)      |
| :--------- | :------------------------------------------------------------------- | :------------------------------ |
| `gpu`      | 显示 NVIDIA GPU 状态。                                                 | `nvidia-smi`                    |
| `gpumem`   | 显示 NVIDIA GPU 内存使用情况 (已用, 总量)。                            | `nvidia-smi --query-gpu=memory.used,memory.total --format=csv,noheader,nounits` |
| `gpuwatch` | 监控 NVIDIA GPU 状态，每1秒刷新一次。                                  | `watch -n 1 nvidia-smi`         |

-----

### Git 别名

*(另请参阅下面的 `gac` 和 `gpush` 函数)*
| 别名        | 描述                                                                 | 原始命令 (示例)      |
| :---------- | :------------------------------------------------------------------- | :------------------------------ |
| `gcl`       | Git 克隆一个仓库。                                                     | `git clone`                     |
| `ga`        | Git 添加指定文件。                                                     | `git add`                       |
| `gaa`       | Git 添加当前目录下的所有更改。                                         | `git add .`                     |
| `gad`       | Git 添加仓库中的所有更改 (已跟踪和未跟踪的)。                            | `git add -A`                    |
| `gc`        | Git 提交并附带消息: `gc "My message"`                                  | `git commit -m`                 |
| `gca`       | Git 添加所有已跟踪文件并提交: `gca "My message"`                       | `git commit -a -m`              |
| `gco`       | Git 检出一个分支或提交。                                               | `git checkout`                  |
| `gcb`       | Git 创建并检出一个新分支。                                             | `git checkout -b`               |
| `gb`        | Git 列出分支。                                                         | `git branch`                    |
| `gbd`       | Git 删除一个分支。                                                     | `git branch -d`                 |
| `gbD`       | Git 强制删除一个分支。                                                 | `git branch -D`                 |
| `gs`        | Git 状态 (短格式，带分支信息)。                                        | `git status -sb`                |
| `gst`       | Git 状态。                                                             | `git status`                    |
| `gd`        | Git diff (显示更改)。                                                  | `git diff`                      |
| `gds`       | Git diff (显示暂存区的更改)。                                           | `git diff --staged`             |
| `gp`        | Git push (简单形式，通常需要更多参数)。                                 | `git push`                      |
| `gpo`       | `gpush` 函数的别名 (默认推送当前分支到 origin)。                         | `gpush` (见函数部分)            |
| `gpl`       | Git pull。                                                             | `git pull`                      |
| `gplr`      | Git pull 并变基 (rebase)。                                             | `git pull --rebase`             |
| `glg`       | Git log (单行、图形、修饰、所有)。                                     | `git log --oneline --graph --decorate --all` |
| `glg_`      | Git log (另一种详细且带颜色的视图)。                                   | `git log --color --graph --pretty=format:"%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset" --abbrev-commit` |
| `gignore`   | 告知 Git 忽略对某个文件的跟踪更改: `gignore <file>`                    | `git update-index --assume-unchanged` |
| `gunignore` | 告知 Git 恢复对某个文件的跟踪更改: `gunignore <file>`                  | `git update-index --no-assume-unchanged` |
| `gundo`     | 撤销最后一次提交，保留工作目录中的更改。                                 | `git reset HEAD~`               |
| `gclean`    | 从 Git 工作树中移除未跟踪的文件和目录 (小心操作！)。                     | `git clean -fd`                 |
| `ghprc`     | GitHub CLI: 创建拉取请求 (如果安装了 `gh`)。                           | `gh pr create`                  |
| `ghprl`     | GitHub CLI: 列出拉取请求 (如果安装了 `gh`)。                           | `gh pr list`                    |
| `ghprv`     | GitHub CLI: 查看拉取请求 (如果安装了 `gh`)。                           | `gh pr view`                    |
| `ghrepoweb` | GitHub CLI: 在网页浏览器中打开当前仓库 (如果安装了 `gh`)。               | `gh repo view --web`            |

-----

### Python 环境别名 (Conda & uv)

| 别名        | 描述                                                                 | 原始命令 (示例)      |
| :---------- | :------------------------------------------------------------------- | :------------------------------ |
| `ca`        | Conda 激活环境: `ca <env_name>`                                      | `conda activate`                |
| `cdx`       | Conda 停用环境。                                                       | `conda deactivate`              |
| `clse`      | Conda 列出环境。                                                       | `conda env list`                |
| `cenv`      | Conda 列出环境 (备选)。                                                | `conda env list`                |
| `cins`      | Conda 安装包: `cins <pkg_name>`                                      | `conda install`                 |
| `cunins`    | Conda 卸载包: `cunins <pkg_name>`                                    | `conda uninstall`               |
| `ccreate`   | Conda 创建新环境 (`conda create -n` 的前缀)。                          | `conda create -n`               |
| `csearch`   | Conda 搜索包。                                                         | `conda search`                  |
| `cupc`      | Conda 更新 conda 本身。                                                | `conda update conda`            |
| `cupall`    | Conda 更新当前环境中的所有包。                                         | `conda update --all`            |
| `crmenv`    | Conda 移除一个环境: `crmenv <env_name>`                                | `conda env remove -n`           |
| `uvv`       | `uv`: 创建虚拟环境 (如果安装了 `uv`): `uvv <env_name>`                 | `uv venv`                       |
| `uva`       | `uv`: 激活常见的 `.venv` (如果安装了 `uv` 且 venv 存在)。              | `source .venv/bin/activate`     |
| `uvi`       | `uv`: 安装包 (如果安装了 `uv`): `uvi <pkg_name>`                       | `uv pip install`                |
| `uvu`       | `uv`: 卸载包 (如果安装了 `uv`): `uvu <pkg_name>`                       | `uv pip uninstall`              |
| `uvl`       | `uv`: 列出已安装的包 (如果安装了 `uv`)。                               | `uv pip list`                   |
| `uvf`       | `uv`:冻结已安装的包 (类似 `pip freeze`, 如果安装了 `uv`)。              | `uv pip freeze`                 |
| `uvsync`    | `uv`: 从 requirements 文件同步环境 (如果安装了 `uv`)。                 | `uv pip sync`                   |
| `uvtree`    | `uv`: 显示依赖树 (如果安装了 `uv`)。                                   | `uv pip tree`                   |
| `uvcompile` | `uv`: 将 `requirements.txt` 编译为 `requirements.lock` (如果安装了 `uv`)。 | `uv pip compile`                |

-----

### Tmux 别名 (终端复用器)

*(另请参阅下面的 `t` 函数以进行全面的会话管理)*

| 别名        | 描述                                                                 | 原始命令 (示例)                      |
| :---------- | :------------------------------------------------------------------- | :---------------------------------------------- |
| `tl`        | 列出活动的 tmux 会话。                                                 | `tmux ls`                                       |
| `ta`, `tat` | 附加到指定会话: `ta <session_name>`                                  | `tmux attach-session -t`                        |
| `tn`        | 创建一个新的命名会话: `tn <session_name>`                              | `tmux new-session -s`                           |
| `td`        | 分离当前客户端 (在 tmux **内部**使用)。                                | `tmux detach-client`                            |
| `tkill`     | 杀死指定会话: `tkill <session_name>`                                 | `tmux kill-session -t`                          |
| `tkillsrv`  | 杀死 tmux 服务器及所有附加会话 (小心操作！)。                            | `tmux kill-server`                              |
| `tnw`       | **在 tmux 内部:** 新建窗口 (在当前窗格路径下打开)。                     | `tmux new-window -c "#{pane_current_path}"`     |
| `trns`      | **在 tmux 内部:** 重命名当前会话: `trns <new_name>`                      | `tmux rename-session`                           |
| `trnw`      | **在 tmux 内部:** 重命名当前窗口: `trnw <new_name>`                      | `tmux rename-window`                            |
| `tsph`      | **在 tmux 内部:** 水平分割窗格 (新窗格在当前路径下)。                    | `tmux split-window -h -c "#{pane_current_path}"`|
| `tspv`      | **在 tmux 内部:** 垂直分割窗格 (新窗格在当前路径下)。                    | `tmux split-window -v -c "#{pane_current_path}"`|
| `tsnxt`     | **在 tmux 内部:** 转到下一个窗口。                                       | `tmux next-window`                              |
| `tsprev`    | **在 tmux 内部:** 转到上一个窗口。                                       | `tmux previous-window`                          |
| `tslw`      | **在 tmux 内部:** 转到最后（上次选择的）窗口。                           | `tmux last-window`                              |
| `tkw`       | \*\*在 tmux 内部:\*\*杀死当前窗口。                                          | `tmux kill-window`                              |
| `tkp`       | \*\*在 tmux 内部:\*\*杀死当前窗格。                                          | `tmux kill-pane`                                |

-----

### 通用函数

| 函数名          | 描述                                                                 | 使用示例                                        |
| :-------------- | :------------------------------------------------------------------- | :--------------------------------------------------- |
| `mkcd`          | 创建一个目录（及任何父目录）然后进入该目录。                             | `mkcd my_new_project`                                |
| `findf`         | 在当前或指定目录中按名称查找文件 (不区分大小写)。                         | `findf report.txt` 或 `findf image.jpg ./docs`       |
| `grepdir`       | 在当前/指定目录的文件中递归搜索文本模式。                                | `grepdir "API_KEY"` 或 `grepdir "TODO" "*.py"`       |
| `backupfile`    | 创建文件或目录的带时间戳备份 (例如 `item.timestamp`)。                  | `backupfile important.doc` 或 `backupfile project_folder` |
| `extract`       | 解压各种存档类型 (tar.gz, zip, rar, 等)。                             | `extract archive.zip` 或 `extract data.tar.bz2`      |
| `lsbig`         | 列出当前目录中 N 个最大的文件/目录 (按大小) (默认 N=10)。                | `lsbig` 或 `lsbig 5`                               |
| `lstree`        | 递归列出文件/目录至 N 层 (如果 `tree` 命令可用)。                        | `lstree` (2层) 或 `lstree 3 ./src`               |
| `conn`          | 快速 SSH 到主机 (如果未指定用户，则使用当前用户)。                       | `conn myserver` 或 `conn user@another.server.com`    |

-----

### Git 函数

| 函数名      | 描述                                                                 | 使用示例                                        |
| :---------- | :------------------------------------------------------------------- | :--------------------------------------------------- |
| `gac`       | Git: 添加所有更改 (`git add -A`) 并使用提供的消息提交。                 | `gac "实现了新的登录功能"`                |
| `gpush`     | Git: 将当前或指定分支推送到 `origin`。                                 | `gpush` 或 `gpush feature-branch`                    |

-----

### 网络与代理函数

| 函数名          | 描述                                                                 | 使用示例                                        |
| :-------------- | :------------------------------------------------------------------- | :--------------------------------------------------- |
| `setproxyhost`  | **(需配置)** 设置 HTTP/HTTPS/FTP 代理环境变量。                       | `setproxyhost myproxy.server.com 8080 [user] [pass]` |
| `unsetproxy`    | 取消设置 HTTP/HTTPS/FTP 代理环境变量。                                 | `unsetproxy`                                         |
| `setsocks`      | **(需配置)** 设置 SOCKS5 代理环境变量 (使用 `ALL_PROXY`)。             | `setsocks my.socks.server 1080 [user] [pass]`        |
| `unsetsocks`    | 取消设置 SOCKS5 代理环境变量。                                         | `unsetsocks`                                         |

-----

### 软件管理与实用工具函数

| 函数名          | 描述                                                                 | 使用示例                                        |
| :-------------- | :------------------------------------------------------------------- | :--------------------------------------------------- |
| `get_anaconda`  | 下载指定的 Anaconda 安装脚本并使其可执行。                             | `get_anaconda` 或 `get_anaconda ~/my_installers`     |
| `emptytrash`    | **回收站管理:** 交互式永久删除 `~/.Trash` 中的所有项目。                 | `emptytrash`                                         |
| `cleartrash`    | `emptytrash` 的别名。                                                  | `cleartrash`                                         |

-----

### Python 环境管理函数

| 函数名          | 描述                                                                 | 使用示例                                        |
| :-------------- | :------------------------------------------------------------------- | :--------------------------------------------------- |
| `mkcenv`        | 创建一个 Conda 环境并可选地安装包。                                    | `mkcenv myenv python=3.9 numpy pandas`               |
| `mkstdvenv`     | 创建一个标准的 Python 虚拟环境 (使用 `python3 -m venv`)。               | `mkstdvenv .myenv` 或 `mkstdvenv` (创建 `.venv`)  |
| `mkuvenv`       | 使用 `uv` 创建 Python 虚拟环境 (如果安装了 `uv`)。                     | `mkuvenv .my_uv_env` 或 `mkuvenv` (创建 `.venv`)  |
| `act`           | 激活 Python 虚拟环境 (如果安装了 `uv`，也通用)。                       | `act` (尝试 `.venv`, `venv`, `env`) 或 `act myenv`    |

-----

### 特定工具辅助函数 (带安装提示)

| 函数名      | 描述                                                                 | 使用示例                                        |
| :---------- | :------------------------------------------------------------------- | :--------------------------------------------------- |
| `t`         | **Tmux:** 启动新会话或附加到现有 tmux 会话。如果 tmux 未安装则提示安装。处理在 tmux 内外等多种情况。 | `t` (智能附加/新建) 或 `t mysession`                   |
| `nvt`       | **Nvitop:** 运行 `nvitop` 进行 NVIDIA GPU 监控。如果 nvitop 未安装则提示安装。 | `nvt` 或 `nvt -m` (将参数传递给 nvitop)         |

## ⚠️ 关键：代理配置 ⚠️

如果您计划使用代理辅助函数 (`setproxyhost`, `setsocks`)：

  * 您 **必须** 打开 `~/.bash_aliases` 文件。
  * 在此文件中找到 `setproxyhost` 和 `setsocks` 函数。
  * 将占位符值（如 `YOUR_PROXY_HOST`, `YOUR_PROXY_PORT`, `YOUR_USERNAME`, `YOUR_PASSWORD`）替换为您的 **实际代理服务器详细信息和凭据**。
  * 调整 `setproxyhost` 中的 `no_proxy` 列表，以包含您需要直接访问而无需通过代理的任何内部域。

## 自定义

### 添加或修改命令

要添加新的别名或函数，或修改现有的：

1.  使用您喜欢的文本编辑器打开 `~/.bash_aliases` (例如, `nano ~/.bash_aliases`)。
2.  进行更改。
3.  保存文件。
4.  应用更改 (见下面的“生效方法”)。

## 生效方法

要使对 `~/.bash_aliases` 的任何更改在当前终端会话中生效，您必须重新加载您的 `~/.bashrc` 文件：

```bash
source ~/.bashrc
```

或者，您可以关闭当前终端窗口并打开一个新的。新的终端会话将自动加载更新后的配置。
