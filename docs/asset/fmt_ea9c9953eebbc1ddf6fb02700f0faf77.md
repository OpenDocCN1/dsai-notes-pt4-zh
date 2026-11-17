
# ChatGPT 编程系列

> 来源：[https://my.feishu.cn/docx/ZDhWdEWfuoCooJxQ7Eacybbfn9Z](https://my.feishu.cn/docx/ZDhWdEWfuoCooJxQ7Eacybbfn9Z)

[转自小林](https://xiaobot.net/p/xiaolin?refer=72e0e5d2-1313-47b5-9a21-77bde98ee72c)，如果你想阅读最新完整版，请点击查看

## ChatGPT 辅助编程 | 专栏 1 核心要素

如果你看过我写的写作核心，你一定也能理解编程核心。

因为借助 GPT 辅助写作或者辅助编程，本质都是在辅助你提高某项技能，啥意思呢？

**意思就是借助 GPT 辅助编程的核心一样是要能提出好问题。**

GPT 是工具，不是人，它理解不了需求不清晰的项目，理解不了用户心理，这些都需要【人】，也就是【你】来实现。

举个例子：在 IT 界，不同职位的程序员在写代码中都会遇到各种难度不一的问题。

于是便去🛰️、🐧 搜索相对应的交流群。例如：C 语言聊、Java 群。

加群之后不同的人有独属于自己的提问方法：

秃头甲：有人在么？

秃头乙：有大佬在么，想问个题？

秃头丙：有人做过什么项目么，想取取经？

秃头丁：各位大佬好，我是 C 语言初学者，请问怎么快速入门？

……

前三种回复：不在，在了也不回你，回你也不是这个问题。

第四种回复：群里开始讨论怎么入门，最终大家一致同意：放弃学 C 语言是最快的入门方法

所以说理解了么，你问人的问题都描述不清，你问 GPT 它更不知道了。

那么如何正确的提问呢？

**今天先说一点，也是最重要的：**你要向 AI 说清楚要用什么语言来实现。

这点很重要，前两天我直播用 AI 来回答问题，很多观众上来就是这个题怎么做，那个题怎么做。

AI 给出回答后，对方还理直气壮的问，为什么 AI 用了 Python ，没有用 Java。

拜托啊，你提问的时候就没告诉人家要用什么语言写好吧。

## ChatGPT 辅助编程 | 专栏 2 案例演示

我：假如你是一名程序员（角色），现在帮我用 C 语言写个冒泡排序（任务），要求代码有思路和注释（指令）。

下述是 GPT 的回答：

角色：程序员

任务：用 C 语言写一个冒泡排序

指令：

1. 定义并初始化一个需要排序的数组，例如 `int array[] = {5， 3， 8， 4， 2}`；
2. 利用两层 `for` 循环进行排序，外层循环控制排序的次数，内层循环控制每次比较相邻的两个数。
3. 在内层循环中，依次比较相邻的两个数，如果前面的数大于后面的数，则交换两个数的位置。
4. 继续进行排序，直到所有的数都按照从小到大的顺序排列好。
5. 冒泡排序的时间复杂度较高，不适合对大数组进行排序。

代码如下（已注释说明）：

```
#include 

int main() 
{
    int array[] = {5, 3, 8, 4, 2}; // 定义并初始化需要排序的数组
    int len = sizeof(array) / sizeof(array[0]); // 数组长度
    int i, j, temp;

    // 冒泡排序
    for (i = 0; i < len - 1; i++)
    {
        for (j = 0; j < len - i - 1; j++)
        {
            if (array[j] > array[j+1]) // 如果前面的数比后面的数大，则交换位置
            {
                temp = array[j];
                array[j] = array[j+1];
                array[j+1] = temp;
            }
        }
    }

    // 输出排序后的结果
    printf("排序后的结果为：\n");
    for (i = 0; i < len; i++)
    {
        printf("%d ", array[i]);
    }
    
    return 0;
}
```

以上，学废了嘛，

## ChatGPT 辅助编程 | 专栏 3 帮你学 C 语言

在利用 ChatGPT 辅助学 C 语言之前，你得先知道，它到底能辅助你干什么。

**我：作为一个语言模型，你能给 C 语言初学者提供什么帮助，请罗列具体的点。**

ChatGPT：作为一个语言模型，我可以为 C 语言初学者提供以下帮助：

1、提供 C 语言的基础知识和语法介绍，包括数据类型、控制语句、函数、指针等等。

2、解答 C 语言相关问题，例如编译错误、运行时错误、逻辑错误等等。

3、给出一些 C 语言编程的例子和实践，帮助初学者更好地理解和掌握 C 语言编程技巧。

4、推荐一些 C 语言编程的学习资源，例如书籍、网站、视频等等，帮助初学者更系统地学习 C 语言。

5、提供一些常用的 C 语言开发工具和环境的介绍，例如 IDE、编辑器、调试器等等，帮助初学者更高效地开发 C 语言程序。

总之，我可以根据初学者的需求和问题，提供相应的帮助和支持，帮助他们更好地学习和掌握 C 语言编程技能。

**既然 ChatGPT 给了 5 点回复，后面我会根据这 5 点回复，来给大家演示，如何利用 ChatGPT 来帮助学习 C 语言。**

我今天用 ChatGPT 帮我做了个贪吃蛇小游戏，感觉还是挺好玩的。

我把代码贴出来，仅供大家参考：

```
// 我的问题是：假如你是一名初级C语言开发，帮我做个贪吃蛇，要求代码有注释。
#include <stdio.h>
#include <stdlib.h>
#include <conio.h>
#include <windows.h>

#define width 20
#define height 20

int gameover;
int score;
int x, y, fruit_x, fruit_y, tail_x[100], tail_y[100], ntail;

enum eDirection {STOP = 0, LEFT, RIGHT, UP, DOWN};
enum eDirection dir;

void Setup()
{
    gameover = 0;
    dir = STOP;
    x = width / 2;
    y = height / 2;
    fruit_x = rand() % width;
    fruit_y = rand() % height;
    score = 0;
}

void Draw()
{
    system("cls");

    // 绘制上边界
    for (int i = 0; i < width+2; i++)
        printf("#");
    printf("\n");

    // 绘制每一行
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            if (j == 0)
                printf("#");

            if (i == y && j == x)
                printf("O");
            else if (i == fruit_y && j == fruit_x)
                printf("F");
            else
            {
                int print_tail = 0;
                for (int k = 0; k < ntail; k++)
                {
                    if (tail_x[k] == j && tail_y[k] == i)
                    {
                        printf("o");
                        print_tail = 1;
                    }
                }
                if (!print_tail)
                    printf(" ");
            }

            if (j == width-1)
                printf("#");
        }
        printf("\n");
    }

    // 绘制下边界
    for (int i = 0; i < width+2; i++)
        printf("#");
    printf("\n");

    printf("Score: %d\n", score);
}

void Input()
{
    if (_kbhit())
    {
        switch (_getch())
        {
        case 'a':
            dir = LEFT;
            break;
        case 'd':
            dir = RIGHT;
            break;
        case 'w':
            dir = UP;
            break;
        case 's':
            dir = DOWN;
            break;
        case 'x':
            gameover = 1;
            break;
        }
    }
}

void Logic()
{
    // 记录贪吃蛇当前位置前进方向的尾部位置
    int prev_tail_x = tail_x[0], prev_tail_y = tail_y[0];
    int prev_tail2_x, prev_tail2_y;
    tail_x[0] = x;
    tail_y[0] = y;
    for (int i = 1; i < ntail; i++)
    {
        prev_tail2_x = tail_x[i];
        prev_tail2_y = tail_y[i];
        tail_x[i] = prev_tail_x;
        tail_y[i] = prev_tail_y;
        prev_tail_x = prev_tail2_x;
        prev_tail_y = prev_tail2_y;
    }

    // 根据方向更新贪吃蛇的头部位置
    switch (dir)
    {
    case LEFT:
        x--;
        break;
    case RIGHT:
        x++;
        break;
    case UP:
        y--;
        break;
    case DOWN:
        y++;
        break;
    }

    // 判断贪吃蛇是否撞到边界或自身
    if (x < 0 || x >= width || y < 0 || y >= height)
        gameover = 1;
    for (int i = 0; i < ntail; i++)
    {
        if (tail_x[i] == x && tail_y[i] == y)
            gameover = 1;
    }

    // 如果吃到了水果，增加分数和贪吃蛇的长度
    if (x == fruit_x && y == fruit_y)
    {
        score += 10;
        fruit_x = rand() % width;
        fruit_y = rand() % height;
        ntail++;
    }
}

int main()
{
    Setup();

    while (!gameover)
    {
        Draw();
        Input();
        Logic();
        Sleep(50);
    }

    printf("Game Over!\n");
    printf("Your Score: %d\n", score);

    return 0;
}
```

## ChatGPT 辅助编程 | 专栏 4 C 语言纠错

纠错是很多人关心的问题，因为对于初学者来说，最大的问题是非常基础的问题，实在找不到答案，问老师吧，又不敢，觉得自己的问题太 low。

去百度搜索，也不知道如何下手搜。

今天这篇就来讲一下如何利用 ChatGPT 纠错你写的 C 语言代码。

先来一个小案例。

我：请帮我检查一下，这个 C 语言代码哪里出错了，为什么编译不成功？

```
#include <stdio.h>  
int main()
{
        printf("hello 我") 
}
```

ChatGPT：你的代码缺少了结束符号，应该加上分号`;`.

以下是修改后的代码：

```
#include <stdio.h>  
int main()
{
        printf("hello 我"); 
        return 0;
}
```

同时，在 `main()` 函数中，最好添加一个 `return 0` 语句以明确表示程序正常结束。

上面这个案例是代码短，如果代码非常长，你去复制粘贴有点不现实，那应该怎么办呢？

很简单呀，先在自己的编译器上运行一下，然后将报错提示复制出来给到 ChatGPT 。

举个例子：还是刚刚的代码，这次我不贴代码，我贴报错提示。

我：我在写 C 语言代码的过程中，遇到了这样的报错，是什么原因导致的呢？

`[Error] expected ';' before '}' token`

ChatGPT：这个错误通常是由于缺少分号引起的，C 语言的语法比较严格，每个语句都必须以分号结尾。

解决方法是检查代码中是否有缺少分号的情况，特别是在函数定义、结构体定义、枚举定义等地方。另外，也要检查是否有多余的括号或者缺少括号的情况，这些也可能会导致类似的错误。

以上，学废了嘛，学会可以点个赞呀～

## ChatGPT 辅助编程 | 专栏 5 C 语言开发工具介绍

今天这篇来让 GPT 帮我介绍一些 C 语言开发工具。

我：请给我推荐一个最适合初学者的 C 语言开发工具。

GPT：推荐初学者使用 Dev-C++，它是一个免费、开源的 C/C++ 集成开发环境（IDE），具有易于使用和灵活的特点。它是一个流行的开发工具，尤其是在学习 C/C++ 编程时。

以下是一些 Dev-C++ 的特点：

1、具有易于使用的图形用户界面，适合初学者使用。

2、内置编译器，支持多种编译器，例如 MinGW 和 TDM-GCC。

3、提供了一些有用的工具，例如调试器、代码补全、语法高亮等。

4、支持多种文件格式，例如 `.cpp`、`.h`、`.c` 等。

5、具有插件扩展功能，可以通过插件添加更多的功能。

总之，Dev-C++ 是一个不错的 C/C++ 开发工具，适合初学者使用，但是它已经有些年头了，开发者也已经停止更新和维护。

我：Dev-C++ 是哪一些出现的？

Dev-C++ 开发工具是由 Bloodshed Software 公司开发的，最早发布于 1998 年。

后来，Bloodshed Software 公司停止了 Dev-C++ 的开发和维护，但是该工具的源代码被转移到了一个名为 Orwell Dev-C++ 的项目中，并由 Orwell 继续开发和维护。

Orwell Dev-C++ 是一个开源的项目，旨在提供一个现代、稳定和易于使用的 C/C++ 开发环境。

我：如果我现在想用 Dev-C++ ，应该去哪里下载？

GPT：您可以前往以下网站下载 Dev-C++：

[sourceforge.net](https://sourceforge.net/projects/orwelldevcpp/)

PS：也可以去👸号 C 语言入门到精通下载。

我：请再给我推荐一些 C 语言开发工具。

GPT：以下是一些常用的 C 语言开发工具。

1、 Visual Studio Code：一个免费的开源代码编辑器，支持 C 语言开发和调试。

2、Code::Blocks：一个免费的跨平台 C/C++ IDE，支持多种编译器和调试器。

3、Eclipse：一个开源的跨平台 IDE，支持多种编程语言，包括 C 语言。

4、NetBeans：一个免费的跨平台 IDE，支持多种编程语言，包括 C 语言。

5、Atom：一个免费的开源代码编辑器，支持 C 语言开发和调试。

6、Sublime Text：一个免费的文本编辑器，支持 C 语言开发和调试。
