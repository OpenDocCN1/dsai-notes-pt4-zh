
```python
import os
import asyncio
import logging
import time
import re
import requests
import hashlib
import random
from pathlib import Path
from playwright.async_api import async_playwright
```

## 🌐 Azure翻译API配置说明

本脚本使用Azure翻译API进行中英文翻译。

配置步骤：
1. 访问 Azure门户: https://portal.azure.com
2. 创建“翻译器”(Translator)服务
3. 获取以下信息：
   - 订阅密钥 (Subscription Key)
   - 资源位置 (Location/Region)，例如：eastus, westus2, global
4. 在 `translate_with_azure()` 函数中替换配置值

注意：Azure翻译API提供免费额度，具体限制请查看Azure定价页面

## 🎯 配置项 - 根据网络状况调整等待时间

### `WaitTimeConfig`

```python
class WaitTimeConfig:
    """等待时间配置类 - 根据网络状况调整这些值"""
    # 图片生成等待时间配置
    FIRST_STAGE_WAIT = 30  # 第一阶段等待时间（秒）- 等待最少图片出现
    MAX_GENERATION_WAIT = 360  # 最大生成等待时间（秒）- 等待所有图片完成
    # 图片生成目标数量配置
    TARGET_IMAGE_COUNT = 8  # 每个故事期望生成的图片数量（默认8张）
    # 说明：这个数量会根据用户选择的每组图片数量和估算的组数自动计算
    # 如果设置为 None，则使用自动计算：images_per_group * estimated_groups
    # 建议根据实际故事内容调整，一般每个故事包含4-6组提示词
    # 页面加载等待时间配置
    PAGE_LOAD_WAIT = 8  # 页面加载等待时间（秒）
    NAVIGATION_WAIT = 5  # 导航后等待时间（秒）
    # 滚动和检查间隔配置
    SCROLL_INTERVAL = 3  # 滚动检查间隔（秒）
    IMAGE_CHECK_INTERVAL = 2  # 图片检查间隔（秒）
    # 输入和提交等待时间配置
    INPUT_WAIT = 3  # 输入完成后等待时间（秒）
    SUBMIT_WAIT = 2  # 提交后等待时间（秒）
    # 重命名操作等待时间配置
    RENAME_WAIT = 5  # 重命名操作等待时间（秒）
    RENAME_VERIFY_WAIT = 3  # 重命名验证等待时间（秒）
```

全局配置实例

```python
WAIT_CONFIG = WaitTimeConfig()
```

## 📝 网络优化配置说明

### 🆕 新增功能：监控模式选择

现在脚本开始前会询问用户选择监控模式：
• 标准模式：启用20秒无增长判定失败（推荐）- 适合网络较好的情况
• 宽松模式：关闭20秒无增长判定，仅等待目标数量完成 - 适合网络较慢的情况

### 🌐 网络状况不佳时的调整建议

4. 图片生成目标数量：
- `TARGET_IMAGE_COUNT`: 根据故事内容调整期望的图片数量
- 设置为 `None` 时使用自动计算（推荐）
- 手动设置时建议：每组图片数 × 组数（通常4-6组）

1. 图片生成等待时间：
- `FIRST_STAGE_WAIT`: 从60秒增加到90-120秒
- `MAX_GENERATION_WAIT`: 从180秒增加到240-300秒

2. 检查间隔时间：
- `IMAGE_CHECK_INTERVAL`: 从2秒增加到3-5秒
- `SCROLL_INTERVAL`: 从3秒增加到5-8秒

3. 页面加载等待：
- `PAGE_LOAD_WAIT`: 从8秒增加到12-15秒
- `NAVIGATION_WAIT`: 从5秒增加到8-10秒

### 💡 使用技巧
- 网络很差时：所有时间都增加1.5-2倍，并选择宽松模式
- 中等网络：适当增加图片生成等待时间，可选择标准模式
- 良好网络：可以适当减少等待时间提高效率，推荐标准模式

⚠️ 注意：等待时间过短可能导致图片识别失败，过长会影响效率

## 配置日志

```python
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')
logger = logging.getLogger(__name__)
```

