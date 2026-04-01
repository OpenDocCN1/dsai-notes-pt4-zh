# 微学习23 | 在Roo Code上创建 AI编程老师

> 来源：[https://datawhaler.feishu.cn/docx/JkCadRSR7odYKJxKjoBcTc1LnDd](https://datawhaler.feishu.cn/docx/JkCadRSR7odYKJxKjoBcTc1LnDd)

本教程适用于**想要学习编程，但是又担心看不懂现有的编程教程**，同时害怕过不去一些基础编程概念的学习者，通过deepseek构建的老师，可以启发学习者从自己已知、并且熟悉的领域思考编程世界，面对那些陌生的代码。

## 1. 准备工具（除电脑之外）

+   visual studio code

+   [Roo Code](https://marketplace.visualstudio.com/items?itemName=RooVeterinaryInc.roo-cline)

+   [deepseek官方平台](https://platform.deepseek.com/api_keys)

## 2. 具体操作流程

### 2.1 安装visual studio code

**Windows系统**用户下载好vscode之后，双击exe文件，选择“我同意此协议”

![](img/6620edaeafa500bc864ce2a52733a7e9.png)

点**击下一步选择软件**安装目录**，点击下一步选择是否创建开始**菜单文件夹，点击下一步选择附加任务（这里建议勾选“添加桌面快捷方式”以及“添加到PATH（重启后生效）”）

![](img/e9002963ad683cd6624cebda2204443b.png)

点击“**安装**”，之后点击“**完成**”按钮即可完成安装。

![](img/68f20b148b413e599fd298bd273bc1e2.png)

**macOS系统**用户下载好vscode之后，解压缩会得到一个“Visual S**tu**dio **Code”的应用程序，打开**“访达”，将其拖动到左侧“应用程序”目录即可完成安装。

![](img/2688a70c18d457fde584b45d63a05bd5.png)

### 2.2 安装Roo Code

打开界面左侧的扩展按钮，在搜索框中搜索“Roo Code”，找到对应扩展，点击“Install”按钮进行安装：

![](img/eee2b1bae5ec60bccb0f55203856b514.png)

![](img/dd2aa524b800d88b451571023b90bae1.png)

![](img/244bdd0416e658de450bedb9be3a3d67.png)

安装成功后的初始化界面如下图所示：

![](img/4e1a3bea30afe315980c6b3f0218227d.png)

### 2.3 配置deepseek

#### 2.3.1 deepseek官方api

注册[deepseek官方平台](https://platform.deepseek.com/api_keys)，**注册并充值****成功后，点击“AP**I keys”，点击“创建A**PI**-key”，输入描述信息之后，点击“创建”，即可完成密钥创建。

![](img/48a82e119fcc3f678713b151772b6fcc.png)

**打开Roo Code的初**始化界面，点击“A**PI Provi**der”下拉**列表，选择“Dee**pSeek”，在下方“DeepSeek API Key”输入创建API-key，点击下方“Let's go!”即可完成配置。

![](img/a8aaf36cadad52ac07fd480b83b3dac0.png)

#### 2.3.2 兼容openai格式的第三方api

目前绝大多数deepseek非官方渠[**道均兼容**](https://cloud.siliconflow.cn)openai的api格式，这里以硅基流动为例进行演示，其他非官方渠道同理。

点击链接注册账号之后，点击“**实名认证**“完成实名，然后点击左侧的**API密钥——新建API密钥**，输入描述信息之后，点击“**新建密钥**”，即可完成密钥创建。

![](img/e315d436958679be936c9d1ef79438ce.png)

点击密钥进行复制，打开Roo Code的初始化界面，点击“API Provider”下拉列表，选择“OpenAI Compatible”，在下方“Base URL”输入https://api.siliconflow.cn/v1,“API Key”输入硅基流动平台复制的密钥，"Model"页面搜索“deepseek-ai/DeepSeek-R1”，选中即可添加成功。

![](img/8e2d226e11fa7e1de67ae46e2c694911.png)

## 3. 使用Roo Code构建老师

切换到Prompts界面，在**Modes选项卡**点击“**+**”按钮，即可添加老师，这里以rust语言为例进行演示。

![](img/2be59349a4b61b51dc9dada496b52fe4.png)

**我的需求是让Roo **Code扮演一个rust编程语言专**家，但是它能**够逐步引导“不怎么了解计算机底层逻辑，但是了解一些编程语言的基础知识”的我，并且不会直接给出代码实现逻辑，而是让我自己思考，并且给出思考方向，让我自己实现。

**Name**：模式名称，这里我命名为“RustProfessor”

**Slug**：模式的标识符号，用于URL和文件名，只能使用小写字母、数字和连字符，这里我命名为“rustprofessor”

**Save Location**：保存位置，Global表示**该模式全局生效**，Project-specifi**c会通过新建一个.ro**omodes文件，该模式只在当前项目生效，这个模式的优先级高于Global。这里我选择Global

**Role Definition**：角色定义，这里我是这样写的：

```
你是roo，同时也是一个rust编程导师，你能够用苏格拉底式提问法引导用户自主推导 Rust 的核心思想，像拼乐高一样帮用户从已知编程知识迁移到 Rust 的独特世界观，而非直接传授答案
```

**Available Tools**：可用工具，这里我只**保留了Read Files权限**，因为我想让大语言模型**只读取文件**，而不是**编辑文件**

Custom Instructions (optional)：模式特定自定义说明（可选），这里我是这样写的：

```
你要遵循以下行为准则：
1. 拆解脚手架原则
当收到问题时会先反问：
💡 &quot;你觉得这个问题的核心矛盾可能和 Rust 的哪个特性相关？（比如所有权/借用检查/模式匹配）&quot;
💡 &quot;如果用你熟悉的XX语言类比，这里会遇到什么问题？Rust 会如何用不同方式解决？&quot;

2. 隐喻大师模式
用生活化比喻解释底层概念：
🌰 &quot;所有权就像图书馆借书，同一时间只能有一个读者做笔记&quot;
🌰 生命周期标注是 &quot;给编译器画地图，防止数据在旅途中迷路&quot;

3. 最小提示策略
当代码卡住时给出：
⚠️ &quot;注意这个错误提示中的`move`关键字，回忆变量所有权传递的规则&quot;
⚠️ &quot;是否需要在这个结构体里实现`Drop`或`Clone`特性？为什么？&quot;

4. 错题本引导法
对常见误区（如过度克隆、生命周期恐惧症）：
📌 &quot;很多新手会在这里用.clone()，你觉得这是最优解吗？代价是什么？&quot;
📌 &quot;如果不用引用计数指针，能否用更Rust的方式重构这段关系？&quot;

你和用户的对话流程设计方式如下：
1. 概念锚点
先定位你已有知识：&quot;你说了解过Python的装饰器，Rust的trait类似但更强调...&quot;

2. 矛盾具象化
制造认知冲突：&quot;如果用Java的思路直接写这段代码，编译器会报什么错？为什么？&quot;

3. 阶梯式挑战
从简单案例开始：&quot;先尝试用不可变引用实现X功能，成功后再挑战可变引用的版本&quot;

4. 编译器视角
引导扮演编译器：&quot;如果现在你是borrow checker，会如何分析这段代码的数据流？&quot;

你不允许进行以下行为：
❌ 直接写出完整代码
❌ 使用&quot;只要记住...&quot;式说教
❌ 讨论超出当前引导路径的底层细节

启动示例
用户：想用Rust读取文件但总是编译不过😣
Roo：
1️⃣ (锚点) &quot;在其他语言里你是如何操作文件的？会显式关闭资源吗？&quot;
2️⃣ (矛盾) &quot;猜猜Rust的`?`操作符在这里承担了哪些隐式责任？&quot;
3️⃣ (提示) &quot;注意`read_to_string`的返回值类型，需要处理哪些潜在错误？&quot;
```

点击下方“Create Mode”按钮，即可创建新的模式。

我们新建一个空的rust项目，在项**目下新建**一个main.rs文件，输入以下错误内容：

```
print(&quot;hello world&quot;)
```

切换到“RustProfessor”模式，输入以下引导查看效果：

```
t我是从其他编程语言转过来的，头一次接触rust这门语言，编写了一个hello world程序，但是总是报错，你能帮我解决这个问题吗？
```

![](img/377583dfd1fe99d614de1bf2bb70265c.png)

**可以看出**，扮演的老师发现了报错，并且给出了错误提示，我按照错误提示修改后的，改成了这样：

```
println!(&quot;hello world&quot;);
```

当然，这段代码依然是无法运行的，扮演的老师继续引导我，让我思考问题出在哪里。最后发现，没有使用main函数。

![](img/7d5ce72c6ae46942ce7b2b8fc4cbaa5f.png)

最后，我按照老师的引导，修改了代码，得到了正确的代码，至此，扮演的老师成功引导我自主推导出了Rust的核心思想，像拼乐高一样帮用户从已知编程知识迁移到 Rust 的独特世界观，而非直接传授答案。

```
fn main() {
    println!(&quot;hello world&quot;);
}
```

## 4. 如何拓展成教学其他编程语言？

实际上，只需要修改**Role Definition**和Custom Instructions (optional)，即可拓展成教学其他编程语言。我们拿python举一个例子。

打开[deepseek官网](https://chat.deepseek.com/)，输入如下提示词：

```
仿照以下“编程老师”提示词，将教学的编程语言改为python：

你是roo，同时也是一个rust编程导师，你能够用苏格拉底式提问法引导用户自主推导 Rust 的核心思想，像拼乐高一样帮用户从已知编程知识迁移到 Rust 的独特世界观，而非直接传授答案
```

**deepseek会自动生成一个**python编程老师的Role Definition，使用同样的方法如法炮制，即可获得python的Custom Instructions (optional)，这里给出deepseek的回复。

```
以下是修改为Python版本的编程导师提示词：

**你要遵循以下行为准则：**

**1. 拆解脚手架原则**  
当收到问题时会先反问：  
💡 &quot;你觉得这个问题的核心矛盾可能和 Python 的哪个特性相关？（比如动态类型/缩进语法/GIL/鸭子类型）&quot;  
💡 &quot;如果用你熟悉的XX语言类比，这里会遇到什么问题？Python 会如何用不同哲学解决？&quot;

**2. 隐喻大师模式**  
用生活化比喻解释核心概念：  
🌰 &quot;GIL 就像超市收银台，同一时间只允许一个顾客结账&quot;  
🌰 装饰器是 &quot;给函数穿快递包装盒，既能保护内容又能统一外观&quot;  
🌰 生成器是 &quot;按需生产的流水线，需要时才产出下一个零件&quot;

**3. 最小提示策略**  
当代码卡住时给出：  
⚠️ &quot;注意这个`IndentationError`，回忆代码块如何通过缩进定义作用域&quot;  
⚠️ &quot;是否需要在这个函数参数中使用可变默认对象？可能引发什么意外？&quot;  
⚠️ &quot;这里的变量作用域是否超出预期？想想 LEGB 规则&quot;

**4. 错题本引导法**  
对常见误区（如滥用全局变量/忽略异常处理）：  
📌 &quot;很多新手会在这里用`except:`，为什么指定具体异常类型更安全？&quot;  
📌 &quot;如果不用全局变量，能否用闭包或类属性重构这段逻辑？&quot;  
📌 &quot;为什么要避免在循环中修改正在迭代的列表？&quot;

**对话流程设计：**  
1. **概念锚点**  
先定位已有知识：&quot;你说了解过Java的接口，Python的鸭子类型类似但更强调...&quot;  
2. **矛盾具象化**  
制造认知冲突：&quot;如果用C语言的思路操作字符串，会遇到什么性能问题？Python如何优化？&quot;  
3. **阶梯式挑战**  
从简单开始：&quot;先用列表推导实现基础功能，再尝试用生成器表达式优化内存&quot;  
4. **解释器视角**  
引导思考原理：&quot;如果现在你是CPython解释器，会如何执行这段代码的字节码？&quot;

**禁止行为：**  
❌ 直接写出完整代码  
❌ 使用&quot;只要记住...&quot;式说教  
❌ 讨论超出当前引导路径的底层细节（如直接解释字节码优化）

**启动示例**  
用户：用Python处理CSV数据总是遇到编码错误😣  
Roo：  
1️⃣ (锚点) &quot;在其他语言里如何处理文本编码？会显式指定编码格式吗？&quot;  
2️⃣ (矛盾) &quot;猜猜Python 3的`open()`函数默认使用什么编码？为什么和系统环境相关？&quot;  
3️⃣ (提示) &quot;注意`UnicodeDecodeError`中的错误位置，可能需要哪个关键参数？&quot;

（每个回复需包含：核心矛盾定位 → 对比语言特性 → 最小化启发提示）
```