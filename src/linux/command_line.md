## 命令行基础

### 什么是Shell？

Shell 是 Linux 系统的“命令解释器”，它接收用户输入的命令，解析后传递给内核执行，并返回结果。常见的 Shell 类型包括：

- **Bash（Bourne-Again Shell）**：最广泛使用的 Shell，默认预装在大多数 Linux 发行版（如 Ubuntu、CentOS）中。
- **Zsh（Z Shell）**：功能更强大，支持主题、插件和自动补全，适合高级用户。
- **Fish（Friendly Interactive Shell）**：以易用性和交互体验为卖点，默认启用语法高亮和自动建议。

本次技术培训我们使用 **Bash**。其他的 Shell 大家可以自行搜索了解。

### 命令行基本语法

```
命令 [选项] [参数]
```

- **命令（Command）**：要执行的操作，如 `ls`（列出文件）、`cd`（切换目录）。
- **选项（Options）**：调整命令行为，通常以 `-`（短选项，如 `-l`）或 `--`（长选项，如 `--help`）开头。
- **参数（Arguments）**：命令作用的对象，如文件名、目录路径。

**示例**：

```
ls -l /home/user  # 命令 ls，选项 -l（长格式），参数 /home/user（目标目录）
```

### 文件结构

GNU/Linux 是以什么形式组织文件的呢？

#### 根目录，家目录，文件路径

+ 对于 Windows 用户，我们最熟悉的就是各种盘，C 盘，D 盘，E 盘等等；每一个**盘符**里由文件夹和各种文件组合成树状结构。这样的模式叫做**多根逻辑存储结构**。
+ 对于 macOS 和 GNU/Linux 用户，我们最熟悉的则是一个 `/` 下面放了很多文件夹，之后在这个 `/` 下面由很多的文件和文件夹组成一个树状结构。这样的模式叫做单根逻辑存储结构。

对于文件在文件系统中所处的位置，我们可以用**路径**进行描述。

+ 根目录：顾名思义，就是在这个文件结构最底层的目录，即 `/`，可以理解为你的硬盘

```bash
# GNU/Linux or MacOS etc.
/

# Windows
C:/
D:/
...
```

+ 家目录：
  + **用户**：事实上，你的电脑是可以由多人同时使用的，这些叫做**用户**，每一个用户都有自己的目录，在 GNU/Linux 中使用 `~/` 表示。
  + **Root 用户**：在大型服务器中，会开很多用户，为了不让这些用户乱搞事，我们会限制他们的一些权限（比如文件的写入、读取、执行），而在所有用户至上有一个超级用户，拥有操纵计算机资源的最高权限。

```bash
# 家目录
~/

# 绝对路径（见下）
/home/name/
```

+ 绝对路径：路径从盘符/根目录开始

```bash
/home/name/academic
```

+ 相对路径：描述某一个文件相对于你当前所处路径的位置
  + `./` 当前文件夹
  + `../` 前一级的文件夹
  + `../..` 上上级别，以此类推

```bash
# 我所处的目录（以绝对路径表示）
/home/name/academic

# 放在 /home/name 下的文件夹 life 的相对路径可以写成
../life

# 有一个放在 academic 文件夹中的文件 linux.pdf 可以写成
./linux.pdf                   # 相对路径
/home/name/academic/linux.pdf # 绝对路径
/linux.pdf                    # 错误
```

### 文件与目录管理

#### `ls`：列出目录内容

**功能**：显示指定目录下的文件和子目录（默认显示当前目录）

**常用选项**：

- `-l`：长格式显示（权限、所有者、大小、修改时间等）。
- `-a`：显示隐藏文件（以 `.` 开头的文件）。
- `-h`：以“人类可读”格式显示文件大小（如 KB、MB）。
- `-t`：按修改时间排序（最新文件在前）。

**示例**：

```bash
ls -l          # 长格式列出当前目录文件
ls -ah ~       # 显示家目录下所有文件（含隐藏文件），并以人类可读格式显示大小
ls -lt /var/log  # 按修改时间排序，列出 /var/log 目录下的日志文件
```

#### `cd`：切换目录

**功能**：切换当前工作目录。

**常用参数**：