图片元素的类名配置

```python
IMAGE_CLASS_NAME = 'image-hDQAs3'
```

## 核心函数

### `translate_to_english`

```python
def translate_to_english(text, aspect_ratio="9:16", style_mode="realistic"):
    """使用Azure翻译API将中文翻译成英文"""
    try:
        print(f"🌐 开始翻译({len(text)}字符)...")
        # 如果文本很长，分段翻译
        if len(text) > 2000:
            return translate_long_text(text, aspect_ratio, style_mode)
        # 使用Azure翻译API
        translated = translate_with_azure(text, 1)
        if translated and len(translated.strip()) > 10:
            print(f"✅ Azure翻译成功: {len(translated)}字符")
            # 检查翻译后的内容是否过长
            if len(translated) > 8000:
                print(f"⚠️ 翻译后内容过长({len(translated)}字符)，进行适当截断...")
                truncated = translated[:7000]
                last_sentence_end = max(
                    truncated.rfind('.'),
                    truncated.rfind('!'),
                    truncated.rfind('?')
                )
                if last_sentence_end > 6000:
                    translated = truncated[:last_sentence_end + 1]
                    print(f"✅ 截断至完整句子结束: {len(translated)}字符")
                else:
                    translated = truncated
                    print(f"✅ 直接截断: {len(translated)}字符")
            return translated
        else:
            print("❌ Azure翻译失败，返回默认英文")
            return f"Generate creative images based on the story content with {aspect_ratio} aspect ratio"
    except Exception as e:
        print(f"翻译过程发生异常: {str(e)}")
        return f"Generate creative images based on the story content with {aspect_ratio} aspect ratio"
```

### `translate_long_text`

