
# LLM多智能体AutoGen教程6：Python typing是如何自动生成工具调用请求参数的？

> 来源：[https://agijuejin.feishu.cn/docx/MIHldcJY7oc2fpxa2xtc5ULYn4W](https://agijuejin.feishu.cn/docx/MIHldcJY7oc2fpxa2xtc5ULYn4W)

> 作者：还就可 | 深入LLM Agent应用开发

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/_w3D_RJr33XKZQTByI🐧Wg)qq.com/s/_w3D_RJr...

书接上文《26K Star!LLM多智能体Aut...oGen教程5：函数调用之避免捏造参数》，我们使用`typing`对可调用函数进行注解，`AutoGen`据此自动生成`OpenAI API`函数调用时的请求参数。本文将简要介绍`Python`中`typing`包的基础操作，顺便简单的介绍一下`Pydantic`，然后深入`AutoGen`的流程，探查`AutoGen`如何通过`typing`帮助自动生成`Open AI API`工具调用的参数。

由于`Python`是动态解释型语言，不像静态语言，必须得声明类型，而`Python`通常无需声明类型，采用自动推导。而`typing`包的引入，就是为了支持类型提示，注意是提示，而不是声明，他的类型在运行时依然是动态的。而`Pydantic` [1] 是 `Python` 中最广泛使用的数据验证库，快速且可扩展，`Pydantic`可以很好地配合你的代码检查工具/`IDE`/`思维方式`，采用纯正的、规范的 `Python 3.8+` 定义数据的格式。`Pydantic`之所以是最快的，是因为它是采用`Rust`编写的。

## 1 Python内置typing

使用`Python`时候，我们常常并不添加函数参数类型，阅读代码时候，常常需要根据上下文才能得知具体的类型。比如上文 `get_current_weather` 函数，通常我们直接写作：

```
def get_current_weather(date, location, unit):
    pass
```

当然，有时候我们还会采用注释来说明函数功能，参数类型，如下所示。

```
def get_current_weather(date, location, unit):
    """Get the current weather in a given location

    Args:
        date: the date of weather
        loction: the location of weather
        unit: the unit of temperature, should be one of Celsius and Fahrenheit

    Returns:
        Returned a dict contain the current weather of given location and date like
        {'date': '2024-01-02',
        'location': '南京',
        'unit': 'Celsius',
        'temeprature': 23}
    """
```

虽然注释已经说明了函数的功能、输入和输出，但是并没有一个统一的标准，只能作为阅读文档参考。所以`Python`自`3.5`引入了`typing`来支持类型提示，让我们简单过一遍`typing`的使用方法，本文并不打算对着`typing`文档翻译，而是选择我们常用的`typing`进行解决，更深入的仍然需要读者自行探索。此外，在 `PEP-484` [2] 仍然备注到：

> `Python` 仍然是一种动态类型语言，并且作者不希望强制使用类型提示，即使按照惯例。

### 1.1 简单类型提示

```
def greeting(name: str, age: int) -> str:
    return f'Hello {name}, you are {age} years old'
```

使用参数名：类型以设置参数类型，通过 `->` 来设置预期返回值的类型。除了这些通用类型，对于`dict`、`tuple`和`list`，也同样适用标注。

```
from typing import List, Tuple, Dict 

x: List[Union[int, float]] = [2, 3, 4.1]
dim: Tuple[int, int, int] = (11, 22, 33)
rectangle: Dict[str, int] = {
    'height': 340,
    'width': 500,
}
```

然而，我们经常一个参数多个类型，我们可以使用`Union`来表示参数可选类型。

```
def greeting(name: str, age: int, height: Union[int, float]) -> str:
    return f'Hello {name}, you are {age} years old and {height} cm'
```

对于同时支持输入除了自身类型，还可以为 `None` ，除了使用`Union`表示 `Union[str, None]` ，还可以使用更简洁的 `Optional` 。

```
def greeting(name: Optional[str], age: int, height: Union[int, float]) -> str:
    return f'Hello {name}, you are {age} years old and {height} cm'
```

有时候我们会使用`enum`来限定输入参数的值选项，在类型提示中也有用于限定输入参数的字面量可选值 `Literal` ，比如打开文件的模式选择，

```
type Mode = Literal['r', 'rb', 'w', 'wb']
def open_helper(file: str, mode: Mode) -> str:
    ...
```

### 1.2 自定义类型提示

以上文中自定义的天气类型为例，我们是用`TypedDict`这一特殊构造函数来添加类型提示到字典`dict`中。有时候，我们经常使用`dict`来传递参数，然而`dict`中有哪些键值是不明确的。个人觉得这并不是是一个好的工程方式，当然`Python`本身是支持这种动态特性的。使用`TypedDict`可以用来限定的`dict`的`key`和类型，如下所示，如果没有提供定义的`key`，也只是在类型提示时候会报错，运行时并不会出问题。

