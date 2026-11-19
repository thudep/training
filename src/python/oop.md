# 面向对象编程

Object-Oriented Programming, OOP

## 主流的编程范式

### 面向过程编程

以执行步骤为中心，描述程序应如何完成任务。

```python
energies = [1, 2, 3, 4, 5]

total = 0
for energy in energies:
    if energy > 2:
        total += energy

print(total)  # 12
```

### 面向对象编程

封装：将数据与操作数据看作统一的对象来维护。

```python
class ParticleEvent:
    def __init__(self, energies):
        self.energies = energies

    def total_energy(self, threshold):
        total = 0
        for energy in self.energies:
            if energy > threshold:
                total += energy
        return total


event = ParticleEvent([1, 2, 3, 4, 5])
print(event.total_energy(2))  # 12
```

### 面向函数编程（函数式编程）

以函数变换和函数组合为中心，而不是显式维护一个不断变化的变量。

```python
energies = [1, 2, 3, 4, 5]

filtered = filter(lambda e: e > 2, energies)
total = sum(filtered)

print(total)  # 12
```

### 面向意图编程（vibe coding）

使用 AI Agent 完成任务。

```python
import os
from openai import OpenAI

client = OpenAI(
    api_key=os.environ.get('DEEPSEEK_API_KEY'),
    base_url="https://api.deepseek.com",
)

energies = [1, 2, 3, 4, 5]

response = client.chat.completions.create(
    model="deepseek-flash",
    messages=[
        {"role": "user", "content": f"计算所有能量大于 2 的粒子的总能量：{energies}，直接返回结果"},
    ],
    stream=False,
    reasoning_effort="high",
    extra_body={"thinking": {"type": "enabled"}},
)

print(response.choices[0].message.content)
```

注意：需要事先在命令行里设置 `DEEPSEEK_API_KEY`：

```bash
export DEEPSEEK_API_KEY="sk-....（你的 API key）"
```

## Python：一切都是对象

Python 中的数据以对象表示，函数、类和模块也都是对象。Python 支持面向过程、面向对象和函数式等编程风格。

```python
help(1)
```

返回 `int` 的帮助文档。

```python
# Python: Everything is an Object

# 基本数据类型
print(type(None))            # <class 'NoneType'>

# 函数
print(type(print))           # <class 'builtin_function_or_method'>

# 类型本身也是对象
print(type(int))             # <class 'type'>
print(type(str))             # <class 'type'>
print(type(list))            # <class 'type'>
print(type(object))          # <class 'type'>
print(type(type))            # <class 'type'>

# 模块
import math
print(type(math))            # <class 'module'>
print(type(math.sqrt))       # <class 'builtin_function_or_method'>
```

面向对象编程（Object-Oriented Programming, OOP）的三个重要概念：

- 封装（Encapsulation）：将数据与操作数据的方法组织在一起，通过接口管理对象的内部状态。
- 继承（Inheritance）：在已有类的基础上定义新类，复用或扩展原有功能。
- 多态（Polymorphism）：不同类型的对象可以通过相同的接口执行各自的操作。

## 类与对象

类是一个模板，调用类可以创建实例。

- 对象：封装将数据与操作组织在一起；继承在已有类的基础上扩展；多态使不同对象可以通过共同接口参与操作
- 迭代器：包含 `__iter__()` 方法的类，需要 `__next__()` 定义迭代方法
- 运算：`__add__()`、`__sub__()` 等

将需要共享的数据保存在实例属性中，可以减少对全局变量的依赖。

### 1. 类与实例

```python
from dataclasses import dataclass

@dataclass
class Student:
    name: str
    age: int
    grade: str
    height: float
    weight: float
    is_sleeping: bool = False
```

- `class Student:` 定义一个类。
- `@dataclass` 是装饰器（Decorator），自动生成 `__init__()`、`__repr__()`、`__eq__()` 等方法，减少样板代码。
- `name: str` 等是类型注解（Type Hints），描述属性预期的数据类型，但 Python 默认不会在运行时强制检查。
- `is_sleeping: bool = False` 为属性提供默认值，创建实例时可以省略。