```python
def translate_long_text(text, aspect_ratio="9:16", style_mode="realistic"):
    """分段翻译长文本，保持段落结构，使用Azure翻译API"""
    try:
        # 调整为更小的每段最大字符数，以提高成功率
        max_chunk_size = 800
        # This will hold the translated chunks, where each chunk corresponds to a paragraph or sub-paragraph segment
        translated_segments = []
        # Split the original text into paragraphs first, preserving empty lines
        paragraphs = text.split('\n\n')
        # 记录翻译失败的段落数
        failed_paragraphs = 0
        total_paragraphs = len([p for p in paragraphs if p.strip()])
        for paragraph_idx, paragraph in enumerate(paragraphs):
            if not paragraph.strip(): # Handle empty paragraphs (from multiple \n\n or trailing \n\n)
                translated_segments.append("") # Preserve the blank line
                continue

            print(f"🔄 翻译段落 {paragraph_idx + 1}/{len(paragraphs)}...")
            # Process sentences within each paragraph
            sentences = re.split(r'(?<=[。！？])\s*', paragraph)
            current_chunk_for_translation = ""
            paragraph_translated_chunks = []  # 存储当前段落的翻译块
            paragraph_failed = False
            for sentence in sentences:
                sentence = sentence.strip()
                if not sentence:
                    continue

                # Check if adding the next sentence exceeds max_chunk_size
                # +1 for potential space between sentences
                if len(current_chunk_for_translation) + len(sentence) + 1 <= max_chunk_size:
                    current_chunk_for_translation += sentence + ' '
                else:
                    # Current chunk is full, translate it
                    if current_chunk_for_translation.strip():
                        try:
                            translated_text = translate_with_azure(current_chunk_for_translation.strip(), 1)
                            if translated_text and len(translated_text.strip()) > 5:
                                paragraph_translated_chunks.append(translated_text)
                                print(f"✅ 段落{paragraph_idx + 1}块翻译成功")
                            else:
                                print(f"❌ 段落{paragraph_idx + 1}中的句子块翻译失败")
                                paragraph_failed = True
                                break
                        except Exception as e:
                            print(f"❌ 段落{paragraph_idx + 1}翻译异常: {str(e)}")
                            paragraph_failed = True
                            break
                        # 增加延迟避免API限制
                        time.sleep(1)
                    # Start a new chunk with the current sentence
                    current_chunk_for_translation = sentence + ' '
            # 如果段落没有失败，翻译最后一个chunk
            if not paragraph_failed and current_chunk_for_translation.strip():
                try:
                    translated_text = translate_with_azure(current_chunk_for_translation.strip(), 1)
                    if translated_text and len(translated_text.strip()) > 5:
                        paragraph_translated_chunks.append(translated_text)
                        print(f"✅ 段落{paragraph_idx + 1}最后块翻译成功")
                    else:
                        print(f"❌ 段落{paragraph_idx + 1}的最后句子块翻译失败")
                        paragraph_failed = True
                except Exception as e:
                    print(f"❌ 段落{paragraph_idx + 1}最后块翻译异常: {str(e)}")
                    paragraph_failed = True
                time.sleep(1)
            # 检查整个段落是否翻译成功
            if paragraph_failed:
                failed_paragraphs += 1
                print(f"❌ 段落{paragraph_idx + 1}翻译失败，保持原文")
                # 对于失败的段落，保持原文
                translated_segments.append(paragraph)
            else:
                # 段落翻译成功，合并所有块
                paragraph_translation = ' '.join(paragraph_translated_chunks)
                translated_segments.append(paragraph_translation)
                print(f"✅ 段落{paragraph_idx + 1}翻译完成")
        # 合并所有翻译后的段落
        final_translation = '\n\n'.join(translated_segments)
        # 添加生成指令
        if final_translation:
            # 检查是否已包含生成指令
            has_generation_instruction = any([
                "Generate" in final_translation,
                "aspect ratio" in final_translation,
                "images" in final_translation
            ])
            if not has_generation_instruction:
                if style_mode == "cartoon":
                    final_translation += f"\n\nGenerate creative cartoon images based on the story content above. All images should be in {aspect_ratio} aspect ratio. No text should appear in the images. Maintain consistent cartoon style."
                else:  # realistic
                    final_translation += f"\n\nGenerate creative realistic images based on the story content above. All images should be in {aspect_ratio} aspect ratio. No text should appear in the images. Maintain consistent realistic photographic style."
        success_rate = (total_paragraphs - failed_paragraphs) / total_paragraphs if total_paragraphs > 0 else 0
        print(f"📊 长文本翻译完成: 成功率 {success_rate:.1%} ({total_paragraphs - failed_paragraphs}/{total_paragraphs}段落)")
        if success_rate >= 0.5:  # 50%以上成功率认为可接受
            return final_translation
        else:
            print(f"⚠️ 翻译成功率过低({success_rate:.1%})，返回默认英文")
            return f"Generate creative images based on the story content with {aspect_ratio} aspect ratio"
    except Exception as e:
        print(f"分段翻译过程发生异常: {str(e)}")
        return f"Generate creative images based on the story content with {aspect_ratio} aspect ratio"
```

### `translate_with_azure`

```python
def translate_with_azure(text, attempt_num=1):
    """使用Azure翻译API进行翻译
    配置说明：
    1. 在Azure门户 (https://portal.azure.com) 中创建“翻译器”服务
    2. 获取订阅密钥 (subscription_key) 和资源位置 (location)
    3. 将下面的配置替换为您的实际值
    """
    try:
        import json
        # Azure翻译API配置
        # 🔧 请将以下配置替换为您的实际Azure翻译服务信息
        subscription_key = ""  # 替换为您的订阅密钥
        endpoint = "https://api.cognitive.microsofttranslator.com/"  # 固定端点
        location = "global"  # 替换为您的资源位置，例如 "eastus", "westus2", "global" 等
        # 检查配置是否已更新
        if subscription_key == "YOUR_SUBSCRIPTION_KEY" or location == "YOUR_RESOURCE_LOCATION":
            print("❌ 请先配置Azure翻译API的订阅密钥和资源位置")
            print("📖 配置方法：")
            print("   1. 在Azure门户创建翻译器服务")
            print("   2. 获取订阅密钥和资源位置")
            print("   3. 在代码中替换 subscription_key 和 location 的值")
            return None
        # 构建请求URL
        path = '/translate'
        constructed_url = endpoint + path
        # 请求参数
        params = {
            'api-version': '3.0',
            'from': 'zh',
            'to': 'en'
        }
        # 请求头
        headers = {
            'Ocp-Apim-Subscription-Key': subscription_key,
            'Ocp-Apim-Subscription-Region': location,
            'Content-Type': 'application/json',
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
        }
        # 请求体
        body = [{'text': text}]
        # 添加延迟避免API限制
        delay_time = 1 + attempt_num * 0.5
        print(f"⏳ Azure翻译延迟 {delay_time} 秒...")
        time.sleep(delay_time)
        # 发送请求
        response = requests.post(constructed_url, params=params, headers=headers, json=body, timeout=30)
        if response.status_code == 200:
            result = response.json()
            if result and len(result) > 0 and result[0].get('translations'):
                translated_text = result[0]['translations'][0]['text']
                return translated_text
            else:
                print(f"Azure翻译第{attempt_num}次尝试返回无效结果")
                return None
        else:
            print(f"Azure翻译第{attempt_num}次尝试失败: {response.status_code} - {response.text}")
            return None
    except Exception as e:
        print(f"Azure翻译第{attempt_num}次尝试异常: {str(e)}")
        return None
```