```
from typing import TypedDict
class Weather(TypedDict):
    location: str
    date: str
    unit: str
    temperature: int
a: Weather = {'location': 'nj', 'date': '2024-01-01', 'unit': 'Celsius', 'temperature': 23}  # OK
b: Weather = {'label': 'nj'}           # Fails type check
```

![](img/12afc3049ed1ec2012dd34d86b68e291.png)

image-20240611125440453

默认 `TypedDict` 是需要填入所有参数才不会提示报错。但如果为了实现`key`是可选的，即部分`key`强制包含，其他可选，可以配合 `total` 使用 `Required` 。

```
class Weather2(TypedDict, total=False):
    location: Required[str]
    date: str
    unit: str
    temperature: int

c: Weather2 = {'location': 'nj'}  # OK
```

> 注意：`Required`只在`3.11`才提供，之前版本需要使用 `from typing_extensions import Required` 。

### 1.3 标注可调用对象

对于函数，我们使用`Callable`标注，使用类似于 `Callable[[int], str]` 来注解，语法包含两个参数，前者是参数类型列表，后者表示返回类型，返回类型必须是单一类型。

```
from typing import Callable

def concat(x: str, y: str) -> str:
    return x + y

x: Callable[..., str] = concat
```

### 1.4 给注解添加元数据

使用`Annotated`给注解加入元数据，方法是使用形如 `Annotated[T, x]` 将元数据`x`添加到给定类型`T`。有点抽象，举个例子，就是在使用`LLM`的工具调用时候，参数也需要类型和注释，`Annotated` 就能实现。

```
def get_current_weather(date: Annotated[str, "the date"], location: Annotated[str, "the location"],
                        unit: Annotated[str, "the unit of temperature"]) -> Annotated[
    Weather, "A dictionary representing a weather with location, date and unit of temperature"]:
    """Get the current weather in a given location"""
    return {
        "location": location,
        "unit": unit,
        "date": date,
        "temperature": 23
    }
```

通过使用 `Annotated` 我们给参数 `date` 、 `location` 、 `unit` 甚至是返回值自定义类型都加上了类型和参数说明。

### 1.5 类型变量TypeVar

类型变量，构造类型变量的推荐方式是使用泛型函数、泛型类和泛型类型别名的专门语法。

```
typing.TypeVar(name, *constraints, bound=None, covariant=False, contravariant=False, infer_variance=False)
```

其中`name`，比如说定义泛型类型`T`，就是这个`T`。`bound`用来将泛型类型绑定到特定类型，这样在静态类型检查器就可以知道他们到底接受的是什么类型，它可以作为泛型类型、泛型函数和类型别名定义形参。

```
T = TypeVar('T')  # Can be anything
S = TypeVar('S', bound=str)  # Can be any subtype of str
A = TypeVar('A', str, bytes)  # Must be exactly str or bytes

def repeat[T](x: T, n: int "T") -> Sequence[T]:
    """Return a list containing n references to x."""
    return [x]*n

def print_capitalized[S: str](x: S "S: str") -> S:
    """Print x capitalized, and return x."""
    print(x.capitalize())
    return x

def concatenate[A: (str, bytes)](x: A, y: A "A: (str, bytes)") -> A:
    """Add two strings or bytes objects together."""
    return x + y
```

## 2. Pydantic

前面已经说到`Pydantic`是世界上最广泛使用的`Python`的`validation`库，它基于`typing`包进行数据验证和序列化，采用`Rust`编写最快的内核验证代码，可以轻易生成`JSON Schema`，支持常见数据类型转换（比如`str`解析为`int`、字符串转换为时间），还兼容`dataclass`和`TypedDict`等。

```
pip install pydantic
```

基本使用方法，是定义类型，并继承自`Pydantic`的`BaseMode`。

```
from datetime import datetime
class User(BaseModel):
    id: int
    name: str = "X2046"
    birthday: Optional[datetime] = None
    age: Annotated[int, Field(description="the age of user", alias="age", gt=18)]
    address: Annotated[str, Field(default='nj', description="the default location of user", alias="city")]

user = User(id='123', birthday='2024-06-01')
assert user.id == 123
print(user.name)
> X2046
print(isinstance(user.id, int))
> True
print(user.model_dump())
> {'id': 123, 'name': 'X2046', 'birthday': datetime.datetime(2024, 6, 1, 0, 0), 'age': 20, 'address': 'nj'}
print(user.model_dump_json())
> {"id":123,"name":"X2046","birthday":"2024-06-01T00:00:00","age":20,"address":"nj"}
print(user.model_dump_json(by_alias=True))
> {"id":123,"name":"X2046","birthday":"2024-06-01T00:00:00","age":20,"city":"nj"}
```

在实例化`User`时，会执行所有的解析和验证，可以看到

+   `id`类型为`int`，但传入的是`str`，自动解析为`int`类型。

+   `birthday`，类型为`datetime`或者`None`，但传入的是字符串，也被自动解析为`datetime`类型

+   `name`和`birthday` 都有默认值，在`typing`中这被认为 `NotRequired` ，而`id`是 `Required` ，因此在`validation`阶段如果`id`没填就会报错。
