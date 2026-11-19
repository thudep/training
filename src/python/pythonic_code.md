# Pythonic代码

## Pythonic

指符合 Python 设计哲学的代码风格：简洁、清晰、可读，充分利用 Python 的语言特性，而不是照搬 C/C++ 或 Java 的写法。



下面是一些经典的 **Non-Pythonic vs. Pythonic** 对比。


### 1. 交换变量：序列解包

Python 支持通过多重赋值同时绑定多个变量

```
a = 10
b = 8
```

Non-Pythonic

```
temp = a
a = b
b = temp
```

Pythonic

```
a, b = b, a
```

输出：

```
print(a, b)
```

8,10

右侧表达式会先求值，再将结果分别赋给左侧变量，因此不需要临时变量



### 2. 条件判断：Truthiness

Python 允许直接使用对象的真假值（Truthiness）进行条件判断。

```
items = [1, 5, 7]

def process(items):
    return [i + 1 for i in items]
```

**Non-Pythonic**

```
if len(items) > 0:
    items = process(items)

print(items)
```

**Pythonic**

```
if items:
    items = process(items)

print(items)
```

输出：

```
[2, 6, 8]
```

空列表、空字典、空字符串、数字 `0` 和 `None` 等在布尔判断中都被视为 `False`。



### **3. 多重条件判断：字典映射**

对于固定值与固定结果之间的对应关系，可以使用字典代替冗长的条件分支。

```
OS = "macOS"
```

**Non-Pythonic**

```
if OS == "GNU/Linux":
    print("大佬，请用Debian的APT工具")
elif OS == "macOS":
    print("大佬，请用UTM，然后安装Debian")
elif OS == "Windows":
    print("大佬，请安装WSL，然后安装Debian")
else:
    print("壮士，请您到主席台上来")
```

**Pythonic**

```
package_manager = {
    "GNU/Linux": "请用Debian的APT工具",
    "macOS": "请用UTM，然后安装Debian",
    "Windows": "请安装WSL，然后安装Debian"
}

if OS in package_manager:
    print("大佬，", end="")
    print(package_manager[OS])
else:
    print("壮士，请您到主席台上来")
```

输出：

```
大佬，请用UTM，然后安装Debian
```

字典通过键（Key）查找对应的值（Value），适合实现固定映射关系



### **4. 字典空白访问：**`get()`

字典的 `get()` 方法可以在键不存在时返回指定的默认值。

```
data = {"mass": 1.67e-27}
```

**Non-Pythonic**

```
if "energy" in data:
    e = data["energy"]
else:
    e = 0
```

**Pythonic**

```
e = data.get("energy", 0)
```

输出：

```
0
```

`data.get(key, default)` 在键存在时返回对应的值，否则返回 `default`。如果没有指定默认值，则返回 `None`。



### **5. 使用** `any()` **和** `all()`

`any()` 判断是否至少有一个元素满足条件，`all()` 判断是否所有元素都满足条件。

```
values = [9, 99, 999]
```

**Non-Pythonic**

```
found = False

for x in values:
    if x > 100:
        found = True
        break

print(found)
```

**Pythonic**

```
found = any(x > 100 for x in values)
print(found)
```

输出：

```
True
```

如果需要判断是否所有元素都大于 0：

```
all(x > 0 for x in values)
# True
```

这里的 `x > 100 for x in values` 是生成器表达式（Generator Expression），可以按需产生布尔值，而不必先构造完整列表。

`any()` 和 `all()` 都支持短路求值：一旦结果能够确定，就停止继续迭代。



Python 支持将函数作为参数传递给其他函数。`map()`、`filter()` 和 `reduce()` 是函数式编程中的三个经典操作。



### **6. 遍历序列：in直接迭代**

Python 可以直接遍历可迭代对象（Iterable），无需通过索引访问元素。

```
items = ["苹果", "香蕉", "李子", "火龙果"]
```

**Non-Pythonic**

```
for i in range(len(items)):
    print(items[i])
```

**Pythonic**

```
for item in items:
    print(item)
```

如果同时需要索引和值，可以使用 `enumerate()`：

```
for i, item in enumerate(items):
    print(i, item)
```

输出：

```
0 苹果
1 香蕉
2 李子
3 火龙果
```



### **7. 同时遍历多个序列：**`zip()`

`zip()` 可以将多个可迭代对象中的元素逐一配对。

```
names = ["张三", "李四", "王五"]
scores = [100, 98, 99, 59]
```

