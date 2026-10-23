## Python 安装

Python 是一种由 Guido van Rossum 于1990年代初设计的高级解释型编程语言。

### Python 的安装

进入 WSL 

输入
````powershell
sudo apt update
sudo apt install -y python3
````

安装完成后，输入
````powershell
python3 --version
````


若出现
````powershell
Python 3.13.5
````
则为下载完成

### iPython 的安装

相比于 Python , iPython 的交互感更强，本节课我将使用 iPython 演示

在 WSL 中输入
````powershell
sudo apt install -y ipython3
````

安装完成后输入
````powershell
ipython3 --version
````

若出现
````powershell
8.35.0
````
则为安装完成

#### iPython 扩展的安装

打开扩展商店，下载 Python Extension Pack

### 初步使用 iPython

#### 进入 iPython
在 WSL 中输入
````powershell
ipython3
````

弹出
````powershell
In[1]:
````
即为成功进入iPython

此时可输入基础指令尝试使用 iPython3

对于仅有一行的代码，输入完成后按下 enter 键就会运行，如
````python
1+1
````

如果代码有多行，如 for 等函数,按下 enter 键后不会自动运行，而是继续输入下一行的内容，如果想运行代码需要按两下 enter 

````python
for i in range (1,10):
    print(i)
````
