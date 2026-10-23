# Python 安装

Python 是一种由 Guido van Rossum 于1990年代初设计的高级**解释型**编程语言。

## Python 的安装

以下给出在 Linux (Ubuntu/Debian) 环境下安装 Python 的方法，对于其他系统请自行查找方案安装。

输入以下命令即可完成安装

```bash
sudo apt update
sudo apt install python3
```

完成后，可以输入以下命令检测安装是否成功

```bash
python3 --version
```

若出现（版本号可能有差异）

```
Python 3.13.5
```

则为安装成功。为了使用 `python` 而非 `python3` 命令运行脚本，可以输入以下命令安装一个小工具

```bash
sudo apt install python-is-python3
```

## VS Code 扩展的安装

在 VS Code 侧边打开扩展商店，搜索并安装 `Python Extension Pack`，之后就可以在 VS Code 中更方便地编写和运行 Python 代码。

## IPython 的安装与使用

### 安装 IPython

相比于 Python，IPython 的交互感更强，本课程将使用 IPython 演示交互式场景。输入以下命令完成安装

```bash
sudo apt install ipython3
```

同样可使用如下命令检测安装是否成功

```bash
ipython3 --version
```

### 初步使用 IPython

在终端中输入

```bash
ipython3
```

弹出
```
In [1]:
```

即为成功进入 IPython ，此时可输入基础指令尝试使用 IPython 。

对于仅有一行的代码，输入完成后按下 Enter 键就会运行，如

```python
1 + 1
```

如果代码有多行，如 for、while、if 等语句，按下 Enter 键后不会自动运行，而是继续输入下一行的内容，如果想运行代码需要按两下 Enter 

```python
for i in range(1, 10):
    print(i)
```