**Non-Pythonic**

```
for i in range(len(names)):
    print(names[i], scores[i])
```

**Pythonic**

```
for name, score in zip(names, scores):
    print(name, score)
```

输出：

```
张三 100
李四 98
王五 99
```

`zip()` 默认在最短的序列结束时停止，多余的元素会被忽略



### **8. 列表推导式**

列表推导式将循环、表达式和可选的条件筛选组合在一起，用于构造新列表。

**Non-Pythonic**

```
squares = []

for x in range(10):
    squares.append(x**2)

print(squares)
```

**Pythonic**

```
squares = [x**2 for x in range(10)]
print(squares)
```

输出：

```
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

还可以加入条件，只计算偶数的平方：

```
squares = [x**2 for x in range(10) if x % 2 == 0]
print(squares)
```

输出：

```
[0, 4, 16, 36, 64]
```

列表推导式的基本结构是：

```
[expression for item in iterable if condition]
```

其中 `if condition` 可以省略。对于复杂的嵌套逻辑，普通循环可能更易读。



### **9. 映射：**`map()`

`map()` 将指定函数应用于可迭代对象中的各个元素，返回一个迭代器

**Non-Pythonic**

```
text_numbers = ["10", "20", "30"]
numbers = []

for text in text_numbers:
    numbers.append(int(text))
```

**Pythonic**

```
text_numbers = ["10", "20", "30"]

list(map(int, text_numbers))
```

也可以使用列表推导式：

```
[int(text) for text in text_numbers]
```

输出：

```
[10, 20, 30]
```

`map(function, iterable)` 会依次将元素传入 `function`，得到转换后的结果。



### **10. 过滤：**`filter()`

`filter()` 根据判断函数的返回值，保留满足条件的元素，并返回迭代器。

**Non-Pythonic**

```
samples = [-2, 0, 3, 5]
result = []

for number in samples:
    if number > 0:
        result.append(number)
```

**Pythonic**

```
def is_positive(number):
    return number > 0

samples = [-2, 0, 3, 5]

list(filter(is_positive, samples))
```

也可以使用列表推导式：

```
[number for number in samples if is_positive(number)]
```

输出：

```
[3, 5]
```

`filter(function, iterable)` 依次将元素传入判断函数，只保留返回值为真（Truthy）的元素



### **11. 归约：**`reduce()`

`reduce()` 使用二元函数依次合并可迭代对象中的元素，最终得到一个结果。

**Non-Pythonic**

```
factors = [1, 2, 3, 4, 5]
product = 1

for number in factors:
    product *= number

print(product)
```

**函数式写法**

```
from functools import reduce

factors = [1, 2, 3, 4, 5]

product = reduce(
    lambda total, number: total * number,
    factors,
    1
)

print(product)
```

**更 Pythonic 的写法**

```
from math import prod

factors = [1, 2, 3, 4, 5]
product = prod(factors)

print(product)
```

输出：

```
120
```

`reduce(function, iterable, initial)` 从初始值开始，依次调用二元函数累积结果。

对于求和、求乘积等常见操作，通常优先使用 `sum()`、`math.prod()` 等语义更明确的函数。



### **12. 函数自我递归**

递归是指函数在执行过程中调用自身，通常需要设置终止条件，避免无限递归。

**循环实现**

```
def div2(n):
    while True:
        print(n)
        if n <= 1:
            break
        n = n // 2