- `cd 目录路径`：切换到指定目录（绝对路径或相对路径）。
- `cd ~` 或 `cd`：切换到当前用户的家目录（如 `/home/user`）。
- `cd -`：切换到上一次所在目录。
- `cd ..`：切换到父目录。

**示例**：

```bash
cd /tmp        # 切换到 /tmp 目录（绝对路径）
cd ../documents  # 切换到当前目录的父目录下的 documents 子目录（相对路径）
cd ~/Downloads  # 切换到 Downloads 目录（家目录的子目录）
cd -           # 回到上一次所在目录（如从 /tmp 回到之前的目录）
```

#### `pwd`：显示当前目录路径

**功能**：打印当前工作目录的绝对路径。

**示例**：

```bash
pwd  # 输出如 /home/user
```

#### `touch`：创建空文件或更新文件时间戳

**功能**：若文件不存在，创建空文件；若已存在，更新其访问和修改时间戳。

**示例**：

```bash
touch note.txt         # 创建空文件 note.txt
touch -d "2026-01-01" note.txt  # 修改 note.txt 的时间戳为 2026 年 1 月 1 日
```

#### `mkdir`：创建目录

**功能**：创建新目录。

**选项**：

- `-p`：递归创建多级目录（若父目录不存在，自动创建）。

**示例**：

```bash
mkdir new_dir          # 在当前目录创建 new_dir
mkdir -p project/src   # 递归创建 project 和其子目录 src（即使 project 不存在）
```

#### `rm`：删除文件/目录

**功能**：删除文件或目录。

**选项**：

- `-f`：强制删除（忽略不存在的文件，不提示）。
- `-r`：递归删除目录及其内容（删除目录时必须加此选项）。
- `-i`：删除前提示确认（安全选项）。

**示例**：

```bash
rm note.txt            # 删除文件 note.txt（若文件不存在，会提示错误）
rm -f old.log          # 强制删除 old.log（无提示）
rm -ri project         # 递归删除 project 目录，删除前逐一确认
```

#### `cp`：复制文件/目录

**功能**：复制文件或目录。

**选项**：

- `-r`：递归复制目录（复制目录时必须加此选项）。
- `-i`：目标文件已存在时提示覆盖。
- `-v`：显示复制过程（详细模式）。

**示例**：

```bash
cp file.txt /tmp/      # 复制 file.txt 到 /tmp 目录
cp -r src/ dest/       # 递归复制 src 目录到 dest 目录（若 dest 不存在，会创建 dest 并复制内容）
cp -iv *.jpg ~/photos/ # 复制所有 .jpg 文件到 photos 目录，显示过程并提示覆盖
```

#### `mv`：移动/重命名文件/目录

**功能**：移动文件/目录，或重命名（目标路径与原路径同目录时为重命名）。

比较操作系统

**示例**：

```bash
mv old.txt new.txt     # 将 old.txt 重命名为 new.txt
mv report.pdf ~/docs/  # 将 report.pdf 移动到 ~/docs 目录
mv /tmp/logs/ ./       # 将 /tmp/logs 目录移动到当前目录
```

#### `cat`/`more`/`less`：查看文件内容

- **`cat`**：一次性显示整个文件内容（适合短文件）。

  ```bash
  cat README.md  # 显示 README.md 的全部内容cat file1.txt file2.txt > combined.txt  # 合并两个文件内容到 combined.txt
  ```

- **`more`**：分页显示文件内容（按 Enter 翻行，按 Space 翻页，`q` 退出）。

  ```bash
  more /var/log/syslog  # 分页查看系统日志
  ```

- **`less`**：功能更强的分页工具（支持上下滚动、搜索，`/关键词` 搜索，`q` 退出）。

  ```bash
  less /etc/profile  # 查看环境配置文件，支持交互式操作
  ```

### 用户与权限管理

#### `sudo`：以管理员权限执行命令

**功能**：普通用户通过 `sudo` 临时获取 root 权限执行命令。

**示例**：

```bash
sudo apt update  # 以 root 权限更新软件包列表（Ubuntu/Debian 系统）
```

![删库跑路](./img/bomb.gif)

> [!WARNIng]
>
> `rm -rf /*` 是极其危险的命令，会删除系统根目录下所有文件，导致系统崩溃！**永远不要在 root 用户下执行此命令**。

#### `chmod`：修改文件权限