## `MessiPictureGenerator` 类

```python
class MessiPictureGenerator:
    def __init__(self):
        self.browser = None
        self.page = None
        self.context = None
        self.failed_stories = []  # 记录生成失败的故事
        self.stories = []  # 存储要处理的故事列表
        self.successful_conversation_positions = []  # 记录成功生成的对话栏位置
        # 新增：记录所有生成尝试的顺序和结果
        self.generation_attempts = []  # 记录每次生成尝试的详细信息
        # 新增：用户选择的图片数量
        self.images_per_group = 2  # 默认每组2张图
        # 新增：用户选择的图片比例
        self.aspect_ratio = "9:16"  # 默认9:16比例
        # 新增：运行模式
        self.mode = "ronaldoshorts"  # 默认模式
        # 新增：是否启用20秒无增长判定失败功能
        self.enable_no_growth_check = True  # 默认启用
        # 新增：画风选择模式
        self.style_mode = "realistic"  # 默认写实风格，可选 "realistic" 或 "cartoon"
        # 添加统计信息
        self.stats = {
            'total_stories': 0,
            'successful_stories': 0,
            'failed_stories': 0,
            'translated_stories': 0,
            'retried_stories': 0,
            'start_time': None,
            'end_time': None,
            'story_times': []
        }
    def ask_user_choices(self):
        """询问用户选择模式和配置"""
        print(f"\n{'='*60}")
        print("🎯 豆包图片生成器")
        print(f"{'='*60}")
        # 首先选择模式
        print("请选择生成模式：")
        print("1. ronaldoshorts - 从messipicture.txt读取故事内容")
        print("2. ronaldolong - 从ronaldomessi.txt读取AI图片转视频提示词")
        print(f"{'='*60}")
        while True:
            try:
                choice = input("请输入选择 (1 或 2): ").strip()
                if choice == '1':
                    self.mode = "ronaldoshorts"
                    print("✅ 已选择：ronaldoshorts 模式")
                    break
                elif choice == '2':
                    self.mode = "ronaldolong"
                    print("✅ 已选择：ronaldolong 模式")
                    # ronaldolong模式固定配置
                    self.aspect_ratio = "16:9"
                    self.images_per_group = 2
                    print("🎯 ronaldolong模式配置：")
                    print("   图片比例: 16:9 (固定)")
                    print("   每段生成: 2张图片 (固定)")
                    print("   分组方式: 每20段为一组")  # 从35改为20
                    print(f"{'='*60}")
                    # ronaldolong模式跳过后续选择，直接进入无增长检查选择
                    break
                else:
                    print("❌ 请输入 1 或 2")
            except KeyboardInterrupt:
                print("\n用户取消操作")
                return False
            except Exception as e:
                print(f"输入错误: {str(e)}")
        # ronaldoshorts模式继续原有的选择流程
        if self.mode == "ronaldoshorts":
            # 选择图片比例
            print(f"\n{'='*60}")
            print("请选择图片比例：")
            print("1. 9:16 (竖屏，适合手机)")
            print("2. 16:9 (横屏，适合电脑/电视)")
            print("3. 2:3 (竖屏，适合社交媒体)")
            print(f"{'='*60}")
            while True:
                try:
                    choice = input("请输入选择 (1、2 或 3): ").strip()
                    if choice == '1':
                        self.aspect_ratio = "9:16"
                        print("✅ 已选择：9:16 (竖屏)")
                        break
                    elif choice == '2':
                        self.aspect_ratio = "16:9"
                        print("✅ 已选择：16:9 (横屏)")
                        break
                    elif choice == '3':
                        self.aspect_ratio = "2:3"
                        print("✅ 已选择：2:3 (竖屏)")
                        break
                    else:
                        print("❌ 请输入 1、2 或 3")
                except KeyboardInterrupt:
                    print("\n用户取消操作")
                    return False
                except Exception as e:
                    print(f"输入错误: {str(e)}")
            # 选择每组图片数量
            print(f"\n{'='*60}")
            print("请选择每组生成的图片数量：")
            print("1. 每组生成1张图片")
            print("2. 每组生成2张图片（默认）")
            print(f"{'='*60}")
            while True:
                try:
                    choice = input("请输入选择 (1 或 2): ").strip()
                    if choice == '1':
                        self.images_per_group = 1
                        print("✅ 已选择：每组生成1张图片")
                        break
                    elif choice == '2':
                        self.images_per_group = 2
                        print("✅ 已选择：每组生成2张图片")
                        break
                    else:
                        print("❌ 请输入 1 或 2")
                except KeyboardInterrupt:
                    print("\n用户取消操作")
                    return False
                except Exception as e:
                    print(f"输入错误: {str(e)}")
        # 画风选择（两种模式都需要）
        print(f"\n{'='*60}")
        print("请选择图片画风：")
        print("1. 不要卡通风格（写实风格）")
        print("2. 都要卡通风格")
        print(f"{'='*60}")
        while True:
            try:
                choice = input("请输入选择 (1 或 2): ").strip()
                if choice == '1':
                    self.style_mode = "realistic"
                    print("✅ 已选择：写实风格 - 不要卡通风格")
                    break
                elif choice == '2':
                    self.style_mode = "cartoon"
                    print("✅ 已选择：卡通风格 - 都要卡通风格")
                    break
                else:
                    print("❌ 请输入 1 或 2")
            except KeyboardInterrupt:
                print("\n用户取消操作")
                return False
            except Exception as e:
                print(f"输入错误: {str(e)}")
        # 无增长检查选择（两种模式都需要）
        print(f"\n{'='*60}")
        print("请选择图片生成监控模式：")
        print("1. 标准模式 - 启用20秒无增长判定失败（推荐）")
        print("2. 宽松模式 - 关闭20秒无增长判定，仅等待目标数量完成")
        print("")
        print("说明：")
        print("• 标准模式：如果20秒内图片数量不增长，判定为生成失败并重试")
        print("• 宽松模式：只等待目标图片数量完成，不进行无增长检查")
        print("• 标准模式能更快发现生成问题，但可能对网络要求较高")
        print("• 宽松模式更适合网络较慢的情况")
        print(f"{'='*60}")
        while True:
            try:
                choice = input("请输入选择 (1 或 2): ").strip()
                if choice == '1':
                    self.enable_no_growth_check = True
                    print("✅ 已选择：标准模式 - 启用20秒无增长判定失败")
                    break
                elif choice == '2':
                    self.enable_no_growth_check = False
                    print("✅ 已选择：宽松模式 - 关闭20秒无增长判定")
                    break
                else:
                    print("❌ 请输入 1 或 2")
            except KeyboardInterrupt:
                print("\n用户取消操作")
                return False
            except Exception as e:
                print(f"输入错误: {str(e)}")
        # 显示最终配置
        print(f"\n{'='*60}")
        print("🎯 最终配置：")
        print(f"   模式: {self.mode}")
        print(f"   图片比例: {self.aspect_ratio}")
        if self.mode == "ronaldoshorts":
            print(f"   每组图片数量: {self.images_per_group}张")
        else:
            print(f"   每段生成: {self.images_per_group}张图片")
        print(f"   画风选择: {'写实风格 (不要卡通风格)' if self.style_mode == 'realistic' else '卡通风格 (都要卡通风格)'}")
        print(f"   监控模式: {'标准模式 (20秒无增长判定失败)' if self.enable_no_growth_check else '宽松模式 (仅等待目标数量)'}")
        print(f"{'='*60}")
        return True
    def ask_user_choice(self):
        """保持向后兼容的方法名，调用新的ask_user_choices方法"""
        return self.ask_user_choices()
    async def initialize_browser(self):
        """初始化浏览器"""
        try:
            self.playwright = await async_playwright().start()
            # 创建用户数据目录以保留登录信息
            user_data_dir = os.path.expanduser("~/Desktop/AutoAI/browser_data")
            os.makedirs(user_data_dir, exist_ok=True)
            self.browser = await self.playwright.chromium.launch_persistent_context(
                user_data_dir=user_data_dir,
                headless=False,
                args=[
                    '--no-sandbox',
                    '--disable-setuid-sandbox',
                    '--disable-dev-shm-usage',
                    '--disable-accelerated-2d-canvas',
                    '--no-first-run',
                    '--no-zygote',
                    '--disable-gpu'
                ]
            )
            # 使用持久化上下文时，browser 就是 context
            self.context = self.browser
            self.page = await self.context.new_page()
            logger.info(f"浏览器初始化成功，用户数据保存在: {user_data_dir}")
            return True
        except Exception as e:
            logger.error(f"浏览器初始化失败: {str(e)}")
            return False
    async def close_browser(self):
        """关闭浏览器"""
        try:
            if self.page:
                await self.page.close()
            if self.browser:
                await self.browser.close()
            if hasattr(self, 'playwright'):
                await self.playwright.stop()
            logger.info("浏览器已关闭")
        except Exception as e:
            logger.error(f"关闭浏览器时发生错误: {str(e)}")
    async def check_generation_error(self):
        """检测页面是否出现生成错误"""
        try:
            # 检查页面可见文本内容，而不是HTML源码
            page_text = await self.page.text_content('body')
            # 调试：显示页面文本的一部分（包含python关键字的部分）
            if 'python' in page_text.lower():
                python_index = page_text.lower().find('python')
                context = page_text[max(0, python_index-50):python_index+200]
                print(f"🔍 发现python关键字，上下文: {context[:100]}...")
            # 更宽松的错误检测模式，基于截图中的实际格式
            error_patterns = [
                # 基本的错误标识
                '"type": "generate"',
                '"numbers": 2',
                # 组合检测：同时包含type和numbers
                r'"type":\s*"generate".*"numbers":\s*2',
                # Python代码块格式
                'python\n{',
                # 更具体的组合
                'python.*"type".*"generate"',
                'python.*"numbers".2',
                # 检测完整的错误JSON结构片段
                r'{\s"type":\s*"generate"',
                r'"type":\s*"generate"."prompt":',
                r'"numbers":\s2.*"prompt":',
            ]
            # 使用正则表达式进行更灵活的匹配
            import re
            for pattern in error_patterns:
                try:
                    match = re.search(pattern, page_text, re.DOTALL | re.IGNORECASE)
                    if match:
                        print(f"🚨 检测到生成错误标识: {pattern}")
                        print(f"🔍 匹配到的内容: {match.group()[:200]}...")
                        # 显示匹配位置的上下文
                        start = max(0, match.start() - 100)
                        end = min(len(page_text), match.end() + 100)
                        context = page_text[start:end]
                        print(f"🔍 上下文: ...{context}...")
                        return True
                except Exception as e:
                    # 如果正则表达式失败，尝试简单字符串匹配
                    if pattern in page_text:
                        print(f"🚨 检测到生成错误标识: {pattern}")
                        print("🔍 简单匹配，无法显示具体位置")
                        return True
            # 额外检查：如果页面包含"python"和JSON结构，很可能是错误
            if 'python' in page_text.lower() and ('"type"' in page_text or '"numbers"' in page_text):
                print("🚨 检测到可疑的python代码块和JSON结构")
                return True
            # 专门针对截图格式的检测
            if 'python' in page_text and '{' in page_text and '"type"' in page_text and '"generate"' in page_text:
                print("🚨 检测到截图中显示的错误格式")
                return True
            return False
        except Exception as e:
            print(f"检测生成错误时出错: {str(e)}")
            return False
    async def check_topic_change_toast(self):
        """检测是否出现“我们换个话题聊聊吧”的浮窗提示"""
        try:
            # 检查特定的浮窗元素
            toast_selectors = [
                'span.semi-toast-content-text',
                'span[x-semi-prop="content"]',
                '.semi-toast-content-text',
                'div.semi-toast-content span',
                'div[class*="toast"] span',
                'div[class*="notification"] span'
            ]
            for selector in toast_selectors:
                try:
                    toast_elements = await self.page.query_selector_all(selector)
                    for element in toast_elements:
                        if element:
                            text_content = await element.text_content()
                            if text_content and "我们换个话题聊聊吧" in text_content:
                                print(f"🚨 检测到话题切换浮窗: {text_content}")
                                return True
                except Exception as e:
                    continue
            # 备用检测：检查页面整体文本内容
            try:
                page_text = await self.page.text_content('body')
                if "我们换个话题聊聊吧" in page_text:
                    print("🚨 在页面文本中检测到话题切换提示")
                    return True
            except Exception as e:
                pass
            # 进一步检测：使用JavaScript查找所有可能的浮窗元素
            try:
                found_toast = await self.page.evaluate('''
                    () => {
                        // 查找所有可能包含浮窗文本的元素
                        const allElements = document.querySelectorAll('*');
                        for (const element of allElements) {
                            const text = element.textContent || element.innerText;
                            if (text && text.includes('我们换个话题聊聊吧')) {
                                console.log('找到话题切换浮窗:', text);
                                return true;
                            }
                        }
                        return false;
                    }
                ''')
                if found_toast:
                    print("🚨 JavaScript检测到话题切换浮窗")
                    return True
            except Exception as e:
                pass
            return False
        except Exception as e:
            print(f"检测话题切换浮窗时出错: {str(e)}")
            return False
    async def check_captcha_popup(self):
        """检测是否出现人机验证弹窗"""
        try:
            selectors = [
                'div.captcha-bar.captcha_verify_bar.captcha-prompt-bar',
                'div.captcha_verify_bar',
                'div.tit.captcha_verify_bar--title'
            ]
            for sel in selectors:
                try:
                    el = await self.page.query_selector(sel)
                    if el and await el.is_visible():
                        print(f"🚧 检测到人机验证弹窗: {sel}")
                        return True
                except Exception:
                    continue
            # 备用：从页面文本中粗略检测
            try:
                page_text = await self.page.text_content('body')
                if page_text and ('人机验证' in page_text or '验证码' in page_text or 'captcha' in page_text.lower()):
                    print("🚧 检测到疑似人机验证提示")
                    return True
            except Exception:
                pass
            return False
        except Exception as e:
            print(f"检测人机验证弹窗时出错: {str(e)}")
            return False
    async def wait_until_captcha_cleared(self, check_interval_seconds=300):
        """检测到人机验证后暂停，按固定间隔重复检测，直到弹窗消失。返回暂停总秒数。"""
        paused = 0
        try:
            print(f"⏸️ 检测到人机验证弹窗，暂停任务。每{int(check_interval_seconds/60)}分钟复检一次...")
            while True:
                await asyncio.sleep(check_interval_seconds)
                paused += check_interval_seconds
                still_present = await self.check_captcha_popup()
                if still_present:
                    print(f"⏳ 人机验证仍存在，已等待 {paused//60} 分钟，继续等待...")
                    continue
                print("✅ 人机验证弹窗已消失，继续当前任务")
                return paused
        except Exception as e:
            print(f"等待人机验证消失时出错: {str(e)}，继续任务")
            return paused
    def find_messipicture_file(self):
        """在桌面查找messipicture.txt文件"""
        desktop_path = os.path.expanduser("~/Desktop")
        file_path = os.path.join(desktop_path, "messipicture.txt")
        if os.path.exists(file_path):
            logger.info(f"找到文件: {file_path}")
            return file_path
        else:
            logger.error("桌面上没有找到 messipicture.txt 文件")
            return None
    def find_ronaldomessi_file(self):
        """在桌面查找ronaldomessi.txt文件"""
        desktop_path = os.path.expanduser("~/Desktop")
        file_path = os.path.join(desktop_path, "ronaldomessi.txt")
        if os.path.exists(file_path):
            logger.info(f"找到文件: {file_path}")
            return file_path
        else:
            logger.error("桌面上没有找到 ronaldomessi.txt 文件")
            return None
    def extract_ai_video_prompts(self, file_path):
        """从ronaldomessi.txt文件中提取AI图片转视频提示词"""
        try:
            with open(file_path, 'r', encoding='utf-8') as f:
                content = f.read()
            logger.info(f"成功读取文件，内容长度: {len(content)} 字符")
            # 查找“AI图片转视频提示词：”和“简单动作指导：”之间的内容
            start_marker = "AI图片转视频提示词："
            end_marker = "简单动作指导："
            start_index = content.find(start_marker)
            if start_index == -1:
                logger.error("没有找到'AI图片转视频提示词：'标记")
                return []
            end_index = content.find(end_marker)
            if end_index == -1:
                logger.error("没有找到'简单动作指导：'标记")
                return []
            if start_index >= end_index:
                logger.error("标记位置错误：'AI图片转视频提示词：'应该在'简单动作指导：'之前")
                return []
            # 提取中间内容
            start_pos = start_index + len(start_marker)
            extracted_content = content[start_pos:end_index].strip()
            # 按行分割并过滤空行
            lines = [line.strip() for line in extracted_content.split('\n') if line.strip()]
            logger.info(f"成功提取 {len(lines)} 段AI图片转视频提示词")
            # 显示前几段作为预览
            for i, line in enumerate(lines[:5]):
                logger.info(f"提示词 {i+1}: {line[:50]}...")
            if len(lines) > 5:
                logger.info(f"... 还有 {len(lines) - 5} 段提示词")
            return lines
        except Exception as e:
            logger.error(f"提取AI图片转视频提示词失败: {str(e)}")
            return []
    def group_prompts_for_ronaldolong(self, prompts):
        """将提示词按20段为一组进行分组"""
        try:
            groups = []
            group_size = 20  # 从35改为20
            for i in range(0, len(prompts), group_size):
                group = prompts[i:i+group_size]
                groups.append(group)
            logger.info(f"将 {len(prompts)} 段提示词分为 {len(groups)} 组")
            for i, group in enumerate(groups):
                logger.info(f"第 {i+1} 组: {len(group)} 段提示词")
            return groups
        except Exception as e:
            logger.error(f"分组提示词失败: {str(e)}")
            return []
    def create_submission_content_for_group(self, group, group_index):
        """为一组提示词创建提交给豆包的内容"""
        try:
            # 将组内的提示词合并，每行一个
            group_content = '\n'.join(group)
            # 添加生成指令，根据画风选择
            if self.style_mode == "cartoon":
                instruction = f"\n\n给上面每段提示词生成2张图，生成对应的2张图 然后继续下一段的出图2张 其他的不用解释 我没写足球你在画面中就不要出现足球 所有的图片都要比例16：9的 所有图片中不要出现中文！！所有图片都要卡通风格！注意！！保持卡通的一致画风！！！出现的角色都要是美国白人的脸！"
            else:  # realistic
                instruction = f"\n\n给上面每段提示词生成2张图，生成对应的2张图 然后继续下一段的出图2张 其他的不用解释 我没写足球你在画面中就不要出现足球 所有的图片都要比例16：9的 所有图片中不要出现中文！！所有图片不要卡通风格！注意！！保持写实的一致画风！！！出现的角色都要是美国白人的脸！"
```