div2(123)
```

**递归实现**

```
def div2(n):
    print(n)
    if n > 1:
        div2(n // 2)

div2(123)
```

输出：

```
123
61
30
15
7
3
1
```

其中，`//` 是整除运算符。每次递归将 `n` 除以 2 并向下取整，直到 `n <= 1` 时停止。

递归并不一定比循环更 Pythonic。Python 没有尾递归优化，并且存在递归深度限制。对于简单重复计算，循环通常更合适；递归则适用于树结构、嵌套数据和分治算法等问题。



### **13. **`itertools`

`itertools` 是 Python 标准库中的迭代工具模块，提供累积运算、排列组合等功能。

这些工具通常采用惰性求值（Lazy Evaluation），按需生成元素，而不是一次性构造所有结果。

#### **13.1 累积最大值：**`accumulate()`

**Non-Pythonic**

```
data = [3, 4, 6, 2, 1, 9, 0, 7, 5, 8]
result = []
current_max = data[0]

for x in data:
    if x > current_max:
        current_max = x
    result.append(current_max)
```

**Pythonic**

```
import itertools as it

data = [3, 4, 6, 2, 1, 9, 0, 7, 5, 8]

list(it.accumulate(data, max))
```

输出：

```
[3, 4, 6, 6, 6, 9, 9, 9, 9, 9]
```

`accumulate()` 计算累积结果。这里得到的是前缀最大值，而不是整个序列的单个最大值；后者可以直接使用 `max(data)`。

#### **13.2 排列：**`permutations()`

**Non-Pythonic**

```
result = []

for a in range(1, 4):
    for b in range(1, 4):
        for c in range(1, 4):
            if a != b and b != c and a != c:
                result.append((a, b, c))
```

**Pythonic**

```
import itertools as it

list(it.permutations(range(1, 4)))
```

输出：

```
[(1, 2, 3), (1, 3, 2), (2, 1, 3),
 (2, 3, 1), (3, 1, 2), (3, 2, 1)]
```

`permutations(iterable, r)` 生成长度为 `r` 的排列；省略 `r` 时，默认使用全部元素。

#### **13.3 笛卡尔积：**`product()`

**Non-Pythonic**

```
result = []

for a in "ABCD":
    for b in "ABCD":
        result.append((a, b))
```

**Pythonic**

```
import itertools as it

list(it.product("ABCD", repeat=2))
```

输出：

```
[('A', 'A'), ('A', 'B'), ('A', 'C'), ('A', 'D'),
 ('B', 'A'), ('B', 'B'), ('B', 'C'), ('B', 'D'),
 ('C', 'A'), ('C', 'B'), ('C', 'C'), ('C', 'D'),
 ('D', 'A'), ('D', 'B'), ('D', 'C'), ('D', 'D')]
```

`product()` 生成多个可迭代对象的笛卡尔积，`repeat` 指定重复使用输入序列的次数。

#### **13.4 反向过滤：**`filterfalse()`

**Non-Pythonic**

```
result = []

for n in range(100):
    if n % 13 == 0:
        result.append(n)
```

**Pythonic**

```
import itertools as it

list(it.filterfalse(lambda n: n % 13, range(100)))
```

输出：

```
[0, 13, 26, 39, 52, 65, 78, 91]
```

`filterfalse()` 与 `filter()` 相反，只保留判断函数返回假值的元素。

在这个特定例子中，还可以更简单地写成：

```
list(range(0, 100, 13))
```



## python之禅

Pythonic 背后的哲学，可以归结为 The Zen of Python 中的几条原则



```
import this
```

The Zen of Python, by Tim Peters

Beautiful is better than ugly.\
Explicit is better than implicit.\
Simple is better than complex.\
Complex is better than complicated.\
Flat is better than nested.\
Sparse is better than dense.\
Readability counts.\
Special cases aren't special enough to break the rules.\
Although practicality beats purity.\
Errors should never pass silently.\
Unless explicitly silenced.\
In the face of ambiguity, refuse the temptation to guess.\
There should be one-- and preferably only one --obvious way to do it.\
Although that way may not be obvious at first unless you're Dutch.\
Now is better than never.\
Although never is often better than *right* now.\
If the implementation is hard to explain, it's a bad idea.\
If the implementation is easy to explain, it may be a good idea.\
Namespaces are one honking great idea -- let's do more of those!

> 译文：
>
> 优美胜于丑陋。\
> 显式胜于隐式。\
> 简单胜于复杂。\
> 复杂胜于繁复。\
> 扁平胜于嵌套。\
> 疏朗胜于密集。\
> 可读性至关重要。\
> 特殊情况不足以成为打破规则的理由。\
> 尽管实用性胜过纯粹性。\
> 错误绝不应该被悄然忽略。\
> 除非明确要求忽略。\
> 面对歧义，应当拒绝猜测的诱惑。\
> 应当有一种——最好只有一种——显而易见的实现方式。\
> 尽管这种方式起初可能并不明显，除非你是荷兰人。\
> 现在做胜于永远不做。\
> 尽管永远不做往往胜于贸然行动。\
> 如果一种实现难以解释，那它就不是好主意。\
> 如果一种实现容易解释，那它或许是个好主意。\
> 命名空间是一个绝妙的主意——让我们多多使用它！



### 设计理念：“符合直觉”

- 动态类型：变量名无需预先声明类型，可以绑定到不同类型的对象
- 面向对象编程：一切皆为对象

- 胶水语言（Glue Language）：可以调用 C/C++、Fortran、R 等语言编写的程序和库