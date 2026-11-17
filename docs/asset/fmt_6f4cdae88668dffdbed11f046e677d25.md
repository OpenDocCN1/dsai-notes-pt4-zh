
```
import asyncio
from playwright.async_api import async_playwright
import nest_asyncio

# 允许 Jupyter Notebook 运行异步任务
nest_asyncio.apply()

async def run():
    async with async_playwright() as p:
        browser = await p.chromium.launch(
            headless=False,  # 设为 False 以可视化运行浏览器
            args=[
                &quot;--disable-extensions&quot;,  # 禁用扩展程序
                &quot;--disable-file-system&quot;  # 禁用文件系统访问
            ]
        )
        page = await browser.new_page()
        await page.set_viewport_size({&quot;width&quot;: 1024, &quot;height&quot;: 768})  # 设置窗口大小
        await page.goto(&quot;https://bing.com&quot;)  # 访问 Bing 网站
        await page.wait_for_timeout(10000)  # 等待 10 秒
        await browser.close()  # 关闭浏览器

# 运行异步任务
asyncio.run(run())
```

![](img/53dc3d3a99f2deb5467656d51ca9d767.png)

# 4. CUA 集成流程

要在你的应用程序中集成 `Computer Use` 工具，你需要遵循以下步骤：

1️⃣ 向模型发送请求（Send a request to the model）

+ 在请求中包含 `Computer Use` 工具，作为可用工具的一部分。

+ 指定显示尺寸（display size）和环境信息（environment）。

+ 可选：在首次请求中附加环境的初始状态截图，帮助模型理解当前界面。

2️⃣ 接收模型的响应（Receive a response from the model）

+ 检查模型返回的响应中是否包含 `computer_call` 项目。

+ `computer_call` 代表模型建议的下一步操作，例如：

+ 点击（clicking）指定位置

+ 输入文本（typing in text）

+ 滚动页面（scrolling）

+ 等待（waiting）

3️⃣ 执行请求的操作（Execute the requested action）

+ 通过代码在计算机或浏览器环境中执行模型建议的操作。

4️⃣ 捕获更新后的界面状态（Capture the updated state）

+ 执行操作后，截取当前界面的最新状态作为截图，用于下一步交互。

5️⃣ 循环执行（Repeat）

+ 将更新后的状态作为 `computer_call_output`，并发送新的请求。

+ 继续这个循环交互，直到：

+ 模型不再请求新操作，或

+ 你决定终止执行。

![](img/0a0c24f86be97eac72de6e75c437140e.png)

具体代码执行流程如下：

## 1. 向模型发送请求（Send a request to the model）

发送请求以创建 `Response`，使用 `computer-use-preview` 模型，并启用 `computer_use_preview` 工具。

该请求应包括环境的详细信息，以及初始输入提示（`input prompt`）。

📌 可选：

你可以附加环境的初始状态截图，帮助模型更好地理解当前界面。

📌 注意：

要使用 `computer_use_preview` 工具，必须将 `truncation` 参数设置为 `"auto"`。

> （默认情况下，`truncation` 是禁用的）。

```
from openai import OpenAI
client = OpenAI()

response = client.responses.create(
    model=&quot;computer-use-preview&quot;,
    tools=[{
        &quot;type&quot;: &quot;computer_use_preview&quot;,
        &quot;display_width&quot;: 1024,
        &quot;display_height&quot;: 768,
        &quot;environment&quot;: &quot;browser&quot; # other possible values: &quot;mac&quot;, &quot;windows&quot;, &quot;ubuntu&quot;
    }],
    input=[
        {
            &quot;role&quot;: &quot;user&quot;,
            &quot;content&quot;: &quot;Check the latest OpenAI news on bing.com.&quot;
        }
        # Optional: include a screenshot of the initial state of the environment
        # {
        #     type: &quot;input_image&quot;,
        #     image_url: f&quot;data:image/png;base64,{screenshot_base64}&quot;
        # }
    ],
    reasoning={
        &quot;generate_summary&quot;: &quot;concise&quot;,
    },
    truncation=&quot;auto&quot;
)

print(response.output)
```

📌 效果：

+ 该 API 请求会启动计算机代理，让它在 Bing 上搜索 OpenAI 新闻。

+ 可选：传入界面截图（Base64 编码）以帮助模型理解当前环境。

## 2. 接收建议操作

模型的 API 响应可能包含：

+ 文本输出

