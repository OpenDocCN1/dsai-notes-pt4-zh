
# Python Learning Notes

> 来源：[https://leonzhang.feishu.cn/docx/GACFdiADRoHSVCxAhvYcTcFwnVf](https://leonzhang.feishu.cn/docx/GACFdiADRoHSVCxAhvYcTcFwnVf)

> 了解基本规则即可，毕竟大模型写的比一个初学者（我）可好太多啦～

## 感谢 Google Bard

> 感谢 Google Bard 为我的代码学习提供了指导，感谢 LLMs、Google、感谢 Bard，嘿嘿 😁

| ![](img/6daaa88ee62bd71299f8b967e3748ce5.png) |

| --- |

## 学习教程

> 【【官方随书视频】最热门编程入门书《Python编程：从入门到实践（第2版）》】 `https://www.bilibili.com/video/BV1Sd4y1a7Us/?p=27&share_source=copy_web&vd_source=672ec070780ce29c890256a5fabe799b`

## 手敲代码（练习用）



# 打印

``` 
print("hello!! 你好呀！！")
print("=====")
```

# 什么是赋值

``` 
# 定义字符串变量 name
name = "Eric"
# 用 f 字符串打印一句打招呼的话
print(f"Hello {name}, would you like to learn some Python today?")

# 把 name 转化成小写
name = "Eric".lower()
print(f"Hello {name}, would you like to learn some Python today?")

name = "Eric".upper()
print(f"Hello {name}, would you like to learn some Python today?")

name = "Eric zhang".title()
print(f"Hello {name}, would you like to learn some Python today?")
print("=====")
```

日期：2023年8月17日  
题目要求：分别用加减乘除得到数字 8，并且打印出来。

``` 
print(1 + 1 + 6)
print(2 * 4)
print(10 - 2)
print(16 / 2)
print("注意：python3 里除法结果为浮点数；加减乘取决于参与运算的数值。如：")
print(1 + 1.0 + 6)
print("=====")
```

``` 
print("- 注释")
# 我是一行注释
# 快捷键 control + /
'''
你这么聪明，相信也看出来了，
这个是多行注释。
'''
print("=====")
```

# 数据类型

``` 
# 对字符串求长度 str
s = "Hello world!"
print(len(s))

# 通过索引获取单个字符（记住：索引是从 0 开始的）
print(s[0])
print(s[11])
print(s[len(s) - 1])

# 布尔类型 bool
b1 = True
b2 = False

# 空值类型
n = None

# type 函数
# 整数为 int，浮点数为 float
print(type(s))
print(type(b1))
print(type(n))
print(type(1))
print(type(1.5))
print("=====")
```

# 列表及操作

> 练习 3-1：姓名  
> 将一些朋友的姓名存储在一个列表中，并将其命名为 `names`。  
> 依次访问该列表中的每个元素，从而将每个朋友的姓名打印出来。

``` 
# names = ["小王", "小郝", "小刘"]
# print(names[0])
# print(names[1])
# print(names[2])
```

> 练习 3-2：问候语  
> 继续使用练习 3-1 中的列表，但不打印每个朋友的姓名，而为每人打印一条消息。  
> 每条消息都包含相同的问候语，但抬头为相应朋友的姓名。

``` 
# names = ["小王", "小郝", "小刘"]
# greeting = "你好呀，"
# print(greeting + names[0])
# print(greeting + names[1])
# print(greeting + names[2])
```

> 练习 3-4：嘉宾名单  
> 如果你可以邀请任何人一起共进晚餐〔无论是在世的还是故去的），你会邀请哪些人？  
> 请创建一个列表，其中包含至少三个你想邀请的人，然后使用这个列表打印消息，邀请这些人来与你共进晚餐。

``` 
# names = ["小王", "小郝", "小刘"]
# invite = "一块儿吃个饭吧？"
# print(invite + names[0])
# print(invite + names[1])
# print(invite + names[2])
```

> 练习 3-5：修改嘉宾名单  
> 你刚得知有位嘉宾无法赴约，因此需要另外邀请一位嘉宾。  
> 以完成练习 3-4 时编写的程序为基础，在程序末尾添加一条 `print` 语句，指出哪位嘉宾无法赴约。  
> 修改嘉宾名单，将无法赴约的嘉宾的姓名替换为新邀请的嘉宾的姓名。  
> 再次打印一系列消息，向名单中的每位嘉宾发出邀请。

``` 
# names = ["小王", "小郝", "小刘"]
# invite = "一块儿吃个饭吧？"
# print(names[2] + "不能参加")
# names[2] = "小李"
# print(invite + names[0])
# print(invite + names[1])
# print(invite + names[2])
```

> 练习 3-6：添加嘉宾  
> 你刚找到了一个更大的餐桌，可容纳更多的嘉宾。请想想你还想邀请哪三位嘉宾。  
> - 以完成练习 3-4 或练习 3-5 时编写的程序为基础，在程序末尾添加一条 `print` 语句，指出你找到了一个更大的餐桌。  
> - 使用 `insert()` 将一位新嘉宾添加到名单开头。  
> - 使用 `insert()` 将另一位新嘉宾添加到名单中间。  
> - 使用 `append()` 将最后一位新嘉宾添加到名单末尾。  
> - 打印一系列消息，向名单中的每位嘉宾发出邀请。

``` 
# print("找到了一个更大的餐桌！")
# names.insert(0, "小林")
# names.insert(2, "小张")
# names.append("小韩")
# print(names)
# print(invite + names[0])
# print(invite + names[1])
# print(invite + names[2])
# print(invite + names[3])
# print(invite + names[4])
# print(invite + names[5])
```

# 遍历

> 练习 4-1：比萨  
> 想出至少三种你喜欢的比萨，将其名称存储在一个列表中，再使用 `for` 循环将每种比萨的名称打印出来。  
> - 修改这个 `for` 循环，使其打印包含比萨名称的句子，而不仅仅是比萨的名称。  
>   对于每种比萨，都显示一行输出，下面是一个例子。  
>   `I like pepperoni pizza.`  
> - 在程序末尾添加一行代码，它不在 `for` 循环中，指出你有多喜欢比萨。  
>   输出应包含针对每种比萨的消息，还有一个总结性句子，下面是一个例子。  
>   `I really love pizza!`

``` 
pizza_list = ["香肠披萨", "榴莲披萨", "芝士披萨"]
for pizza in pizza_list:
    print("我喜欢" + pizza)
print("I really love pizza！")
```

# 创建数值列表

> `range(1 , 9)` 即起始值、结束值（不含）  
> `range(2, 10, 2)` 即起始值、结束值、步长

> 练习 4-4：一百万  
> 创建一个包含数 1 ~ 1 000 000 的列表，再使用一个 `for` 循环将这些数打印出来。  
> (如果输出的时间太长，按 `Ctrl` + `C` 停止输出或关闭输出窗口。)

``` 
numbers = []
for num in range(1, 1000001):
    numbers.append(num)
# for num in numbers:
#     print(num)
```

> 练习 4-5：一百万求和  
> 创建一个包含数 1 ~ 1000 000 的列表，再使用 `min()` 和 `max()`  
> 核实该列表确实是从 1 开始、到 1 000 000 结束的。  
> 另外，对这个列表调用函数 `sum()`，看看 Python 将一百万个数相加需要多长时间。

``` 
print(min(numbers))
print(max(numbers))
print(sum(numbers))
```

# if

> 练习 5-3：外星人颜色  
> 假设在游戏中刚射杀了一个外星人，请创建一个名为 `alien_color` 的变量，并将其赋值为 `'green'`、`'yellow'` 或 `'red'`。  
> - 编写一条 `if` 语句，检查外星人是否是绿色的。如果是，就打印一条消息，指出玩家获得了 5 分。  
> - 编写这个程序的两个版本，在一个版本中上述测试通过了，而在另一个版本中未通过（未通过测试时没有输出）。

``` 
# alien_color = "green"
# if alien_color == "green":
#     print("You got 5 points!")
```

> 练习 5-4：外星人颜色 2  
> 像练习 5-3 那样设置外星人的颜色，并编写一个 `if_else` 结构。  
> - 如果外星人是绿色的，就打印一条消息，指出玩家因射杀该外星人获得了 5 分。  
> - 如果外星人不是绿色的，就打印一条消息，指出玩家获得了 10 分。  
> - 编写这个程序的两个版本，在一个版本中执行 `if` 代码块，在另一个版本中执行 `else` 代码块。

``` 
# alien_color = "green"
# if alien_color == "green":
#     print("You got 5 points!")
# else:
#     print("You got 10 points!")
#
# alien_color = "yellow"
# if alien_color == "green":
#     print("You got 5 points!")
# else:
#     print("You got 10 points!")
```

> 练习 5-5：外星人颜色了  
> 将练习 5-4 中的 `if_else` 结构改为 `if_elif-else` 结构。  
> - 如果外星人是绿色的，就打印一条消息，指出玩家获得了 5 分。  
> - 如果外星人是黄色的，就打印一条消息，指出玩家获得了 10 分。  
> - 如果外星人是红色的，就打印一条消息，指出玩家获得了 15 分。  
> 编写这个程序的三个版本，分别在外星人为绿色、黄色和红色时打印一条消息。

``` 
# alien_color = "red"
# if alien_color == "green":
#     print("You got 5 points!")
# elif alien_color == "yellow":
#     print("You got 10 points!")
# else:
#     print("You got 15 points!")
```

# 逻辑运算

`and`（一个错都错）`or`（一个对都对）`not`（逆反）  
优先级 `not` > `and` > `or`

# 字典

字典 `dictionary` 键值对 == 键 `key` ：值 `value`  
元组 `tuple`

> 练习 6-1：人  
> 使用一个字典来存储一个熟人的信息，包括名、姓、年龄和居住的城市。  
> 该字典应包含键 `first_name`、`last_name`、`age` 和 `city`。  
> 将存储在该字典中的每项信息都打印出来。

``` 
# friend = {"first_name": "小美", "last_name": "王", "age": "24", "city": "杭州"}
# print("姓名：" + friend["last_name"] + friend["first_name"])
# print("年龄：" + friend["age"])
# print("城市：" + friend["city"])
```

> 练习 6-5：河流  
> 创建一个字典，在其中存储三条重要河流及其流经的国家。  
> 例如，一个键值对可能是 `'nile'` ：`'egypt'` 。  
> - 使用循环为每条河流打印一条消息，下面是一个例子。  
>   `The Nile runs through Egypt.`  
> - 使用循环将该字典中每条河流的名字打印出来。  
> - 使用循环将该字典包含的每个国家的名字打印出来。

``` 
# rivers = {"Nile": "Egypt", "yangtze river": "China", "Amazon river": "Brazil"}
# for river, country in rivers.items():
#     print("The " + river + "runs through " + country + ".")
# for river in rivers.keys():
#     print(river)
# for country in rivers.values():
#     print(country)
```

本期遇到问题：1. 未切换为英文半角的冒号；  
2. 代码如何区分字典中的键或值为 `river` 还是 `country`？

> 练习 7-3：10 的整数倍  
> 让用户输入一个数，并指出该数是否是 10 的整数倍。

``` 
# number = int(input("请输入一个数字: "))
# if number % 10 == 0:
#     print("你输入的数字是 10 的整数倍")
# else:
#     print("你输入的数字不是 10 的整数倍")
```

在条件何时结束未知的情况下，`while` 循环比 `for` 循环（循环次数未知）更适合使用。

> 练习 7-4：比萨配料  
> 编写一个循环，提示用户输入一系列比萨配料，并在用户输入 `'quit'` 时结束循环。  
> 每当用户输入一种配料后，都打印一条消息，指出我们会在比萨中添加这种配料。

``` 
# user_input = input("请输入你想要输入的披萨配料：")
# while user_input != "quit":
#     print("好的，披萨中会为你添加" + user_input)
#     user_input = input("请输入你想要输入的披萨配料：")
```

本期遇到问题：未切换为英文半角的冒号

# 软件开发原则

软件开发原则之一：`DRY` 原则，`Don't Repeat Yourself`。

> 练习 8-2：喜欢的图书  
> 编写一个名为 `favorite_book()` 的函数，其中包含一个名为 `title` 的形参。  
> 这个函数打印一条消息，下面是一个例子。  
> `One of my favorite books is Alice in Wonderland.`  
> 调用这个函数，并将一本图书的名称作为实参传递给它。

``` 
# def favorite_book(title):
#     # 调用函数，可以将图书名称作为实参替换形参。
#     print(f"One of my favorite books is {title}.")
#
# favorite_book("我的奋斗")
```

> 练习 8-6：城市名  
> 编写一个名为 `city_country()` 的函数，它接受城市的名称及其所属的国家。  
> 这个函数应返回一个格式类似于下面的字符串：  
> `"Santiago, Chile"`  
> 至少使用三个城市国家对来调用这个函数，并打印它返回的值。

``` 
# def city_country(city, country):
#     return f"{city}, {country}"
#
# print(city_country("杭州", "中国"))
# print(city_country("柏林", "德国"))
# print(city_country("纽约", "美国"))
#
# # 问题：因为 return 也是函数，所以不存在括号啊
```

# 面向对象编程与面向过程编程

- 面向对象：一种以对象为基础进行编程的方法。它将程序组织成一个个相互关联的对象，每个对象都有自己的状态（属性）和行为（方法）。  
  通过定义类来创建对象的实例，并使用 `封装`、`继承` 和 `多态` 等特性来实现代码的重用性和灵活性  
- 面向过程：注重解决问题的步骤和流程，将程序组织为一系列的函数或过程。  
- 下划线命名法则（定义普通变量）、`Pascal` 命名法则（定义类名：首字母大写来分割单词 + 「:」）

- 创建类和使用类、使用类和实例、继承  
- 创建类 `class`，构造方法、定义属性 `__init__`，创建新对象 `class()`，定义方法（在 `class` 中）`def`

> 练习 9-1：餐馆  
> 创建一个名为 `Restaurant` 的类，为其方法 `__init__()` 设置属性 `restaurant_name` 和 `cuisine_type`。  
> 创建一个名为 `describe_restaurant()` 的方法和一个名为 `open_restaurant()` 的方法，前者打印前述两项信息，而后者打印一条消息，指出餐馆正在营业。  
> 根据这个类创建一个名为 `restaurant` 的实例，分别打印其两个属性，再调用前述两个方法。

``` 
# class Restaurant:
#     def __init__(self, restaurant_name, cuisine_type):
#         self.restaurant_name = restaurant_name
#         self.cuisine_type = cuisine_type
#
#     def describe_restaurant(self):
#         print(f"餐厅名: {self.restaurant_name}")
#         print(f"菜系： {self.cuisine_type}")
#
#     def open_restaurant(self):
#         print("餐厅正在营业...")
#
# taotaoju = Restaurant("陶陶居", "粤菜")
#
# print(taotaoju.restaurant_name)
# print(taotaoju.cuisine_type)
# taotaoju.describe_restaurant()
# taotaoju.open_restaurant()
#
# xiaoliuchuancaiguan = Restaurant("小刘川菜馆", "川菜")
#
# print(xiaoliuchuancaiguan.restaurant_name)
# print(xiaoliuchuancaiguan.cuisine_type)
# xiaoliuchuancaiguan.describe_restaurant()
# xiaoliuchuancaiguan.open_restaurant()
```

> 练习 9-4：就餐人数  
> 在为完成练习 9-1 而编写的程序中，添加一个名为 `number_served` 的属性，并将其默认值设置为 0。  
> 根据这个类创建一个名为 `restaurant` 的实例。打印有多少人在这家餐馆就餐过，然后修改这个值并再次打印它。  
> 添加一个名为 `set_number_served()` 的方法，让你能够设置就餐人数。  
> 调用这个方法并向它传递一个值，然后再次打印这个值。  
> 添加一个名为 `increment_number_served()` 的方法，让你能够将就餐人数递增。  
> 调用这个方法并向它传递一个这样的值：你认为这家餐馆每天可能接待的就餐人数。

``` 
# class Restaurant:
#     def __init__(self, restaurant_name, cuisine_type):
#         self.restaurant_name = restaurant_name
#         self.cuisine_type = cuisine_type
#         self.number_served = 0
#
#     def describe_restaurant(self):
#         print(f"餐厅名: {self.restaurant_name}")
#         print(f"菜系： {self.cuisine_type}")
#
#     def open_restaurant(self):
#         print("餐厅正在营业...")
#
#     def set_number_served(self, number_served):
#         self.number_served = number_served
#
#     def increment_number_served(self, increment_number):
#         self.number_served += increment_number
#         # self.number_served = self.number_served + increment_number
#
# taotaoju = Restaurant("陶陶居", "粤菜")
#
# print(taotaoju.restaurant_name)
# print(taotaoju.cuisine_type)
# print(taotaoju.number_served)
# taotaoju.describe_restaurant()
# taotaoju.open_restaurant()
# taotaoju.set_number_served(500)
# taotaoju.increment_number_served(20)
# print(taotaoju.number_served)
```

什么时候用继承？ `'A 是 B'`，即 `classA(B)`  
`__init__`：构造函数，用于在创建对象时进行初始化操作。  
`super`：内置函数，用于调用父类的方法。

> 练习 9-6：冰激凌小店  
> 冰激凌小店是一种特殊的餐馆。编写一个名为 `IceCreamStand` 的类，让它继承为完成练习 9-1 或练习 9-4 而编写的 `Restaurant` 类。  
> 这两个版本的 `Restaurant` 类都可以，挑选你更喜欢的那个即可。  
> 添加一个名为 `flavors` 的属性，用于存储一个由各种口味的冰激凌组成的列表。  
> 编写一个显示这些冰激凌的方法。创建一个 `IceCreamStand` 实例，并调用这个方法。

``` 
# class Restaurant:
#     def __init__(self, restaurant_name, cuisine_type):
#         self.restaurant_name = restaurant_name
#         self.cuisine_type = cuisine_type
#
#     def describe_restaurant(self):
#         print(f"餐厅名: {self.restaurant_name}")
#         print(f"菜系： {self.cuisine_type}")
#
#     def open_restaurant(self):
#         print("餐厅正在营业...")
#
# class IceCreamStand(Restaurant):
#     def __init__(self, restaurant_name, flavors):
#         super().__init__(restaurant_name, "甜品")
#         self.flavors = flavors
#
#     def show_flavors(self):
#         print(self.flavors)
#
#
# so_sweet = IceCreamStand("So Sweet",  ["香草", "巧克力", "草莓", "西瓜"])
# so_sweet.describe_restaurant()
# so_sweet.show_flavors()
```

# 引入模块

- `import` 语句  
- `from...import` 语句  
- `from...import *`

# 读文件

`read()`（全部字符串）`readline()`（只读一行）`readlines()`（全部文件内容组成的列表）

> 练习 10-1：Python 学习笔记  
> 在文本编辑器中新建一个文件，写几句话来总结一下你至此学到的 Python 知识，其中每一行都以 `"In Python you can"` 打头。  
> 将这个文件命名为 `learning_python.txt`，并存储到为完成本章练习布编写的程序所在的目录中。  
> 编写一个程序，它读取这个文件，并将你所写的内容打印三次：  
> 第一次打印时读取整个文件；第二次打印时遍历文件对象；  
> 第三次打印时将各行存储在一个列表中，再在 `with` 代码块外打印它们。

``` 
# f = open("./Learning_python.txt", "r", encoding = "utf-8")
# content = f.read()
# print(content)
# f.close()

# with open("./Learning_python.txt", "r", encoding = "utf-8") as f:
#     # 1 print(f.readlines())
# # 遇到问题：with open as 函数，再次未加冒号「:」
#
#     # 2 for line in f.readlines():
#     #     print(line)
#     line_list = f.readlines()
# print(line_list)
```

# 写入文件

> 练习 10-4：访客名单  
> 编写一个 `while` 循环，提示用户输入名字。用户输入名字后，在屏幕上打印一句问候语，并将一条到访记录添加到文件 `guest_book.txt` 中。  
> 确保这个文件中的每条记录都独占一行。

``` 
# user_input = input("请输入访客的名字（完成所有名字输入后，请输入 q 终止程序): ")
# with open("guest_book.txt", "a") as f:
#     while user_input != "q":
#         f.write(user_input + "\n")
#         print(f"欢迎{user_input}")
#         user_input = input("请输入访客的名字（完成所有名字输入后，请输入 q 终止程序): ")
```

> 练习 10-6：加法运算  
> 提示用户提供数值输入时，常出现的一个问题是，用户提供的是文本而不是数。  
> 在此情况下，当你尝试将输入转换为整数时，将引发 `ValueError` 异常。  
> 编写一个程序，提示用户输入两个数，再将其相加并打印结果。  
> 在用户输入的任何一个值不是数时都捕获 `ValueError` 异常，并打印一条友好的错误消息。  
> 对你编写的程序进行测试：先输入两个数，再输入一些文本而不是数。

``` 
# try 函数，通常在可能会出现错误行代码的上一行

# user_input1 = input("请输入第一个整数：")
# user_input2 = input("请输入第二个整数：")
# try:
#     result = int(user_input1) + int(user_input2)
# except ValueError:
#     print("请输入整数。 ")
# except NameError:
#     print("汉字，请输入整数。 ")
# else:
#     print(result)
```