**功能**：修改文件/目录的读（r）、写（w）、执行（x）权限，针对所有者（u）、所属组（g）、其他用户（o）。

**权限表示**：

- 数字法：r=4，w=2，x=1，三者之和为权限值（如 `755` 表示所有者 rwx，组和其他 rx）。
- 符号法：`u+rwx`（给所有者添加 rwx）、`o-w`（移除其他用户的写权限）。

**示例**：

```bash
chmod 755 script.sh  # 所有者 rwx，组和其他 rx（脚本可执行）
chmod u+x file.txt   # 仅给所有者添加执行权限
chmod o-rw secret.txt  # 移除其他用户对 secret.txt 的读和写权限
```

#### `chown`：修改文件/目录的所有者与组

**功能**：修改文件/目录的所有者和所属组，常用于“转移文件归属权”或“统一目录的用户组”。

**基本语法**：chown [新所有者:新所属组] 文件/目录

+ 只改所有者：`chown new_owner filename`
+ 同时改所有者和组：`chown new_owner:new_group filename`
+ 递归改目录（含子文件 / 子目录）：`chown -R new_owner:new_group /path/to/dir`

**示例**：

```bash
chown john file.txt                # 将 file.txt 的所有者改为用户 john
chown john:developers file.txt     # 将 file.txt 的所有者改为 john，所属组改为 developers
chown -R dev:devteam /project/code # 将 /project/code 目录（包括所有子文件/子目录）的所有者改为 dev, 所属组改为 devteam
```

### 网络命令

#### `ping`：测试网络连通性

**功能**：向目标主机发送 ICMP 数据包，检测与另一个主机之间的网络连接。

**选项**：

- `-c 次数`：指定发送数据包数量（默认无限发送）。
- `-4`：强制使用 IPv4 协议。
- `-6`：强制使用 IPv6 协议。

**示例**：

```bash
ping -c 4 baidu.com  # 向百度发送 4 个数据包，测试连通性
ping -4 baidu.com    # 强制使用 IPv4 协议向百度发送数据包
```

#### `ip`：查看/配置网络接口（替代 `ifconfig`）

**功能**：现代 Linux 推荐使用 `ip` 命令管理网络接口，替代传统的 `ifconfig`。

**常用子命令**：

- `ip addr` 或 `ip a`：查看所有网络接口的 IP 地址。
- `ip link set eth0 up/down`：启用/禁用网卡 eth0。

**示例**：

```bash
ip a  # 输出所有网卡信息
```

#### `curl`/`wget`：下载文件或测试 HTTP 请求

- **`curl`**：发送 HTTP/HTTPS 请求，支持下载、提交表单等。

  ```bash
  curl https://example.com  # 获取网页内容并打印到终端
  curl -O https://example.com/file.iso  # 下载文件（保存为 file.iso）
  ```

- **`wget`**：专注于文件下载，支持断点续传。

  ```bash
  wget https://example.com/largefile.zip  # 下载文件，支持中断后继续（-c 选项）wget -c https://example.com/largefile.zip  # 断点续传未下载完成的文件
  ```

## 一些技巧

#### Tab 自动补全

#### `history`：查看和操作终端中输入的命令历史

**选项**：

+ `-c`：清除所有历史记录
+ `-d`：删除指定位置的历史记录

**示例**：

```bash
history         # 查看完整历史记录
history 10      # 查看最近 10 条命令
history -c      # 清除所有历史记录
history -d 1010 # 删除第 1010 条命令

#快速执行历史命令
!n      # 执行历史记录中第 n 条命令
!!      # 执行上一条命令
!string # 执行最近一条以 string 开头的命令
```

此外，在 shell 中按上下方向键也可以查看最近输入的命令。

#### 管道（`|`）：连接多个命令

我们发现有时需要将一个程序的输出交给另一个程序进行处理，这样的操作可以通过**管道 (pipe) **完成。

目前在任何一个 shell 中，都可以使用 `|` 连接两个命令，shell 会将前一个命令的标准输出作为后一个命令的标准输入，实现进程间的通信。

**示例**：

```bash
hitory | grep ls  # 列出历史记录中包含 "ls" 的命令
ls -l | grep new  # 列出当前目录中包含 "new" 的文件和目录
```

#### 重定向：控制输入/输出流向

