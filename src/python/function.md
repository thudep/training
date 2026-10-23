# 控制流语句与函数

## `if` 语句

Python 中的**条件结构**用 `if` 语句实现，与之配套的有 `else` 和 `elif` 。

`if` 语句的结构为

```python
if 条件:
    # 缩进的代码: 条件成立 (True) 时才执行
else:
    # 缩进的代码: 条件不成立 (False) 时才执行
```

注意在 `if` 后面的条件写完后打冒号，在进到下一行后缩进4个空格。一般在冒号打完后会自动缩进。按 `tab` 键也可以缩进4个空格。

如果选择较多，则使用 `elif`

```python
score = int(input("请输入成绩:"))
if score >= 90:
    print("优秀")
elif score >= 60:
    print("及格")
else:
    print("不及格")
```

## 循环语句

### `for` 语句

如果一个程序需要跑很多遍，就可以借助**循环结构**。Python 中的循环结构可由两种语句实现，此处先讲 `for` 语句。

`for` 语句的结构为

```python
for 变量 in 序列:
    要重复执行的代码
```

常用序列有列表，元组以及 `range` 等。

`range(a, b, c)` 就是从 `a` 到 `b - 1` ，间距为 `c` 的等差数列，其中起始数默认为 `a = 0`，间距默认为 `c = 1`，所以 `range(n)` 就表示从 `0` 到 `n - 1` 的等差数列。注意 `range` 函数包括起始数，但是不包含终止数。例如 `range(5)` 就是 `0, 1, 2, 3, 4` ， `range(0,5,2)` 是 `0, 2, 4` 。

现在可以利用循环语句依次输出 `range` 函数中的数，也可以输出列表、字符串、元组等内部的元素

```python
for i in range(5):
    print(i)
```

```python
for score in [85, 92, 78]:
    print(score)
```

```python
for ch in "abc":
    print(ch)
```

```python
student = {"name": "小明", "age": 18}
for key, value in student.items():
    print(key, value)
```

用 `for` 可以做到计数、累加等功能，如计算从 1 加到 100 

```python
total = 0
for i in range(1, 101):
    total += i
print("1 到 100 的和为:", total)
```

### `while` 语句

Python 中另一种实现循环的语句为 `while` 。

`while` 语句的一般结构为：

```python
while 条件:
    条件成立时一直执行的代码
```

下面是一个使用 `while` 语句的案例，假定你有 1000 元，每年投资收益为 5% ，经过多少年后钱会翻番？

该问题可以用以下程序解答

```python
money = 1000.0
n = 0
while money < 2000.0:
    money = money * 1.05
    n = n + 1
print(f"我的钱将会在 {n} 年内翻番")
```

### `break` 和 `continue`

`break` 语句的功能：
* 用来从循环体内跳出循环体，即提前结束循环，接着执行循环下面的语句；
* 只跳出最近的循环。

使用案例：输入一组整数，当输入 `0` 时表示输入结束，然后统计这组整数中正数和负数的个数。

```python
pos_num = 0
neg_num = 0
while True:
    value = int(input())
    if value == 0:
        break
    elif value < 0:
        neg_num += 1
    else:
        pos_num += 1
print("正数个数:", pos_num, "\n负数个数:", neg_num)
```

注意：若有多个循环， `break` 只跳出最近的循环。

`continue` 语句的功能为结束本次循环，即跳过循环体中尚未执行的语句，直接回到循环条件的判别。

```python
total = 0
for i in range(10):
    if i == 4:
        break
    if i == 2:
        continue
    total += i
print(total)
```

### 死循环

若循环时代码出问题导致无法跳出循环就会不停地执行，导致代码陷入死循环，如：

```python
i = 0
while i < 5:
    print(i)
```

触发死循环时，可以按 `Ctrl + C` 终止进程。

## 函数

Python 中可以用 `def` 提前定义函数，等需要时直接调用，结构为

```python
def 函数名(参数列表):
    函数体
    return 返回值
```

- 参数可以有 `0` 个或多个；
- `return` 可以没有（返回 `None`），可以返回多个值（元组）；
- 函数不调用就不执行，定义只是“登记”。

```python
def greet(name):
    return f"你好, {name}!"
print(greet("小明"))
```

### pass 占位符

如果一段代码写了一部分之后暂时不写，可以用 `pass`，表示跳过这部分代码。
