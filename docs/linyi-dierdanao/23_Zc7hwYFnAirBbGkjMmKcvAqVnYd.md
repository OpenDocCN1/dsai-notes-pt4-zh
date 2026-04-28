# Python编程语言-从入门到精通

> 来源：[https://docs.feishu.cn/docx/WcGIdqKZzoLvFExbMLwcf5hNnVy](https://docs.feishu.cn/docx/WcGIdqKZzoLvFExbMLwcf5hNnVy)

# Python编程语言-1个月学会

1.  Python 教程列表：

https://www.bilibili.com/video/BV1Sz4y1U77N/?spm_id_from=333.337.search-card.all.click&vd_source=390875fa901ff2bc2178a9d8b58b5bf9

https://www.bilibili.com/video/BV1944y1x7SW?spm_id_from=333.788.player.switch&vd_source=61c77e316992315215eb430d8fd14b3f&p=6

https://www.youtube.com/watch?v=bPrmA1SEN2k&list=PLZoTAELRMXVNUL99R4bDlVYsncUNvwUBB

1.  Flask 教程列表：

https://www.youtube.com/watch?v=4L_xAWDRs7w&list=PLZoTAELRMXVPBaLN3e-uoVRR9hlRFRfUc

1.  FastAPI 教程：

https://www.youtube.com/watch?v=WU65u9d-97c&list=PLZoTAELRMXVPgsojPOHF9i0u2L83-m9P7

1.  mooc平台的中文教程：北理工

https://www.icourse163.org/course/BIT-268001?tid=1471277463#/info

# Python：

# 0、前置事项：

+   安装Python解释器，机器无法直接读懂我们的代码，需要有个解释器去翻译，翻译一行执行一行，去官网下载即可。

> 还有种交互模式，不需要print，写一行执行一行

+   PyCharm：IDE，集成开发环境。

> vene：独立的虚拟环境，让每个项目都可以使用不同的解释器或者三方库。

# 1、输出：

> print("输出内容")

> print("a"+"b"+"c")，字符串连接

> print("he is \"a\")，转义符\

> print("hello!\nHi")，换行

> print（'''

> 任意内容

> '''），三引号，不需要换行，原文打印

# 2、变量：

> 变量起名：my_love1，最好是英文名，比如username，大小写不一样

> 变量赋值：my_love1=13679259362

> print（my_love1），打印变量不需要引号，引号是打印字符串

# 3、数学运算：

> 字符串："6"，整数：6，浮点数：6.0

> +、-、*、/、**（二次方）

+   函数和三方库（工具箱，啥时候用啥时候引入）

> 内置函数：print

> 三方库：math

> 引入方法：

1.  import math

> math.sin(1)，计算完需要取值、打印

1.  from math import math.sin，math.cos

> sin(1)

> cos(1)

1.  pip install xx库 ，安装库到环境中，在命令提示符安装

>

# 4、注释：

> #注释

> #又是注释

> '''

> 我是注释

> 我是备注

> 我也是注释

> '''

# 5、数据类型：

1.  **字符串****str**

> 长度：len('abc')

> 索引："abc"[0]，第0个元素

1.  整数、浮点数：int、float

1.  **布尔类型****：bool**

> Ture、False

1.  **空值类型：NoneType**

> None，是个值，只不过是暂时空的。

1.  **列表：**

> shopping_list=['字符串1','字符串2']

> 列表加东西：shopping_list.append('字符串3')，列表是可以变的

> 删除东西：.remove("aaa")

> 索引：shopping_list[0]

1.  **字典：**

> 键值对：{key:value}，字典可变，但键的类型不能变，所以列表不能作为键

> dict={**"****a****":"****11****",**"b:22’","c:33"}

> 增加：dict["aaa"]="1111"

> 删除：**del **dict["key"]

> 检查键是否存在："某个key" **in** dict

> 返回字典中的键、值、键值对：dict.keys()、.values()、.items()；键值对返回的是元组（x，y）

1.  **元组：不可变**

> shopping_list("aa","bb","bb")

# 6、用户输入：

> input("给用户的提示")

> 获取输入：user_info=input("给用户的提示")

> input输入一律是字符串，可以转换，int（user_info）

# 7、条件：单条件、多条件（哪个先满足就退出）、嵌套条件

> if 条件一：

> [条件为真时的执行语句]

> [需要缩进四个空格]

> elif 条件二：

> [条件为真时的执行语句]

> [需要缩进四个空格]

> else：

> [条件为假时的执行语句]

> [需要缩进四个空格]

> 等于号：==

> 不等于号：!=

> <、>、>=、

# 8、逻辑运算：

> 与、或、非（or、and、not，not优先级最高）

> 多条件时可以简洁代码

# 9、循环：

+   **For**

> for xx in xxxx:

> if xx>3:

> print("哈哈")

> 从一个字典中判断key，输出满足条件的value：

> for xx,yy in dict.items():

> if xx>3:

> print(yy)

> for xx in range(1起始,5结尾不包括这个值,2步长)：

+   **while（循环次数当前未知）**

> while 条件：

> [条件为真时执行语句]

# 10、格式化：

> "{0}你好，你的成绩是{1:2f}".format(变量1，变量2)

> "{变量1}你好，你的成绩是{变量2:2f}".format(变量1，变量2)

# 11、函数：

> 函数名（参数）：print("hello world")

> 定义函数：def 函数名（参数1，参数2）：

> 定义函数不会执行代码，只有调用才会。

> 如果需要函数返回一个值：return a

>

# 12、面向对象编程：OOP，模仿世界

> **面向过程编程**解决具体case，一步一步写出来；面向对象用于解决复杂问题，先看这个对象有哪些性质，能干嘛，然后再考虑对象之间的交互，比如人和洗衣机

> c纯面向过程，java纯面向对象。

1.  类：

> 造房子的图纸，一个事情既有属性还有方法（能干嘛），具体说方法是类的函数，属性是类的变量

> **封装：**写类的人考虑细节，使用类的人不需要了解，照说明书用就行。

> **继承：**父类和子类

> **多态：**子类还能有自己的属性和方法，不需要定义在父类

> Class 类的名字（类名UserName风格命名）：

+       定义属性（_init_构造属性的方法，self代表类自己,self.xx代表类的参数)

+       定义方法

> Class CuteCat：

> def _init_(self，cat_name，cat_age，参数3)：

> self.name=cat_name

> self.age=cat_age

> def speak（self，language，参数2）：

> print（"喵"*self.age）

> 创建实例：

> cat1=CuteCat（"JOJO",2,"白色"）

> cat1.speak（'中文'）

> print(cat1.name)

>

1.  实例：造的具体房子

>

# 13、读写文件：

> 绝对路径：C:\home\data\a.py

> 相对路径：

+   读文件：以字符串返回，读完返回空字符串""

> f=open（"C:\home\data\a.py","r"，encoding="utf-8"）

> r:只读，r+：读+追加写都行，w：只写，会清空原文档，a：附加内容

> print(f.read())#读全部内容，（11）可以传读11字节

> print(f.readline())#只读一行

> print(f.readlines())#以行为单位，全部读完

> f.close()#一定记得关文件

+   写文件

> f.write("hello\n")

# 14、提前捕捉异常类型

> 和条件语句一样，except只会执行第一个，其他不执行，else没有错误时会执行，finally无论错误发生与否都执行。

> try：

> 这是程序

> 这是程序

> except 错误类型1（比如 Value Error）：

> print（"错误出现后的提示等"）

> except 错误类型2（比如 Value Error）：

> print（"错误出现后的提示等"）

> except ：#会捕捉所有错误类型

> print（"错误出现后的提示等"）

> else：

> print（"错误出现后的提示等"）

> finally：

> print（"错误出现后的提示等"）

# 15.测试

> assert

# 16.高阶函数

> 把函数当参数

> aaa_1（参数，abc_2）