创建实例：

```python
student = Student("Alice", 20, "大二", 1.70, 60.0)

print(student.name)  # Alice
print(student.age)   # 20
```

这里的 `student` 是 `Student` 类的一个实例。

如果不使用 `@dataclass`，则通常需要手动编写构造初始化方法：

```python
class Student:
    def __init__(self, name, age, grade, height, weight):
        self.name = name
        self.age = age
        self.grade = grade
        self.height = height
        self.weight = weight
        self.is_sleeping = False
```

`__init__()` 在创建实例时负责初始化对象状态；`self` 表示当前实例。

### 2. 实例属性与实例方法

实例属性（Instance Attribute）用于保存对象自身的数据，实例方法（Instance Method）用于操作这些数据。

```python
def sleep(self):
    if self.is_sleeping:
        print(f"{self.name} is already sleeping.")
    else:
        self.is_sleeping = True
        print(f"{self.name} is now sleeping.")

def study(self):
    print(f"{self.name} 正在学习。")
```

以上方法定义在 `Student` 类内部。

调用：

```python
student.sleep()
student.sleep()
student.study()
```

输出：

```text
Alice is now sleeping.
Alice is already sleeping.
Alice 正在学习。
```

实例方法的第一个参数通常命名为 `self`，表示调用该方法的实例。

```python
student.study()
```

在普通实例方法中，大致等价于：

```python
Student.study(student)
```

因此，可以通过 `self.name`、`self.age` 等访问当前实例的属性。

将相关状态保存在实例属性中，可以减少对全局变量的依赖，使代码更容易维护和复用。

### 3. 特殊方法

Python 使用双下划线包围的方法名定义特殊行为，称为 Dunder Methods（Double Underscore Methods）。

例如：

```python
def __str__(self):
    return f"{self.name}，{self.age} 岁，{self.grade} 级"
```

调用：

```python
print(student)
```

输出：

```text
Alice，20 岁，大二 级
```

`print()` 会通过 `str()` 获取对象的字符串表示，而 `str()` 会调用对象的 `__str__()` 方法。

常见特殊方法：

| 方法         | 作用                       |
| ------------ | -------------------------- |
| `__init__()` | 初始化实例                 |
| `__str__()`  | 定义面向用户的字符串表示   |
| `__repr__()` | 定义面向开发者的字符串表示 |
| `__len__()`  | 支持 `len(obj)`            |
| `__eq__()`   | 定义 `==` 比较             |
| `__add__()`  | 定义 `+` 运算              |
| `__sub__()`  | 定义 `-` 运算              |
| `__iter__()` | 返回迭代器                 |
| `__next__()` | 返回下一个元素             |

例如，自定义向量加法：

```python
@dataclass
class Vector:
    x: float
    y: float

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

v1 = Vector(1, 2)
v2 = Vector(3, 4)

print(v1 + v2)
# Vector(x=4, y=6)
```

这种机制称为运算符重载（Operator Overloading）。

需要注意，可迭代对象只需要提供能够返回迭代器的 `__iter__()`；迭代器本身则需要实现 `__next__()`，并通常让 `__iter__()` 返回自身。

### 4. 属性装饰器：`@property`

`@property` 可以将方法包装成属性，使其通过属性访问语法调用。

```python
@property
def bmi(self):
    if self.height <= 0 or self.weight <= 0:
        raise ValueError("身高和体重必须为正")
    return self.weight / self.height ** 2
```

调用：

```python
print(student.bmi)
# 20.761245674740486
```

虽然 `bmi` 是一个方法，但调用时不需要括号：

```python
student.bmi    # 正确
student.bmi()  # 错误
```

`@property` 适合定义由其他属性计算得到的派生属性（Computed Property），也可以用于控制属性访问。

### 5. 静态方法：`@staticmethod`

静态方法不需要访问实例或类本身，因此不接收隐式的 `self` 或 `cls` 参数。