+ 计算机操作（`computer_call`）

+ 其他工具调用

📌 示例：模型返回点击操作

```
&quot;output&quot;: [
    {
        &quot;type&quot;: &quot;reasoning&quot;,
        &quot;id&quot;: &quot;rs_67cc...&quot;,
        &quot;content&quot;: []
    },
    {
        &quot;type&quot;: &quot;computer_call&quot;,
        &quot;id&quot;: &quot;cu_67cc...&quot;,
        &quot;call_id&quot;: &quot;call_zw3...&quot;,
        &quot;action&quot;: {
            &quot;type&quot;: &quot;click&quot;,
            &quot;button&quot;: &quot;left&quot;,
            &quot;x&quot;: 156,
            &quot;y&quot;: 50
        },
        &quot;pending_safety_checks&quot;: [],
        &quot;status&quot;: &quot;completed&quot;
    }
]
```

📌 解析：

+ `computer_call`：告诉应用程序执行点击操作（在 `x=156, y=50`）。

+ `reasoning`：提供推理过程（如果有）。

+ `pending_safety_checks`：如果存在安全检查，应用程序需要处理。

## 3. 执行操作

在计算机或浏览器环境中执行模型建议的操作。

📌 如何通过代码将 `computer_call` 映射到具体操作 取决于你的环境。

```
def handle_model_action(page, action):
    &quot;&quot;&quot;
    Given a computer action (e.g., click, double_click, scroll, etc.),
    execute the corresponding operation on the Playwright page.
    &quot;&quot;&quot;
    action_type = action.type
    
    try:
        match action_type:

            case &quot;click&quot;:
                x, y = action.x, action.y
                button = action.button
                print(f&quot;Action: click at ({x}, {y}) with button &#x27;{button}&#x27;&quot;)
                # Not handling things like middle click, etc.
                if button != &quot;left&quot; and button != &quot;right&quot;:
                    button = &quot;left&quot;
                page.mouse.click(x, y, button=button)

            case &quot;scroll&quot;:
                x, y = action.x, action.y
                scroll_x, scroll_y = action.scroll_x, action.scroll_y
                print(f&quot;Action: scroll at ({x}, {y}) with offsets (scroll_x={scroll_x}, scroll_y={scroll_y})&quot;)
                page.mouse.move(x, y)
                page.evaluate(f&quot;window.scrollBy({scroll_x}, {scroll_y})&quot;)

            case &quot;keypress&quot;:
                keys = action.keys
                for k in keys:
                    print(f&quot;Action: keypress &#x27;{k}&#x27;&quot;)
                    # A simple mapping for common keys; expand as needed.
                    if k.lower() == &quot;enter&quot;:
                        page.keyboard.press(&quot;Enter&quot;)
                    elif k.lower() == &quot;space&quot;:
                        page.keyboard.press(&quot; &quot;)
                    else:
                        page.keyboard.press(k)
            
            case &quot;type&quot;:
                text = action.text
                print(f&quot;Action: type text: {text}&quot;)
                page.keyboard.type(text)
            
            case &quot;wait&quot;:
                print(f&quot;Action: wait&quot;)
                time.sleep(2)

            case &quot;screenshot&quot;:
                # Nothing to do as screenshot is taken at each turn
                print(f&quot;Action: screenshot&quot;)

            # Handle other actions here

            case _:
                print(f&quot;Unrecognized action: {action}&quot;)

    except Exception as e:
        print(f&quot;Error handling action {action}: {e}&quot;)
```

## 4. 获取更新后的界面状态

执行操作后，截取当前环境的最新状态截图，以反映操作后的界面变化。

📌 注意：

+ 截图的获取方式因环境不同而有所不同。

+ 你需要根据具体环境选择合适的方法来捕获更新后的界面状态。

```
def get_screenshot(page):
    &quot;&quot;&quot;
    Take a full-page screenshot using Playwright and return the image bytes.
    &quot;&quot;&quot;
    return page.screenshot()
```

📌 效果：

+ 代码会截图当前屏幕，并将其作为 `input_image` 传给模型，继续执行任务。

## 5. 循环执行（Repeat）

当你获取到最新的截图后，可以将其作为 `computer_call_output` 发送回模型，以获取下一步操作。

📌 循环执行：

+ 只要模型返回 `computer_call` 项目，就继续按照「发送请求 → 执行操作 → 获取截图」的流程重复执行，直到模型不再请求新操作。
