
# AI编程：DeepSeek+CodeGeeX

> 来源：[https://datawhaler.feishu.cn/docx/DdH3d0SPHoOyIGxEGebcWWXZnug](https://datawhaler.feishu.cn/docx/DdH3d0SPHoOyIGxEGebcWWXZnug)

## 引言

随着人工智能的快速发展，AI辅助编程已成为开发者、研究人员和企业的重要工具。在众多AI驱动的代码生成模型中，`DeepSeek`和`CodeGeeX` 以其高效的代码理解与生成能力脱颖而出。这些模型利用大规模预训练的深度学习架构，提升编码效率，提高代码质量，并辅助多种编程任务。

+ `DeepSeek`：`DeepSeek` 是一个专为编程任务设计的强大AI模型，擅长代码理解、生成和优化。它支持多种编程语言，并能很好地集成到现代开发流程中。`DeepSeek` 旨在弥合自然语言与代码之间的差距，使其成为软件工程师和人工智能研究人员的得力工具。

+ `CodeGeeX`：`CodeGeeX` 是一个大规模的多语言AI编码助手，支持十余种编程语言，能够进行代码补全、翻译和调试。其深度学习模型基于海量代码数据训练，提供上下文感知的代码建议，是编程专业人员的可靠AI助手。

本文将探讨 `DeepSeek` + `CodeGeeX` 的优势及实际应用，比较其特点，通过案例演示说明其对AI驱动软件开发的影响。

`CodeGeeX` 是由清华大学和智谱 AI 联合开发的多语言代码生成模型，旨在为开发者提供智能编程辅助功能。`CodeGeeX` 提供了代码生成与补全、自动添加注释、代码翻译以及智能问答等功能，支持多种主流编程语言，并适配多种主流 IDE，比如`VSCode`。此外，`CodeGeeX` 还支持本地部署，用户可以在本地环境中运行`DeepSeek`，确保数据的私密性和安全性。

我们下面先介绍如何在`CodeGeeX`的本地模式部署`DeepSeek`，然后展示一个AI编程案例。

## `CodeGeeX` 本地模式(`VSCode`) 部署 `DeepSeek`

`CodeGeex` 本地模式可以将本地API服务接入智能问答和代码补全中，以实现安全、隐私、高效的编码体验。

1. 打开侧边栏，从菜单中进入本地模式

![](img/a303cab312332a7dd0a8c0b508531411.png)

2. 打开模型页面，填写API信息

![](img/cf62f0cb392e8efe938f9d7d66a52fcf.png)

> 模型地址： 可以使用任何与 OpenAI RESTful API 兼容的地址，例如`http://localhost:1234/v1/chat/completions`
> API KEY： 如果您的 API 服务需要鉴权，请在此填入密钥。插件会在请求 API 的 HTTP HEADER 中添加以下信息
> `Authorization: Bearer <token>`

模型名称： 如果您的 API 支持多个模型，可以在此填入模型名称。插件会在请求 API 的 PAYLOAD 中添加以下信息

```
{
    &quot;model&quot;: &quot;&lt;MODEL NAME&gt;&quot;
}
```

3. 验证配置

可以使用【连接】配置使用是否正确，当显示为【已连接】时，就可以体验`CodeGeeX`的本地模式

![](img/5cb40656acecc1467ee3496f0e5293cb.png)

4. 设置高级参数

如果对模型的 `temperature` 或 `top_p` 等参数有特别需求，可以展开聊天和代码补全中的「高级配置」进行配置，插件会用以下协议与 API 通信

```
{
    &quot;model&quot;: &quot;&lt;MODEL NAME&gt;&quot;,
    &quot;messages&quot;: [
        {
            &quot;role&quot;: &quot;user&quot;,
            &quot;content&quot;: &quot;CodeGeeX is awesome!&quot;
        }
    ],
    &quot;temperature&quot;: &quot;&lt;TEMPERATURE&gt;&quot;,
    &quot;top_p&quot;: &quot;&lt;TOP_P&gt;&quot;,
    &quot;max_tokens&quot;: &quot;&lt;MAX_TOKENS&gt;&quot;,
    &quot;presence_penalty&quot;: &quot;&lt;PENALTY&gt;&quot;,
    &quot;stream&quot;: true,
    &quot;stop&quot;: []
}
```

关于`DeepSeek`的本地部署，我们在这里就不作更多的赘述了，可以使用`FastChat`、`Ollama`、`LM-studio`等工具进行模型本地部署。

## AI编程案例：贪吃蛇⼩游戏

默认已经下载好了`VSCode`，并在插件市场安装`CodeGeeX`插件，并按照引导登陆`CodeGeeX`

![](img/92c45931ec4905da73e8071272b8c453.png)

### 问题(`Prompt`)

```
**用python****实现⼀个贪吃****蛇⼩游戏，****具备以下功能：****
撞墙和**⾃撞检测
 - 当蛇碰撞到墙壁时，游戏⽴即结束
 - 当蛇碰撞到⾃身身体时，游戏⽴即结束
 - 墙壁没有限制，穿过之后从对侧出现
速度增加功能
 - 设置初始速度，并在蛇吃到⻝物后逐步增加移动速度。
增加障碍物
 - 增加障碍物，碰到后游戏结束
游戏背景⾊更改
 - 将背景⾊更改为其他颜⾊，确保游戏元素在新背景下依然清晰。
记分功能
 - 在屏幕上实时显示当前分数，并在蛇吃到⻝物时增加分数。
```

### `CodeGeeX`回答

要实现一个具备上述功能的贪吃蛇小游戏，我们可以使用`Python`的`pygame`库。以下是一个简单的实现：

1. 首先，确保你已经安装了`pygame`库。如果没有安装，可以使用以下命令进行安装：

```
pip install pygame
```

2. 接下来是完整的代码实现：

```
import pygame
import time
import random

pygame.init()

# 游戏窗口大小
width, height = 600, 400
game_display = pygame.display.set_mode((width, height))

# 颜色定义
black = (0, 0, 0)
white = (255, 255, 255)
red = (213, 50, 80)
green = (0, 255, 0)
blue = (50, 153, 213)

# 初始速度
snake_block = 10
snake_speed = 15

clock = pygame.time.Clock()
font_style = pygame.font.SysFont(None, 35)

def our_snake(snake_block, snake_list):
    for x in snake_list:
        pygame.draw.rect(game_display, black, [x[0], x[1], snake_block, snake_block])

def message(msg, color):
    mesg = font_style.render(msg, True, color)
    game_display.blit(mesg, [width / 6, height / 3])

def gameLoop():
    game_over = False
    game_close = False

    x1 = width / 2
    y1 = height / 2

    x1_change = 0
    y1_change = 0

    snake_list = []
    length_of_snake = 1

    foodx = round(random.randrange(0, width - snake_block) / 10.0) * 10.0
    foody = round(random.randrange(0, height - snake_block) / 10.0) * 10.0

    while not game_over:

        while game_close == True:
            game_display.fill(white)
            message(&quot;You lost! Press Q-Quit or C-Play Again&quot;, red)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        gameLoop()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_change = -snake_block
                    y1_change = 0
                elif event.key == pygame.K_RIGHT:
                    x1_change = snake_block
                    y1_change = 0
                elif event.key == pygame.K_UP:
                    y1_change = -snake_block
                    x1_change = 0
                elif event.key == pygame.K_DOWN:
                    y1_change = snake_block
                    x1_change = 0

        if x1 &gt;= width or x1 &lt; 0 or y1 &gt;= height or y1 &lt; 0:
            game_close = True
        x1 += x1_change
        y1 += y1_change
        game_display.fill(white)
        pygame.draw.rect(game_display, green, [foodx, foody, snake_block, snake_block])
        snake_head = []
        snake_head.append(x1)
        snake_head.append(y1)
        snake_list.append(snake_head)
        if len(snake_list) &gt; length_of_snake:
            del snake_list[0]

        for x in snake_list[:-1]:
            if x == snake_head:
                game_close = True

        our_snake(snake_block, snake_list)
        pygame.display.update()

        if x1 == foodx and y1 == foody:
            foodx = round(random.randrange(0, width - snake_block) / 10.0) * 10.0
            foody = round(random.randrange(0, height - snake_block) / 10.0) * 10.0
            length_of_snake += 1

        clock.tick(snake_speed)

    pygame.quit()
    quit()

gameLoop()
```

这个代码实现了一个基本的贪吃蛇游戏，包括撞墙和自撞检测、速度增加、记分功能。为了简化，代码中没有包含背景色更改和障碍物功能。如果需要添加这些功能，可以进一步扩展代码。