```python
@staticmethod
def is_adult(age):
    return age >= 18
```

调用：

```python
Student.is_adult(20)  # True
Student.is_adult(16)  # False
```

静态方法通常用于逻辑上属于某个类，但不依赖实例状态的辅助操作。

### 6. 类方法：`@classmethod`

类方法的第一个参数通常命名为 `cls`，表示调用该方法的类。

```python
@classmethod
def from_text(cls, text):
    name, age, grade = text.split(",")
    return cls(name.strip(), int(age), grade.strip())
```

调用：

```python
student = Student.from_text("Bob,21,大三")

print(student)
# Bob，21 岁，大三 级
```

三种方法的区别：

| 方法     | 第一个参数 | 典型用途           |
| -------- | ---------- | ------------------ |
| 实例方法 | `self`     | 访问和修改实例状态 |
| 类方法   | `cls`      | 访问类、创建实例   |
| 静态方法 | 无隐式参数 | 与类相关的辅助函数 |

### 7. 继承与方法重写

继承允许子类（Subclass）复用父类（Superclass）的属性和方法。

```python
class GraduateStudent(Student):
    def study(self):
        super().study()
        print(f"{self.name} 还在进行科研。")
```

这里：

- `GraduateStudent(Student)` 表示 `GraduateStudent` 继承 `Student`。
- 子类重新定义 `study()`，称为方法重写（Method Overriding）。
- `super().study()` 调用父类提供的实现，再扩展新的行为。

调用：

```python
graduate = GraduateStudent("Charlie", 24, "研二", 1.80, 70)

graduate.study()
```

输出：

```text
Charlie 正在学习。
Charlie 还在进行科研。
```

子类继承了父类的属性和其他方法，因此也可以调用：

```python
graduate.sleep()
print(graduate.bmi)
```

### 8. 多态（Polymorphism）

多态允许不同类型的对象通过相同的接口表现出不同的行为。

```python
students = [
    Student("Alice", 20, "大二", 1.70, 60),
    GraduateStudent("Charlie", 24, "研二", 1.80, 70)
]

for student in students:
    student.study()
```

输出：

```text
Alice 正在学习。
Charlie 正在学习。
Charlie 还在进行科研。
```

虽然两个对象都调用 `study()`，但实际执行的方法取决于对象的类型。

### 汇总示例

```python
from dataclasses import dataclass

@dataclass
class Student:
    name: str
    age: int
    grade: str
    height: float
    weight: float
    is_sleeping: bool = False

    def sleep(self):
        if self.is_sleeping:
            print(f"{self.name} is already sleeping.")
        else:
            self.is_sleeping = True
            print(f"{self.name} is now sleeping.")

    def study(self):
        print(f"{self.name} 正在学习。")

    def __str__(self):
        return f"{self.name}，{self.age} 岁，{self.grade} 级"

    @property
    def bmi(self):
        if self.height <= 0 or self.weight <= 0:
            raise ValueError("身高和体重必须为正")
        return self.weight / self.height ** 2

    @staticmethod
    def is_adult(age):
        return age >= 18

    @classmethod
    def from_text(cls, text):
        name, age, grade = text.split(",")
        return cls(name.strip(), int(age), grade.strip())

class GraduateStudent(Student):
    def study(self):
        super().study()
        print(f"{self.name} 还在进行科研。")

bob = Student("Bob", 17, "2026")
alice = Student("Alice", 19, "2024")

print(bob.name)
print(alice.name)
bob.age = 18
print(bob.age)

bob.sleep()

print(alice.is_sleeping)
alice.sleep()

print(bob)
print(str(bob))

print(f"BMI = {bob.bmi:.2f}")
bob.weight = 75.0
print(f"BMI = {bob.bmi:.2f}")

print(Student.is_adult(bob.age))
charlie = Student.from_text("Charlie, 20, 2025")
print(charlie)
david = GraduateStudent("David", 23, "2024")
print(david)
david.study()

print(repr(charlie))
print(charlie == Student("Charlie", 20, "2025"))
```
