# 模块与代码架构

模块将相关的函数、类、常量及其他语句组织在独立的命名空间（namespace）中。


#### 自定义模块导入

例如，定义 `constant.py`；其中 `f` 是频率（Hz），`energy(f)` 返回能量（J）：

```
hbar = 1.054571817e-34
energy = lambda f: 2 * 3.141592653589793 * hbar * f
_internal = 42
```

在另一个文件main.py中，假设文件目录为

pythonLec/
├── main.py
└── constant.py

可以写

```
from constant import hbar, energy

print(hbar)
print(energy(1e15))
```



如果模块位于更深层的目录中，例如：

```
pythonLec/
├── main.py
└── physics/
    ├── __init__.py
    └── constants/
        ├── __init__.py
        └── constant.py
```



##### 方法一：绝对导入

```
from physics.constants.constant import hbar, energy
print(hbar)
print(energy(1e15))
```

其中：

- `physics`：顶层包（Package）。
- `constants`：子包（Subpackage）。
- `constant`：模块（Module），对应 `constant.py`。
- `hbar, energy`：模块中定义的名称。

`__init__.py` 用于标识和初始化常规 Python 包。现代 Python 也支持不包含该文件的命名空间包（Namespace Package）。



##### 方法二：相对导入

假设目录结构为：

```
pythonLec/
├── main.py
└── physics/
    ├── __init__.py
    ├── constant.py
    └── particles/
        ├── __init__.py
        └── neutrino.py
```

在 `neutrino.py` 中，可以使用：

```
from ..constant import hbar
```

其中：

- `.` 表示当前包。
- `..` 表示上一级包。

> [!CAUTION]
>
> 相对导入是相对于当前模块所属的包，而不是相对于终端的工作目录。



### 代码架构

##### 1. 基本原则

- 模块化（Modularity）：将程序划分为独立模块，每个模块负责特定功能。
- 高内聚、低耦合（High Cohesion, Low Coupling）：相关功能集中在一起，减少模块之间不必要的依赖。
- 关注点分离（Separation of Concerns）：将数据处理、核心计算、结果可视化等不同职责分开。
- 避免重复（DRY, Don't Repeat Yourself）：将重复逻辑封装为可复用的函数或类。
- 单一职责（Single Responsibility Principle）：每个模块或类应具有清晰、相对单一的职责。

##### 2. 一个典型的 Python 项目结构

以中微子数据分析软件MENUS为例：

```
menus/
├── README.md               # 项目说明
├── requirements.txt        # 项目配置与依赖
├── .gitignore              # Git 忽略规则
├── configs/
│   └── default.yaml        # 运行参数
├── data/                   # 数据文件
├── src/
│   └── particle_analysis/
│       ├── __init__.py
│       ├── io.py           # 数据读取
│       ├── physics.c       # 物理计算
│       ├── analysis.c      # 数据分析
│       └── plotting.py     # 结果可视化
├── scripts/
│   └── main.py     # 程序入口
└── tests/
    ├── test_physics.py     # 物理计算测试
    └── test_analysis.py    # 分析流程测试
```

各部分职责：

| 目录或文件       | 功能               |
| ---------------- | ------------------ |
| `src/`           | 核心程序代码       |
| `scripts/`       | 程序入口、任务脚本 |
| `configs/`       | 参数与配置         |
| `data/`          | 输入数据           |
| `tests/`         | 自动化测试         |
| `pyproject.toml` | 依赖和项目元信息   |
| `README.md`      | 使用方法与文档     |



#### `__init__.py`：包初始化文件

`__init__.py` 用于定义 Python 包的初始化行为和公共接口，可以为空，也可以导出常用函数、类和变量。

例如，目录结构为：

```
particle_analysis/
├── __init__.py
├── physics.py
└── analysis.py
```

在 `__init__.py` 中：

```
from .physics import calculate_energy
from .analysis import analyze_events

__all__ = ["calculate_energy", "analyze_events"]
```

其他文件便可以直接导入：

```
from particle_analysis import calculate_energy
```

而不必写：

```
from particle_analysis.physics import calculate_energy
```

- `.`：表示当前包，用于相对导入。
- `__all__`：指定 `from package import *` 时导出的名称。
- 执行时机：包首次导入时执行 `__init__.py`。



#### `main.py`程序主入口

主程序通过 `if __name__ == "__main__":` 提供入口：直接执行文件时调用 `main()`，被导入时不执行该条件块。

```
def main():
    print("Starting particle analysis...")
    # 读取数据
    # 执行分析
    # 保存结果

if __name__ == "__main__":
    main()
```