- **`>`**：覆盖写入文件（若文件存在，清空原有内容）。
- **`>>`**：追加写入文件（保留原有内容，新内容加在末尾）。
- **`2>`**：重定向标准错误（stderr）到文件（`2` 代表 stderr 的文件描述符）。
- **`<`**：从文件读取输入（替代键盘输入）。

**示例**：

```bash
ls -l > file_list.txt  # 将 ls -l 的输出写入 file_list.txt（覆盖原有内容）
echo "新内容" >> notes.txt  # 追加 "新内容" 到 notes.txt
command 2> error.log  # 将命令的错误输出写入 error.log（不影响 stdout）
sort < unsorted.txt > sorted.txt  # 从 unsorted.txt 读取内容，排序后写入 sorted.txt
```

#### 环境变量：系统级/用户级配置

环境变量是全局变量，控制 Shell 和程序的行为（如 `PATH` 决定命令搜索路径）。

**常用操作**：

- 查看变量：`echo $变量名`。
- 设置临时变量：`变量名=值`（仅当前 Shell 有效）。
- 导出全局变量：`export 变量名=值`（子进程可继承）。
- 永久生效：写入 Shell 配置文件（如 `.bashrc`、`.zshrc`），需执行 `source ~/.bashrc` 或重启终端生效。

**示例**：

```bash
echo $PATH  # 输出命令搜索路径，如 /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin...export PATH=$PATH:/home/user/bin  # 将 ~/bin 目录添加到 PATH（临时生效）# 永久添加：编辑 ~/.bashrc，加入一行 export PATH=$PATH:/home/user/bin，然后 source ~/.bashrc
```

#### 别名（alias）：简化常用命令

别名用于将复杂命令映射为简短名称，提升效率。

**示例**：

```bash
alias ll='ls -l'  # 临时设置 ll 为 ls -l 的别名
ll  # 等效于 ls -l 

# 永久生效：将别名写入 ~/.bashrc
echo "alias ll='ls -l'" >> ~/.bashrc
source ~/.bashrc  # 立即生效
```

#### `find`：在指定目录下查找文件和目录

+ **通配符**：通配符是 Shell 提供的特殊字符，用于匹配文件或目录名称中的字符模式，实现快速批量操作。当用户在命令行中输入包含通配符的表达式时，Shell 会先将其扩展为符合模式的文件名列表，再将结果传递给具体命令，从而简化文件管理操作。
  + **星号（`*`）**：匹配任意数量的字符（包括 0 个字符），但不匹配路径分割符 `/`。
  + **问号（`?`）**：匹配单个字符，占据文件名中的一个位置。
  + 其他的还有方括号、否定匹配、大括号等，大家可以自行查阅了解。
+ **按文件名查找**：

```bash
find . -name file.txt # 查找当前目录下名为 file.txt 的文件
find . -name "*.c"    # 列出当前目录及子目录下所有后缀为 ".c" 的文件
find . -name ".*"     # 列出当前目录及子目录下的所有隐藏文件
```

#### man 与 --help

- `man`：查看命令的手册页（Manual），按 `q` 退出。

  ```bash
  man ls  # 查看 ls 命令的详细文档
  ```

- `--help`：快速查看命令的选项摘要（适合简单查询）。

  ```bash
  ls --help  # 输出 ls 的选项列表和简要说明
  ```

### 一些小工具

#### apt

apt（Advanced Packaging Tool）是一个在 Debian 和 Ubuntu 中的 Shell 前端软件包管理器。

apt 命令提供了查找、安装、升级、删除某一个、一组甚至全部软件包的命令，而且命令简洁而又好记。

apt 命令执行需要超级管理员权限(root)。

+ **查看一些可更新的包**：

```bash
sudo apt update
```

+ **安装指定的软件/多个软件包**：

```bash
sudo apt install <package_name> # 安装指定的软件
sudo apt install <package_1> <package_2> <package_3> # 安装多个软件包
```

+ **更新指定的软件**：

```bash
sudo apt update <package_name>
```

+ **删除软件包**：

```bash
sudo apt remove <package_name>
```

+ **列出已安装的包**：

```bash
apt list --installed
```

#### build-essential

