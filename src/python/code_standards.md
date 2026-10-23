# 代码规范

实际写代码时需要方便他人以及未来的自己看懂代码，因此除了代码能跑之外，最好还能遵守一些规范。

## Pylint 

Pylint 是一款插件，作用是:

- 检查潜在错误
- 检查代码风格
- 给出评分和报告

<!-- ![Pylint 使用界面](./image/Pylint.png) -->

示例代码

```python
"""这是一个统计正数和负数个数的模块。"""

def count_numbers():
    """统计用户输入的正数和负数个数，输入0结束。"""
    pos_num = 0
    neg_num = 0

    while True:
        try:
            user_input = input("请输入一个整数（输入0结束）：")
            value = int(user_input)
        except ValueError:
            print("输入无效，请输入一个整数。")
            continue

        if value == 0:
            break
        if value < 0:
            neg_num += 1
        else:
            pos_num += 1

    print(f"正数个数：{pos_num}，负数个数：{neg_num}")

if __name__ == '__main__':
    count_numbers()

```

- 在模块、函数、类开头用一组或三组双引号编写一段说明文字，称为文档字符串 (docstring)；
- 注意变量命名，小写为变量，大写为常量；
- 一行不要过多字符，结尾不要跟空格。
- 脚本末尾保留一行空行。

右下角会有 Pylint 对于代码的评分，满分 10 分。

## 类型注释

```python
def average(scores: list[float]) -> float:
    """返回平均分。"""
    return sum(scores) / len(scores)

def greet(name: str, times: int = 1) -> str:
    return f"你好，{name}！" * times
```

这段代码中的 `-> float` 和 `: int` 等就是类型注释（之所以说是注释，是因为纵使实际传入类型与该注释不匹配，也不会影响代码运行），可以方便人和静态代码分析看数据类型。