Debian/Ubuntu 系统中的一个元包（meta‑package），用于一次性安装构建 C/C++ 程序所需的核心工具链，包括 gcc、g++、make、libc6-dev 和 dpkg-dev 等组件。

**安装**：

```bash
sudo apt update
sudo apt install build-essential
```

#### CMake

**CMake** 是个一个**开源的跨平台自动化建构系统**，用来管理软件建置的程序，并不依赖于某特定编译器，并可支持多层目录、多个应用程序与多个函数库。

CMake 通过使用简单的**配置文件 CMakeLists.txt**，自动生成不同平台的构建文件（如 Makefile、Ninja 构建文件、Visual Studio 工程文件等），简化了项目的编译和构建过程。

CMake 本身不是构建工具，而是**生成构建系统的工具**，它生成的构建系统可以使用不同的编译器和工具链。

关于 CMake 如何使用，大家可以自行查阅资料了解，我们后面介绍简单介绍另一种自动化建构软件 **make** 的用法。

**安装**：

```bash
sudo apt install cmake
```

#### make 与 Makefile

- 一个工程中的源文件不计其数，其按类型、功能、模块分别放在若干个目录中，Makefile 定义了一系列的规则来指定，哪些记录得先编译，哪些文件需要后编译，哪些文件需要重新编译，甚至于进行更复杂的能力运行
- **makefile**带来的好处就是 ——“**自动化编译**”，一旦写好，只需要一个 make 命令，整个工程完全自动编译，极大的提高了软件开发的效率。
- **make 是一条命令**，**makefile 是一个文件**，两个搭配使用，完成任务自动化构建。

**安装**：

```bash
sudo apt install make
```

#### tree

tree 命令用于以**树状图**列出目录的内容。

执行 tree 指令，它会列出指定目录下的所有文件，包括子目录里的文件。

**选项**：

+ `-a`：显示所有文件和目录。
+ `-L`：限制目录显示层级

**示例**：

```bash
tree      # 以树状图列出当前目录结构
tree -a   # 以树状图列出所有文件和目录
tree -L 2 # 以树状图列出当前目录结构，限制显示层级为 2
```

**安装**：

```bash
sudo apt install tree
```

#### curl / wget

用法前面介绍过了。

```bash
sudo apt install curl
sudo apt install wget
```

#### htop

+ **`top`**：top 命令是一个非常实用的工具，用于监控Linux系统的性能和运行状态。它可以实时显示系统中各个进程的资源占用情况，类似于Windows的任务管理器。

+ **`htop`**：htop 则是功能强大的交互式进程查看与系统监控工具，相比传统 top，它提供了**彩色界面**、**鼠标操作**、**进程树视图**等更直观的功能，非常适合实时监控和管理系统资源。

  界面主要分为以下几个部分：

  1. **顶部区域**：系统概览信息
     - CPU 使用率（按核心显示）
     - 内存使用情况
     - 交换空间使用情况
     - 系统运行时间和平均负载
  2. **中间区域**：进程列表
     - PID：进程 ID
     - USER：进程所有者
     - PRI：进程优先级
     - NI：nice 值
     - VIRT：虚拟内存使用量
     - RES：物理内存使用量
     - SHR：共享内存大小
     - S：进程状态（运行、睡眠等）
     - CPU%：CPU 使用率
     - MEM%：内存使用率
     - TIME+：CPU 时间
     - COMMAND：命令名称
  3. **底部区域**：功能键提示

**安装**：

```bash
sudo apt install htop
```

#### zip / unzip

`zip` 用于压缩文件或目录，生成跨平台兼容的 `.zip` 文件；`unzip` 用于解压缩。

**语法**：

```bash
zip [options] output.zip file1 file2 ...
```

**选项**：

+ `-r`：递归压缩目录及其子目录中的所有文件。
+ `-e`：为压缩文件设置密码保护。
+ `-x`：排除某些文件或目录，不进行压缩。
+ `-m`：压缩后删除原始文件。

**示例**：

```bash
zip archive.zip file.txt # 压缩单个文件
zip archive.zip file1 file2 file3 # 压缩多个文件
zip -r archive.zip directory/ # 递归压缩目录
zip -r archive.zip directory/ -x "*.log" # 排除特定文件
zip -m archive.zip file.txt # 压缩后删除原始文件
unzip archive.zip # 解压缩文件
```
