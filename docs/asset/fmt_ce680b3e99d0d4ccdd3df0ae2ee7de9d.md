
# 豆包图片自动下载器（Python）

本脚本基于 Playwright 实现豆包网页版图片的自动化下载，包含浏览器初始化、对话栏导航、图片容器检测、hover 下载、页面刷新重试、防睡眠以及下载统计等功能。

## 代码结构概览

- 导入依赖并配置日志
- 定义页面加载、下载重试、页面刷新等时间常量
- 实现 `PreventSleep` 类，支持 macOS、Windows、Linux 跨平台防睡眠
- 注册 `SIGINT` / `SIGTERM` 信号处理函数
- 实现 `DoubaoImageDownloader` 类，完成浏览器控制、对话查找、图片滚动与下载
- 提供 `main()` 入口及依赖自动安装逻辑

```python
import os
import asyncio
import logging
import time
import re
import signal
import sys
import requests
import threading
import subprocess
import platform
from pathlib import Path
from playwright.async_api import async_playwright

# 配置日志
logging.basicConfig(level=logging.WARNING, format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')
logger = logging.getLogger(__name__)

# ==================== 时间配置 ====================
# 页面导航和加载时间
PAGE_LOAD_WAIT_TIME = 3  # 页面加载后等待时间（秒）
PAGE_NAVIGATION_TIMEOUT = 30000  # 页面导航超时时间（毫秒）
SIDEBAR_WAIT_TIME = 3  # 侧边栏显示等待时间（秒）

# 图片查找和滚动时间
SCROLL_CONTENT_WAIT_TIME = 5  # 滚动对话内容后等待图片加载时间（秒）
IMAGE_CONTAINER_PROCESS_DELAY = 1  # 处理每个图片容器间的延迟（秒）
IMAGE_CONTAINER_LOCATE_DELAY = 20  # 快速定位图片容器时的等待时间（秒）
IMAGE_CONTAINER_WAIT_MAX_TIME = 120  # 等待图片容器加载的最大时间（秒）
IMAGE_CONTAINER_CHECK_INTERVAL = 10  # 检查图片容器是否加载的间隔时间（秒）
CONVERSATION_SWITCH_DELAY = 2  # 切换对话后的等待时间（秒）

# 下载重试配置
MAX_DOWNLOAD_RETRIES = 3  # 每张图片最大重试次数
DOWNLOAD_RETRY_DELAY = 1  # 每次下载重试间的延迟（秒）
HOVER_DOWNLOAD_MAX_RETRIES = 2  # hover下载的内部重试次数

# 页面刷新和恢复时间
PAGE_REFRESH_TIMEOUT = 30000  # 页面刷新超时时间（毫秒）
PAGE_REFRESH_WAIT_TIME = 3  # 页面刷新后等待时间（秒）
CONTENT_RESCROLL_WAIT_TIME = 2  # 重新滚动内容后等待时间（秒）
CONVERSATION_REENTER_WAIT_TIME = 3  # 重新进入对话后等待时间（秒）

# 防睡眠配置
PREVENT_SLEEP_INTERVAL = 30  # 防睡眠操作间隔（秒）
# ==================== 配置结束 ====================

# 图片元素的类名配置
IMAGE_CLASS_NAME = 'image-hDQAs3'

# 全局变量保存浏览器实例
global_browser = None

class PreventSleep:
    """防睡眠类，支持跨平台防睡眠功能"""
    
    def __init__(self):
        self.is_running = False
        self.thread = None
        self.system = platform.system().lower()
        
    def start(self):
        """开始防睡眠"""
        if self.is_running:
            return
            
        self.is_running = True
        self.thread = threading.Thread(target=self._prevent_sleep_loop, daemon=True)
        self.thread.start()
        print("✅ 防睡眠功能已启动")
        
    def stop(self):
        """停止防睡眠"""
        self.is_running = False
        if self.thread and self.thread.is_alive():
            self.thread.join(timeout=1)
        print("⏹️ 防睡眠功能已停止")
        
    def _prevent_sleep_loop(self):
        """防睡眠循环"""
        while self.is_running:
            try:
                self._prevent_sleep_once()
                time.sleep(PREVENT_SLEEP_INTERVAL)  # 使用配置常量
            except Exception as e:
                logger.warning(f"防睡眠操作失败: {e}")
                time.sleep(PREVENT_SLEEP_INTERVAL)  # 使用配置常量
                
    def _prevent_sleep_once(self):
        """执行一次防睡眠操作"""
        try:
            if self.system == 'darwin':  # macOS
                # 使用caffeinate命令防止睡眠
                subprocess.run(['caffeinate', '-u', '-t', '1'], 
                             capture_output=True, timeout=5)
            elif self.system == 'windows':  # Windows
                # 使用Windows API防止睡眠
                try:
                    import ctypes
                    # ES_CONTINUOUS | ES_SYSTEM_REQUIRED | ES_DISPLAY_REQUIRED
                    ctypes.windll.kernel32.SetThreadExecutionState(0x80000000 | 0x00000001 | 0x00000002)
                except Exception:
                    # 备用方案：使用powercfg命令
                    subprocess.run(['powercfg', '/requests'], 
                                 capture_output=True, timeout=5)
            elif self.system == 'linux':  # Linux
                # 使用多种方法防止睡眠
                try:
                    # 方法1：使用systemd-inhibit
                    subprocess.run(['systemd-inhibit', '--what=sleep', '--who=doubao_downloader', 
                                  '--why=Downloading images', 'sleep', '1'], 
                                 capture_output=True, timeout=5)
                except Exception:
                    try:
                        # 方法2：使用xset (如果有X11)
                        subprocess.run(['xset', 's', 'reset'], 
                                     capture_output=True, timeout=5)
                    except Exception:
                        # 方法3：写入/sys/power/wake_lock (需要root权限)
                        try:
                            with open('/sys/power/wake_lock', 'w') as f:
                                f.write('doubao_downloader')
                        except Exception:
                            pass
        except Exception as e:
            logger.debug(f"防睡眠操作异常: {e}")

def signal_handler(signum, frame):
    """信号处理函数，防止浏览器被关闭"""
    print(f"\n接收到信号 {signum}，脚本将退出但浏览器保持打开")
    print("浏览器将继续运行，您可以手动关闭它")
    sys.exit(0)

# 注册信号处理器
signal.signal(signal.SIGINT, signal_handler)
signal.signal(signal.SIGTERM, signal_handler)

class DoubaoImageDownloader:
    def __init__(self):
        self.browser = None
        self.page = None
        self.context = None
        self.download_dir = None
        self.start_time = None
        self.failed_images = []  # 记录失败的图片
        self.no_images_conversations = []  # 记录没有找到图片的对话栏
        self.prevent_sleep = PreventSleep()  # 防睡眠实例
        
    async def initialize_browser(self):
        """初始化浏览器"""
        global global_browser
        try:
            # 启动防睡眠功能
            self.prevent_sleep.start()
            
            self.playwright = await async_playwright().start()
            
            # 创建用户数据目录以保留登录信息
            user_data_dir = os.path.expanduser("~/Desktop/AutoAI/browser_data")
            os.makedirs(user_data_dir, exist_ok=True)
            
            # 创建下载目录并设置权限
            self.download_dir = os.path.expanduser("~/Downloads")
            os.makedirs(self.download_dir, exist_ok=True)
            
            # 确保下载目录有写权限
            import stat
            os.chmod(self.download_dir, stat.S_IRWXU | stat.S_IRWXG | stat.S_IRWXO)
            
            # 启动浏览器
            self.browser = await self.playwright.chromium.launch_persistent_context(
                user_data_dir=user_data_dir,
                headless=False,
                downloads_path=self.download_dir,  # 设置下载路径
                accept_downloads=True,  # 允许下载
                args=[
                    '--no-sandbox',
                    '--disable-setuid-sandbox',
                    '--disable-dev-shm-usage',
                    '--disable-accelerated-2d-canvas',
                    '--no-zygote',
                    '--disable-gpu',
                    '--enable-features=VaapiVideoDecoder',
                    '--disable-features=VizDisplayCompositor'
                ]
            )
            
            # 保存到全局变量
            global_browser = self.browser
            
            # 使用持久化上下文时，browser 就是 context
            self.context = self.browser
            self.page = await self.context.new_page()
            
            # 设置页面的下载行为
            await self.page.route('**/*', lambda route: route.continue_())
            
            return True
        except Exception as e:
            logger.error(f"浏览器初始化失败: {str(e)}")
            # 如果初始化失败，停止防睡眠
            self.prevent_sleep.stop()
            return False
    
    async def close_browser(self):
        """关闭浏览器"""
        try:
            # 停止防睡眠功能
            self.prevent_sleep.stop()
            
            if self.page:
                await self.page.close()
            if self.browser:
                await self.browser.close()
            if hasattr(self, 'playwright'):
                await self.playwright.stop()
        except Exception as e:
            logger.error(f"关闭浏览器时发生错误: {str(e)}")
    
    async def navigate_to_doubao(self, max_retries=3):
        """导航到豆包生图页面"""
        for attempt in range(max_retries):
            try:
                url = "https://www.doubao.com/chat/create-image"
                
                await self.page.goto(url, wait_until='networkidle', timeout=PAGE_NAVIGATION_TIMEOUT)  # 使用配置常量
                
                # 等待页面加载完成
                await asyncio.sleep(PAGE_LOAD_WAIT_TIME)  # 使用配置常量
                return True
            except Exception as e:
                print(f"导航失败 (尝试 {attempt + 1}/{max_retries}): {e}")
                if attempt < max_retries - 1:
                    await asyncio.sleep(5)
                else:
                    return False
    
    async def find_numbered_conversations(self):
        """查找编号1及以后的纯数字对话栏，每个数字只保留最靠上的一个"""
        max_retries = 5
        for attempt in range(max_retries):
            try:
                # 等待历史对话列表项出现
                await self.page.wait_for_selector('span[data-testid="chat_list_item_title"]', state='attached', timeout=10000)
                await asyncio.sleep(1) # Give it a moment to fully render after being attached
                
                # 直接使用最准确的选择器
                conversations = await self.page.query_selector_all('span[data-testid="chat_list_item_title"]')
                
                # 如果没有找到对话栏，重试
                if not conversations:
                    logger.warning(f"尝试 {attempt + 1}/{max_retries}: 未找到对话栏元素，重试...")
                    await asyncio.sleep(2)
                    continue

                numbered_conversations = []
                seen_numbers = set()  # 用于记录已经找到的数字
                
                for conv in conversations:
                    try:
                        title = await conv.text_content()
                        title = title.strip()
                        
                        # 检查是否是纯数字且大于等于1
                        if title.isdigit() and int(title) >= 1:
                            number = int(title)
                            
                            # 如果这个数字已经找到过了，跳过（保留最靠上的）
                            if number in seen_numbers:
                                continue
                            
                            # 确保元素可见且可点击
                            is_visible = await conv.is_visible()
                            if is_visible:
                                numbered_conversations.append({
                                    'number': number,
                                    'title': title,
                                    'element': conv
                                })
                                seen_numbers.add(number)
                                print(f"找到对话栏 {title}（数字 {number}）") # 重新启用此行，以便调试
                            
                    except Exception as e:
                        continue
                
                if numbered_conversations: # 只有找到有效编号对话栏才返回
                    # 按编号排序
                    numbered_conversations.sort(key=lambda x: x['number'])
                    print(f"DEBUG: find_numbered_conversations returning sorted list: {[c['title'] for c in numbered_conversations]}") # Added debug
                    print(f"成功找到 {len(numbered_conversations)} 个编号对话栏") # 在成功找到后打印
                    return numbered_conversations
                else:
                    logger.warning(f"尝试 {attempt + 1}/{max_retries}: 找到0个有效编号对话栏，重试...")
                    await asyncio.sleep(2)
                    continue
            
            except Exception as e:
                logger.error(f"查找编号对话失败 (尝试 {attempt + 1}/{max_retries}): {str(e)}")
                await asyncio.sleep(2)
                continue
        logger.error("在多次尝试后未能找到任何编号对话。")
        return []
    
    async def click_conversation(self, conversation, max_retries=3):
        """点击对话栏"""
        for attempt in range(max_retries):
            try:
                # 确保元素可见
                await conversation['element'].scroll_into_view_if_needed()
                await asyncio.sleep(1)
                
                # 尝试多种点击方式
                click_success = False
                
                # 方法1：直接点击
                try:
                    await conversation['element'].click()
                    click_success = True
                except Exception as e1:
                    # 方法2：强制点击
                    try:
                        await conversation['element'].click(force=True)
                        click_success = True
                    except Exception as e2:
                        # 方法3：JavaScript点击
                        try:
                            await self.page.evaluate('(element) => element.click()', conversation['element'])
                            click_success = True
                        except Exception as e3:
                            # 方法4：尝试点击父元素
                            try:
                                parent = await conversation['element'].evaluate_handle('el => el.parentElement')
                                if parent:
                                    await parent.click()
                                    click_success = True
                            except Exception as e4:
                                # 方法5：尝试点击包含该元素的可点击区域
                                try:
                                    # 查找包含该标题的可点击容器
                                    clickable_parent = await self.page.evaluate(f'''
                                        (title) => {{
                                            const titleElements = Array.from(document.querySelectorAll('span[data-testid="chat_list_item_title"]'));
                                            const targetElement = titleElements.find(el => el.textContent.trim() === title);
                                            if (targetElement) {{
                                                let parent = targetElement.parentElement;
                                                while (parent && parent !== document.body) {{
                                                    const style = window.getComputedStyle(parent);
                                                    if (style.cursor === 'pointer' || parent.onclick || parent.getAttribute('role') === 'button') {{
                                                        return parent;
                                                    }}
                                                    parent = parent.parentElement;
                                                }}
                                            }}
                                            return null;
                                        }}
                                    ''', conversation['title'])
                                    
                                    if clickable_parent:
                                        await clickable_parent.click()
                                        click_success = True
                                except Exception as e5:
                                    pass
                
                if not click_success:
                    if attempt < max_retries - 1:
                        await asyncio.sleep(2)
                        continue
                    else:
                        return False
                
                # 等待页面加载
                await asyncio.sleep(5)
                
                # 验证是否成功进入对话
                try:
                    # 检查URL是否改变或页面内容是否更新
                    current_url = self.page.url
                    if conversation['title'] in current_url or 'chat/' in current_url:
                        return True
                    else:
                        # 检查页面是否有图片内容
                        images = await self.page.query_selector_all(f'img.{IMAGE_CLASS_NAME}')
                        if len(images) > 0:
                            return True
                        else:
                            if attempt < max_retries - 1:
                                await asyncio.sleep(2)
                                continue
                            else:
                                return False
                except:
                    return True  # 如果验证失败，假设成功
                    
            except Exception as e:
                if attempt < max_retries - 1:
                    await asyncio.sleep(2)
                    continue
        
        return False
    
    async def download_image(self, img_url, filename):
        """下载单张图片"""
        try:
            # 使用requests下载图片
            headers = {
                'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36'
            }
            
            response = requests.get(img_url, headers=headers, timeout=30)
            response.raise_for_status()
            
            # 保存图片
            file_path = os.path.join(self.download_dir, filename)
            with open(file_path, 'wb') as f:
                f.write(response.content)
            
            return True
        except Exception as e:
            return False
    
    async def scroll_page_to_find_all_images(self):
        """滚动页面以找到所有图片"""
        try:
            # 先滚动到页面顶部
            await self.page.evaluate('window.scrollTo(0, 0)')
            await asyncio.sleep(1)
            
            # 逐步向下滚动
            for i in range(5):
                await self.page.evaluate(f'window.scrollBy(0, {500 * (i + 1)})')
                await asyncio.sleep(1)
            
            # 滚动到页面底部
            await self.page.evaluate('window.scrollTo(0, document.body.scrollHeight)')
            await asyncio.sleep(2)
            
            # 再滚动回顶部确保所有图片都加载
            await self.page.evaluate('window.scrollTo(0, 0)')
            await asyncio.sleep(2)
            
        except Exception as e:
            pass

    async def get_original_image_url(self, img_element):
        """获取原图URL"""
        try:
            # 尝试获取原图链接
            img_src = await img_element.get_attribute('src')
            
            if not img_src:
                return None
            
            # 如果是data URL，跳过
            if img_src.startswith('data:'):
                return None
            
            # 尝试获取原图URL（去掉缩略图参数）
            original_url = img_src
            
            # 豆包图片URL通常包含缩略图参数，尝试去掉这些参数获取原图
            if '~tplv-' in original_url:
                # 去掉缩略图参数
                original_url = original_url.split('~tplv-')[0]
            
            # 也尝试从srcset获取更高分辨率的图片
            srcset = await img_element.get_attribute('srcset')
            if srcset:
                # srcset通常包含多个分辨率，选择最高分辨率的
                srcset_urls = srcset.split(',')
                for url_desc in srcset_urls:
                    url_part = url_desc.strip().split(' ')[0]
                    if '2x' in url_desc or url_part.endswith('2x'):
                        if not url_part.startswith('data:'):
                            original_url = url_part
                            if '~tplv-' in original_url:
                                original_url = original_url.split('~tplv-')[0]
                            break
            
            return original_url
            
        except Exception as e:
            return None

    async def scroll_conversation_content(self):
        """滚动对话内容区域（绿色框区域）"""
        try:
            print("    开始滚动对话内容区域以加载图片...")
            
            # 首先等待页面稳定
            await asyncio.sleep(2)
            
            # 简化滚动策略：只做必要的滚动
            scrollable_container = None
            content_selectors = [
                'div[data-testid="message-list"]',  # 真正的消息列表滚动容器
            ]
            
            # 找到可滚动的对话内容容器
            for selector in content_selectors:
                try:
                    containers = await self.page.query_selector_all(selector)
                    for container in containers:
                        # 检查是否可滚动
                        is_scrollable = await container.evaluate('(el) => el.scrollHeight > el.clientHeight')
                        if is_scrollable:
                            scrollable_container = container
                            print(f"    找到可滚动容器: {selector}")
                            break
                    if scrollable_container:
                        break
                except Exception as e:
                    continue
            
            if scrollable_container:
                print("    执行简化滚动...")
                
                # 获取滚动信息
                scroll_info = await scrollable_container.evaluate('''
                    (el) => ({
                        scrollHeight: el.scrollHeight,
                        clientHeight: el.clientHeight,
                        scrollTop: el.scrollTop,
                        className: el.className
                    })
                ''')
                
                print(f"    容器信息: 总高度={scroll_info['scrollHeight']}, 可视高度={scroll_info['clientHeight']}")
                
                # 检查是否是反向滚动
                is_reverse = 'reverse-' in scroll_info['className']
                
                if is_reverse:
                    # 反向滚动：直接滚动到顶部
                    max_scroll_top = scroll_info['scrollHeight'] - scroll_info['clientHeight']
                    await scrollable_container.evaluate(f'(el) => el.scrollTop = {max_scroll_top}')
                    await asyncio.sleep(2)
                    print(f"    反向滚动完成，位置: {max_scroll_top}")
                else:
                    # 正常滚动：直接滚动到底部
                    max_scroll = scroll_info['scrollHeight'] - scroll_info['clientHeight']
                    await scrollable_container.evaluate(f'(el) => el.scrollTop = {max_scroll}')
                    await asyncio.sleep(2)
                    print(f"    正常滚动完成，位置: {max_scroll}")
            else:
                print("    未找到滚动容器，跳过滚动")
            
            # 快速定位图片容器
            print("    快速定位图片容器...")
            try:
                # 持续等待直到找到图片容器
                start_time = time.time()
                image_containers = []
                
                while True:
                    image_containers = await self.page.query_selector_all('div[data-testid="mdbox_image"]')
                    elapsed_time = time.time() - start_time
                    
                    if len(image_containers) > 0:
                        print(f"    找到 {len(image_containers)} 个图片容器 (用时 {elapsed_time:.1f} 秒)")
                        
                        # 只滚动到前几个和后几个容器，确保都可见
                        # 滚动到第一个容器
                        await image_containers[0].scroll_into_view_if_needed()
                        await asyncio.sleep(IMAGE_CONTAINER_LOCATE_DELAY)  # 使用配置常量
                        
                        # 滚动到最后一个容器
                        await image_containers[-1].scroll_into_view_if_needed()
                        await asyncio.sleep(IMAGE_CONTAINER_LOCATE_DELAY)  # 使用配置常量
                        
                        print("    图片容器定位完成")
                        break
                    else:
                        if elapsed_time >= 30:  # 最多等待30秒
                            print(f"    未找到图片容器，已等待 {elapsed_time:.1f} 秒，停止等待")
                            break
                        
                        print(f"    暂未找到图片容器，已等待 {elapsed_time:.1f} 秒...")
                        await asyncio.sleep(2)  # 每2秒检查一次
                
            except Exception as e:
                print(f"    快速定位图片容器失败: {e}")
            
            print("    滚动完成，开始图片处理")
            
        except Exception as e:
            print(f"    滚动对话内容时发生错误: {e}")
            print("    跳过滚动，直接进入图片处理")

    async def ensure_sidebar_visible(self, max_retries=3):
        """确保侧边栏可见，如果隐藏则点击按钮显示"""
        for attempt in range(max_retries):
            try:
                # 先检查是否能找到历史对话栏，并且检查是否真正可见
                conversations = await self.page.query_selector_all('span[data-testid="chat_list_item_title"]')
                
                sidebar_truly_visible = False
                if len(conversations) > 0:
                    # 检查第一个对话栏是否真正可见（不在屏幕外）
                    try:
                        first_conv = conversations[0]
                        bounding_box = await first_conv.bounding_box()
                        if bounding_box and bounding_box['x'] >= 0 and bounding_box['width'] > 0:
                            sidebar_truly_visible = True
                    except:
                        pass
                
                if sidebar_truly_visible:
                    return True
                
                # 如果找不到历史对话栏或者不可见，尝试点击侧边栏按钮
                sidebar_button = await self.page.query_selector('button[data-testid="siderbar_closed_status_btn"]')
                if sidebar_button:
                    await sidebar_button.click()
                    await asyncio.sleep(SIDEBAR_WAIT_TIME)  # 使用配置常量
                    
                    # 再次检查是否显示了历史对话栏且真正可见
                    conversations = await self.page.query_selector_all('span[data-testid="chat_list_item_title"]')
                    if len(conversations) > 0:
                        try:
                            first_conv = conversations[0]
                            bounding_box = await first_conv.bounding_box()
                            if bounding_box and bounding_box['x'] >= 0 and bounding_box['width'] > 0:
                                return True
                            else:
                                if attempt < max_retries - 1:
                                    await asyncio.sleep(2)
                                    continue
                        except:
                            if attempt < max_retries - 1:
                                await asyncio.sleep(2)
                                continue
                    else:
                        if attempt < max_retries - 1:
                            await asyncio.sleep(2)
                            continue
                else:
                    if attempt < max_retries - 1:
                        await asyncio.sleep(2)
                        continue
                    
            except Exception as e:
                if attempt < max_retries - 1:
                    await asyncio.sleep(2)
                    continue
        
        return False

    async def download_image_by_hover(self, img_element, filename, max_retries=5):
        """通过hover图片并点击下载按钮来下载原图，增加重试机制"""
        for attempt in range(max_retries):
            try:
                # 直接获取图片的边界框，不进行滚动
                img_box = await img_element.bounding_box()
                if not img_box:
                    continue
                
                # hover到图片上，强制执行不滚动
                await img_element.hover(force=True)
                await asyncio.sleep(2)  # 增加等待时间确保hover效果出现
                
                # 设置下载监听器，只接受图片文件
                download_info = {'started': False, 'completed': False, 'path': None, 'download_obj': None}
                
                def handle_download(download):
                    # 检查下载的文件类型，只处理图片文件
                    suggested_filename = download.suggested_filename
                    if suggested_filename and any(suggested_filename.lower().endswith(ext) for ext in ['.jpg', '.jpeg', '.png', '.webp', '.gif']):
                        download_info['started'] = True
                        download_info['download_obj'] = download
                        
                        async def save_download():
                            try:
                                # 保存到指定路径
                                save_path = os.path.join(self.download_dir, filename)
                                await download.save_as(save_path)
                                download_info['completed'] = True
                                download_info['path'] = save_path
                            except Exception as e:
                                pass
                        
                        # 在事件循环中运行保存操作
                        asyncio.create_task(save_download())
                    else:
                        # 取消非图片文件的下载
                        asyncio.create_task(download.cancel())
                
                # 移除之前的监听器（如果有的话）
                try:
                    self.page.remove_all_listeners('download')
                except:
                    pass
                
                # 添加新的下载监听器
                self.page.on('download', handle_download)
                
                # 查找下载按钮 - 只查找在图片区域内的hover下载按钮
                download_button = None
                
                # 首先查找hover时出现的下载按钮（最精确的方式）
                hover_download_selectors = [
                    'div[data-testid="edit_image_hover_tag_download_btn"]',
                    'button[data-testid="edit_image_hover_tag_download_btn"]',
                    '[data-testid="edit_image_hover_tag_download_btn"]',
                ]
                
                for selector in hover_download_selectors:
                    try:
                        buttons = await self.page.query_selector_all(selector)
                        for button in buttons:
                            is_visible = await button.is_visible()
                            if is_visible:
                                # 检查按钮是否在图片附近（严格限制范围）
                                button_box = await button.bounding_box()
                                if button_box:
                                    # 非常严格的位置检查：按钮必须在图片区域内
                                    x_in_range = (button_box['x'] >= img_box['x'] - 20 and 
                                                 button_box['x'] <= img_box['x'] + img_box['width'] + 20)
                                    y_in_range = (button_box['y'] >= img_box['y'] - 20 and 
                                                 button_box['y'] <= img_box['y'] + img_box['height'] + 20)
                                    
                                    # 额外检查：确保按钮不在页面底部区域
                                    not_at_bottom = button_box['y'] < img_box['y'] + img_box['height'] + 50
                                    
                                    if x_in_range and y_in_range and not_at_bottom:
                                        download_button = button
                                        break
                        if download_button:
                            break
                    except:
                        continue
                
                # 如果没找到hover按钮，查找纯SVG下载图标（排除带文字的按钮）
                if not download_button:
                    # 查找只包含SVG图标的下载按钮，排除带有"下载"文字的按钮
                    try:
                        # 查找所有包含下载SVG的元素
                        svg_buttons = await self.page.query_selector_all('svg[viewBox="0 0 24 24"]:has(path[d*="M20.207 11.707"])')
                        for svg in svg_buttons:
                            # 找到SVG的父按钮元素
                            parent_button = await svg.evaluate_handle('el => el.closest("button, div[role=button], [role=button]")')
                            if parent_button:
                                # 检查按钮是否包含文字内容
                                button_text = await parent_button.text_content()
                                # 如果按钮包含"下载"文字，跳过（这是底部按钮）
                                if button_text and "下载" in button_text.strip():
                                    continue
                                
                                # 检查按钮是否可见
                                is_visible = await parent_button.is_visible()
                                if is_visible:
                                    button_box = await parent_button.bounding_box()
                                    if button_box:
                                        x_in_range = (button_box['x'] >= img_box['x'] - 20 and 
                                                     button_box['x'] <= img_box['x'] + img_box['width'] + 20)
                                        y_in_range = (button_box['y'] >= img_box['y'] - 20 and 
                                                     button_box['y'] <= img_box['y'] + img_box['height'] + 20)
                                        not_at_bottom = button_box['y'] < img_box['y'] + img_box['height'] + 50
                                        
                                        if x_in_range and y_in_range and not_at_bottom:
                                            download_button = parent_button
                                            break
                    except Exception as e:
                        pass
                
                if download_button:
                    try:
                        # 确认下载按钮位置
                        button_box = await download_button.bounding_box()
                        
                        # 最后一次检查：确保按钮不在页面底部
                        if button_box and button_box['y'] > img_box['y'] + img_box['height'] + 100:
                            download_button = None
                        else:
                            # 点击下载按钮
                            await download_button.click()
                            
                            # 等待下载开始
                            for i in range(30):  # 等待最多30秒
                                if download_info['started']:
                                    break
                                await asyncio.sleep(1)
                            
                            if download_info['started']:
                                # 等待下载完成
                                for i in range(60):  # 等待最多60秒
                                    if download_info['completed']:
                                        break
                                    await asyncio.sleep(1)
                                
                                if download_info['completed']:
                                    # 移除下载监听器
                                    try:
                                        self.page.remove_listener('download', handle_download)
                                    except:
                                        pass
                                    return True
                            
                    except Exception as e:
                        pass
                
                # 移除下载监听器
                try:
                    self.page.remove_listener('download', handle_download)
                except:
                    pass
                
                if attempt < max_retries - 1:
                    await asyncio.sleep(2)
                    continue
                    
            except Exception as e:
                if attempt < max_retries - 1:
                    await asyncio.sleep(2)
                    continue
        
        return False

    async def scroll_to_load_more_conversations(self):
        """滚动加载更多历史对话，直到内容不再变化"""
        try:
            print("开始滚动加载历史对话栏...")
            # 专门滚动左侧历史对话区域（红色框区域）
            # 查找最有可能包含聊天列表项的可滚动容器
            scrollable_container = None
            sidebar_selectors = [
                'div[class*="sidebar"]',
                'div[class*="history"]', 
                'div[class*="chat-list"]',
                '.wrapper-CDsNzq',
                'div[class*="conversation-list"]',
                'aside',
                'nav'
            ]

            for selector in sidebar_selectors:
                containers = await self.page.query_selector_all(selector)
                for container in containers:
                    # 检查是否是可滚动的容器且包含聊天列表项
                    if container and \
                       await container.evaluate('(el) => el.scrollHeight > el.clientHeight') and \
                       await container.query_selector('span[data-testid="chat_list_item_title"]'):
                        scrollable_container = container
                        break
                if scrollable_container:
                    break
            
            if not scrollable_container:
                # 备用方案：尝试找到包含聊天项的可滚动父元素
                chat_items = await self.page.query_selector_all('span[data-testid="chat_list_item_title"]')
                if chat_items:
                    parent = await chat_items[0].evaluate_handle('el => el.parentElement')
                    document_body_handle = await self.page.evaluate_handle('document.body') # 获取document.body的句柄
                    while parent and parent != document_body_handle: # 使用Python的!=运算符进行比较
                        if await parent.evaluate('(el) => el.scrollHeight > el.clientHeight'):
                            scrollable_container = parent
                            break
                        parent = await parent.evaluate_handle('el => el.parentElement')

            if scrollable_container:
                previous_height = 0
                max_scroll_attempts = 10 # 防止无限循环
                for i in range(max_scroll_attempts):
                    current_scroll_height = await scrollable_container.evaluate('(el) => el.scrollHeight')
                    if current_scroll_height == previous_height:
                        print(f"  滚动加载完成，内容不再变化 (尝试 {i+1})")
                        break
                    previous_height = current_scroll_height
                    await scrollable_container.evaluate('(el) => el.scrollTop = el.scrollHeight')
                    await asyncio.sleep(2) # 留出时间让新内容加载
                    print(f"  第 {i+1} 次滚动加载历史对话，当前高度: {current_scroll_height}")
            else:
                print("未找到可滚动的历史对话容器。")
            
            # 最终备用方案：以防万一，滚动整个页面
            await self.page.evaluate('window.scrollTo(0, document.body.scrollHeight)')
            await asyncio.sleep(2)

            print("历史对话滚动加载完成")
            
        except Exception as e:
            print(f"滚动加载历史对话时出错: {str(e)}")
            pass

    async def download_images_from_conversation(self, conversation_number):
        """从当前对话中下载所有图片"""
        try:
            print(f"  开始处理对话 {conversation_number}...")
            
            # 先滚动对话内容区域以确保所有图片都可见
            await self.scroll_conversation_content()
            
            # 增加等待时间，让图片完全加载
            await asyncio.sleep(SCROLL_CONTENT_WAIT_TIME)  # 使用配置常量
            
            print(f"  开始查找对话 {conversation_number} 中的图片...")
            
            # 使用持续等待机制查找图片容器
            print(f"  使用 div[data-testid=\"mdbox_image\"] 选择器查找图片...")
            print(f"  最大等待时间: {IMAGE_CONTAINER_WAIT_MAX_TIME} 秒，检查间隔: {IMAGE_CONTAINER_CHECK_INTERVAL} 秒")
            
            final_images = []
            seen_positions = set()  # 位置去重
            
            # 持续等待直到找到图片容器
            start_wait_time = time.time()
            image_containers = []
            
            while True:
                try:
                    # 查找所有图片容器
                    image_containers = await self.page.query_selector_all('div[data-testid="mdbox_image"]')
                    current_wait_time = time.time() - start_wait_time
                    
                    if len(image_containers) > 0:
                        print(f"    ✅ 找到 {len(image_containers)} 个图片容器 (等待了 {current_wait_time:.1f} 秒)")
                        break
                    else:
                        print(f"    ⏳ 暂未找到图片容器，已等待 {current_wait_time:.1f} 秒，继续等待...")
                        
                        # 检查是否超时
                        if current_wait_time >= IMAGE_CONTAINER_WAIT_MAX_TIME:
                            print(f"    ❌ 等待超时 ({IMAGE_CONTAINER_WAIT_MAX_TIME} 秒)，未找到图片容器")
                            break
                        
                        # 等待一段时间后再次检查
                        await asyncio.sleep(IMAGE_CONTAINER_CHECK_INTERVAL)
                        
                        # 在等待过程中，可以尝试重新滚动内容
                        if int(current_wait_time) % 10 == 0 and current_wait_time > 0:  # 每10秒重新滚动一次
                            print(f"    🔄 重新滚动内容以刷新图片加载...")
                            await self.scroll_conversation_content()
                            await asyncio.sleep(2)
                        
                except Exception as e:
                    current_wait_time = time.time() - start_wait_time
                    print(f"    查找图片容器时出错 (已等待 {current_wait_time:.1f} 秒): {e}")
                    
                    if current_wait_time >= IMAGE_CONTAINER_WAIT_MAX_TIME:
                        print(f"    ❌ 等待超时，停止查找")
                        break
                    
                    await asyncio.sleep(IMAGE_CONTAINER_CHECK_INTERVAL)
                    continue
            
            # 如果最终还是没找到图片容器
            if not image_containers:
                print(f"  对话栏 {conversation_number} 中没有找到图片容器")
                self.no_images_conversations.append(conversation_number)
                return 0
            
            try:
                # 从每个容器中提取图片信息
                for i, container in enumerate(image_containers):
                    try:
                        print(f"    处理第 {i+1}/{len(image_containers)} 个图片容器...")
                        
                        # 获取容器位置信息
                        bounding_box = await container.bounding_box()
                        if not bounding_box:
                            print(f"      容器 {i+1} 无法获取位置信息")
                            continue
                        
                        # 检查位置是否重复
                        position_key = f"{int(bounding_box['x'])},{int(bounding_box['y'])},{int(bounding_box['width'])},{int(bounding_box['height'])}"
                        
                        if position_key in seen_positions:
                            print(f"      容器 {i+1} 位置重复，跳过")
                            continue
                        
                        # 添加到最终列表
                        final_images.append({
                            'container': container,
                            'x': bounding_box['x'],
                            'y': bounding_box['y'],
                            'width': bounding_box['width'],
                            'height': bounding_box['height'],
                            'container_index': i + 1
                        })
                        
                        seen_positions.add(position_key)
                        
                        print(f"      容器 {i+1} 成功添加: 位置({bounding_box['x']:.0f}, {bounding_box['y']:.0f})")
                        
                    except Exception as e:
                        print(f"      处理容器 {i+1} 时出错: {e}")
                        continue
                
                if not final_images:
                    print(f"  对话栏 {conversation_number} 中没有找到任何有效图片容器")
                    self.no_images_conversations.append(conversation_number)
                    return 0
                
                # 按照从上到下、从左到右的顺序排序
                # 首先按Y坐标（行）排序，然后按X坐标（列）排序
                # 使用四舍五入避免微小的坐标差异影响排序
                final_images.sort(key=lambda img: (round(img['y'], -1), round(img['x'], -1)))
                
                print(f"  对话栏 {conversation_number} 最终确认找到 {len(final_images)} 个有效图片容器")
                print(f"  图片容器排序后的位置信息:")
                for i, img_info in enumerate(final_images):
                    print(f"    第 {i+1} 张: 位置({img_info['x']:.0f}, {img_info['y']:.0f}), 尺寸({img_info['width']:.0f}x{img_info['height']:.0f}), 来源容器{img_info['container_index']}")
                
                # 开始下载图片
                print(f"  开始下载 {len(final_images)} 个图片容器中的图片...")
                downloaded_count = 0
                page_refreshed = False  # 标记页面是否已刷新
                
                for i, img_info in enumerate(final_images):
                    try:
                        # 生成文件名
                        filename = f"{conversation_number}-{i+1:02d}.jpg"
                        image_id = f"{conversation_number}-{i+1}"
                        
                        print(f"    开始下载图片 {image_id} (容器 {img_info['container_index']})")
                        
                        # 如果页面已经刷新过，需要重新查找容器
                        current_container = img_info['container']
                        current_img_element = None
                        
                        if page_refreshed:
                            try:
                                # 重新查找所有图片容器
                                fresh_containers = await self.page.query_selector_all('div[data-testid="mdbox_image"]')
                                if len(fresh_containers) > i:
                                    current_container = fresh_containers[i]
                                    print(f"    页面刷新后重新定位容器 {image_id}")
                                else:
                                    print(f"    页面刷新后找不到容器 {image_id}，跳过")
                                    self.failed_images.append(image_id)
                                    continue
                            except Exception as e:
                                print(f"    重新定位容器 {image_id} 失败: {e}")
                                self.failed_images.append(image_id)
                                continue
                        
                        # 检查容器是否仍然有效
                        try:
                            is_visible = await current_container.is_visible()
                            if not is_visible:
                                print(f"    图片容器 {image_id} 不可见，跳过")
                                continue
                        except Exception as e:
                            print(f"    图片容器 {image_id} 已失效: {e}")
                            # 如果容器失效，跳过该图片，让下载重试逻辑处理
                            continue
                        
                        # 在容器内查找img元素用于hover下载
                        try:
                            current_img_element = await current_container.query_selector('img')
                            if not current_img_element:
                                print(f"    容器 {image_id} 中没有找到img元素")
                                continue
                        except Exception as e:
                            print(f"    查找容器 {image_id} 中的img元素失败: {e}")
                            continue
                        
                        # 直接使用hover方式下载
                        print(f"    容器 {image_id} 使用hover方式下载...")
                        
                        # 尝试下载，最多重试3次
                        download_success = False
                        max_download_retries = MAX_DOWNLOAD_RETRIES  # 使用配置常量
                        
                        for retry_count in range(max_download_retries):
                            try:
                                if retry_count > 0:
                                    print(f"    图片 {image_id} 第 {retry_count + 1} 次重试下载...")
                                
                                success = await self.download_image_by_hover(current_img_element, filename, max_retries=HOVER_DOWNLOAD_MAX_RETRIES)  # 使用配置常量
                                if success:
                                    downloaded_count += 1
                                    print(f"    图片 {image_id} 下载成功")
                                    download_success = True
                                    break
                                else:
                                    if retry_count < max_download_retries - 1:
                                        print(f"    图片 {image_id} 第 {retry_count + 1} 次尝试失败，准备重试...")
                                        
                                        # 第一次失败：尝试刷新页面
                                        if retry_count == 0:
                                            print(f"    第 {retry_count + 1} 次重试：刷新页面...")
                                            try:
                                                await self.page.reload(wait_until='networkidle', timeout=PAGE_REFRESH_TIMEOUT)  # 使用配置常量
                                                await asyncio.sleep(PAGE_REFRESH_WAIT_TIME)  # 使用配置常量
                                                page_refreshed = True
                                                
                                                # 重新滚动到对话内容
                                                await self.scroll_conversation_content()
                                                await asyncio.sleep(CONTENT_RESCROLL_WAIT_TIME)  # 使用配置常量
                                                
                                                # 重新查找该图片容器
                                                fresh_containers = await self.page.query_selector_all('div[data-testid="mdbox_image"]')
                                                if len(fresh_containers) > i:
                                                    current_container = fresh_containers[i]
                                                    current_img_element = await current_container.query_selector('img')
                                                    if current_img_element:
                                                        print(f"    刷新页面后成功重新定位容器 {image_id}")
                                                        continue
                                                    else:
                                                        print(f"    刷新页面后找不到容器 {image_id} 的img元素")
                                                        break
                                                else:
                                                    print(f"    刷新页面后找不到容器 {image_id}")
                                                    break
                                                    
                                            except Exception as refresh_error:
                                                print(f"    刷新页面失败: {refresh_error}")
                                                continue
                                        
                                        # 第二次失败：尝试重新导航到对话
                                        elif retry_count == 1:
                                            print(f"    第 {retry_count + 1} 次重试：重新导航到对话...")
                                            try:
                                                await self.navigate_to_doubao()
                                                await asyncio.sleep(PAGE_LOAD_WAIT_TIME)  # 使用配置常量
                                                
                                                # 重新找到并点击对话
                                                await self.ensure_sidebar_visible()
                                                await self.scroll_to_load_more_conversations()
                                                fresh_conversations = await self.find_numbered_conversations()
                                                
                                                target_conv = None
                                                for conv in fresh_conversations:
                                                    if int(conv['title']) == conversation_number:
                                                        target_conv = conv
                                                        break
                                                
                                                if target_conv:
                                                    success = await self.click_conversation(target_conv)
                                                    if success:
                                                        await self.scroll_conversation_content()
                                                        await asyncio.sleep(CONVERSATION_REENTER_WAIT_TIME)  # 使用配置常量
                                                        page_refreshed = True
                                                        
                                                        # 重新查找图片容器
                                                        fresh_containers = await self.page.query_selector_all('div[data-testid="mdbox_image"]')
                                                        if len(fresh_containers) > i:
                                                            current_container = fresh_containers[i]
                                                            current_img_element = await current_container.query_selector('img')
                                                            if current_img_element:
                                                                print(f"    重新导航后成功定位容器 {image_id}")
                                                                continue
                                                            else:
                                                                print(f"    重新导航后找不到容器 {image_id} 的img元素")
                                                                break
                                                        else:
                                                            print(f"    重新导航后找不到容器 {image_id}")
                                                            break
                                                    else:
                                                        print(f"    重新导航后无法点击对话 {conversation_number}")
                                                        break
                                                else:
                                                    print(f"    重新导航后找不到对话 {conversation_number}")
                                                    break
                                                    
                                            except Exception as nav_error:
                                                print(f"    重新导航失败: {nav_error}")
                                                continue
                                    else:
                                        # 最后一次尝试也失败了
                                        print(f"    图片 {image_id} 经过 {max_download_retries} 次重试仍然失败")
                                        break
                                        
                            except Exception as download_error:
                                print(f"    图片 {image_id} 第 {retry_count + 1} 次下载尝试出现异常: {download_error}")
                                if retry_count < max_download_retries - 1:
                                    continue
                                else:
                                    break
                        
                        if not download_success:
                            print(f"    图片 {image_id} 经过 {max_download_retries} 次完整重试后仍然失败，跳过")
                            self.failed_images.append(image_id)
                        
                        # 添加延迟避免操作过快
                        await asyncio.sleep(IMAGE_CONTAINER_PROCESS_DELAY)  # 使用配置常量
                        
                    except Exception as e:
                        image_id = f"{conversation_number}-{i+1}"
                        print(f"    图片 {image_id} 下载失败（处理异常）: {e}")
                        self.failed_images.append(image_id)
                        continue
                
                print(f"  对话栏 {conversation_number} 完成，成功下载 {downloaded_count}/{len(final_images)} 张图片")
                return downloaded_count
                
            except Exception as e:
                print(f"  查找图片容器时出错: {e}")
                self.no_images_conversations.append(conversation_number)
                return 0
            
        except Exception as e:
            print(f"  下载对话 {conversation_number} 的图片时发生错误: {e}")
            return 0
    
    async def clean_non_image_files(self):
        """清理下载目录中的UUID格式文稿文件"""
        try:
            if not os.path.exists(self.download_dir):
                return
            
            # UUID格式的正则表达式：8-4-4-4-12位十六进制字符
            import re
            uuid_pattern = re.compile(r'^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$', re.IGNORECASE)
            
            # 遍历下载目录
            files_to_delete = []
            
            for filename in os.listdir(self.download_dir):
                file_path = os.path.join(self.download_dir, filename)
                if os.path.isfile(file_path):
                    # 获取文件名（不包含扩展名）
                    name_without_ext = os.path.splitext(filename)[0]
                    
                    # 如果文件名符合UUID格式，标记为删除
                    if uuid_pattern.match(name_without_ext):
                        files_to_delete.append(file_path)
            
            # 删除UUID格式的文稿文件
            deleted_count = 0
            deleted_files = []
            
            for file_path in files_to_delete:
                try:
                    filename = os.path.basename(file_path)
                    os.remove(file_path)
                    deleted_count += 1
                    deleted_files.append(filename)
                except Exception as e:
                    pass
            
            if deleted_count > 0:
                print(f"清理了 {deleted_count} 个UUID格式的文稿文件:")
                for filename in deleted_files:
                    print(f"  删除: {filename}")
            else:
                print("没有找到需要清理的UUID格式文稿文件")
            
        except Exception as e:
            pass

    async def process_all_conversations(self):
        """处理所有编号对话"""
        try:
            self.start_time = time.time()
            total_downloaded = 0
            total_conversations = 0
            
            # 首先确保侧边栏可见
            sidebar_visible = await self.ensure_sidebar_visible()
            if not sidebar_visible:
                print("无法显示侧边栏，终止处理")
                return 0
            
            # 滚动加载更多对话
            print("\n开始滚动加载历史对话栏...")
            await self.scroll_to_load_more_conversations()
            
            # 查找所有编号对话
            print("\n正在查找编号对话栏...")
            conversations = await self.find_numbered_conversations()
            
            if not conversations:
                print("没有找到任何编号对话")
                return 0
            
            # 让用户选择要下载的对话栏
            selected_conversations = await self.select_conversations_to_download(conversations)
            
            if selected_conversations is None:
                # 用户选择退出
                print("程序已退出")
                return 0
            
            if not selected_conversations:
                print("没有选择任何对话栏")
                return 0
            
            # 显示最终选择的对话栏信息
            conversation_names = [conv['title'] for conv in selected_conversations]
            print(f"\n开始下载 {len(selected_conversations)} 个对话栏: {', '.join(conversation_names)}")
            print(f"DEBUG: selected_conversations at loop start: {[c['title'] for c in selected_conversations]}") # Added debug
            
            for i, conv in enumerate(selected_conversations):
                try:
                    print(f"\n--- DEBUG: Starting processing for iteration {i+1}, conversation: {conv['title']} ---") # New debug print
                    print(f"正在处理第 {i+1}/{len(selected_conversations)} 个对话栏: {conv['title']}")
                    print(f"DEBUG: Current conv title at iteration start (from selected_conversations): {conv['title']}") # Added debug
                    
                    # 重新确保侧边栏可见
                    await self.ensure_sidebar_visible()
                    print("  重新确保侧边栏可见并等待页面稳定...")
                    # 等待聊天项附加到DOM上
                    await self.page.wait_for_selector('span[data-testid="chat_list_item_title"]', state='attached', timeout=15000)
                    
                    # 重新滚动加载更多对话，以确保所有对话都可见
                    print("  重新滚动加载历史对话...")
                    await self.scroll_to_load_more_conversations()
                    
                    # 重新查找对话元素以避免DOM分离问题
                    print(f"  当前迭代正在查找对话栏 (from loop variable): {conv['title']}") # Existing debug print
                    fresh_conversations = await self.find_numbered_conversations()
                    # Ensure fresh_conversations is sorted by number, defensively
                    fresh_conversations.sort(key=lambda x: int(x['title'])) # Add this line
                    print(f"DEBUG: Freshly found conversations (after re-sort in process_all_conversations): {[c['title'] for c in fresh_conversations]}") # Updated debug print
                    target_conv = None
                    current_conv_number = int(conv['title']) # Convert current conv title to integer for robust comparison
                    for fresh_conv in fresh_conversations:
                        # Compare numbers (integers) instead of strings to avoid subtle issues
                        fresh_conv_number = int(fresh_conv['title'])
                        print(f"DEBUG: Comparing fresh_conv_number={fresh_conv_number} with current_conv_number={current_conv_number}") # New debug
                        if fresh_conv_number == current_conv_number:
                            target_conv = fresh_conv
                            break
                    
                    if not target_conv:
                        print(f"  无法重新找到对话栏 {conv['title']}")
                        print(f"DEBUG: Target conversation {conv['title']} not found in fresh_conversations.") # New debug
                        continue
                    
                    # 点击对话栏
                    success = await self.click_conversation(target_conv)
                    if not success:
                        print(f"  无法点击对话栏 {conv['title']}")
                        print(f"DEBUG: Failed to click conversation {conv['title']}.") # New debug
                        continue
                    
                    print(f"  成功进入对话栏 {conv['title']}")
                    
                    # 下载该对话中的所有图片
                    downloaded = await self.download_images_from_conversation(conv['number'])
                    total_downloaded += downloaded
                    total_conversations += 1
                    
                    # 返回到主页面
                    try:
                        await self.navigate_to_doubao()
                        await asyncio.sleep(2) # 添加一个短暂的等待时间，确保页面加载稳定
                    except Exception as nav_error:
                        print(f"  返回主页面时出错: {nav_error}")
                        # 尝试重新初始化页面
                        try:
                            await self.page.goto("https://www.doubao.com/chat/create-image", wait_until='networkidle', timeout=30000)
                            await asyncio.sleep(3)
                        except Exception as goto_error:
                            print(f"  重新导航到主页面失败: {goto_error}")
                    
                    print(f"--- DEBUG: Finished processing for iteration {i+1}, conversation: {conv['title']} ---") # New debug print
                    
                except Exception as conv_error:
                    print(f"  处理对话栏 {conv['title']} 时发生异常: {conv_error}")
                    print(f"  跳过该对话栏，继续处理下一个...")
                    # 尝试恢复到主页面
                    try:
                        await self.navigate_to_doubao()
                        await asyncio.sleep(2)
                    except Exception as recovery_error:
                        print(f"  恢复到主页面失败: {recovery_error}")
                    continue

            # 清理UUID格式的文稿文件
            print("\n正在清理UUID格式的文稿文件...")
            await self.clean_non_image_files()
            
            # 计算统计信息
            end_time = time.time()
            total_time_seconds = end_time - self.start_time
            total_time_minutes = total_time_seconds / 60
            avg_time_per_image_seconds = total_time_seconds / total_downloaded if total_downloaded > 0 else 0
            
            print(f"\n=== 下载完成统计 ===")
            print(f"总共处理了 {total_conversations} 个对话栏")
            print(f"成功下载了 {total_downloaded} 张图片")
            print(f"总耗时: {total_time_seconds:.2f} 秒 ({total_time_minutes:.2f} 分钟)")
            print(f"平均每张图片耗时: {avg_time_per_image_seconds:.2f} 秒")
            
            if self.failed_images:
                print(f"重试5次后仍失败的图片编号: {', '.join(self.failed_images)}")
            else:
                print("所有图片都下载成功！")
            
            # 报告没有找到图片的对话栏
            if self.no_images_conversations:
                print(f"\n=== 没有找到图片的对话栏 ===")
                print(f"以下 {len(self.no_images_conversations)} 个对话栏在刷新后仍然没有找到图片:")
                for conv_num in self.no_images_conversations:
                    print(f"  对话栏编号: {conv_num}")
            else:
                print("\n所有对话栏都找到了图片！")
            
            return total_downloaded
            
        except Exception as e:
            print(f"处理所有对话时发生严重错误: {e}")
            return 0

    async def select_conversations_to_download(self, conversations):
        """让用户选择要下载的对话栏"""
        try:
            if not conversations:
                print("没有找到任何对话栏")
                return []
            
            # Create a mapping from conversation number (title) to conversation object
            conversation_map = {int(conv['title']): conv for conv in conversations}
            available_numbers = sorted(list(conversation_map.keys()))

            print(f"\n=== 找到 {len(conversations)} 个对话栏 ===")
            for conv in conversations: # Iterating through original conversations list (sorted by number)
                print(f"  对话栏编号: {conv['title']}") # Display the actual conversation number
            
            print("\n请选择要下载的对话栏:")
            print(f"  输入对话栏编号选择单个对话栏，例如: {available_numbers[0]}") # Use an example from available numbers
            print(f"  输入多个对话栏编号选择多个对话栏，用逗号分隔，例如: {available_numbers[0]},{available_numbers[1]}")
            print("  输入 'all' 或 'a' 选择全部对话栏")
            print("  输入 'quit' 或 'q' 退出程序")
            
            while True:
                try:
                    user_input = input("\n请输入您的选择: ").strip().lower()
                    
                    if user_input in ['quit', 'q']:
                        print("用户选择退出程序")
                        return None
                    
                    if user_input in ['all', 'a']:
                        print(f"已选择全部 {len(conversations)} 个对话栏")
                        return conversations
                    
                    selected_conv_numbers = []
                    if ',' in user_input:
                        parts = user_input.split(',')
                        for part in parts:
                            try:
                                conv_num = int(part.strip())
                                if conv_num in conversation_map:
                                    selected_conv_numbers.append(conv_num)
                                else:
                                    print(f"错误: 对话栏编号 '{part.strip()}' 不存在。")
                                    selected_conv_numbers = [] # Clear selection on error
                                    break
                            except ValueError:
                                print(f"错误: '{part.strip()}' 不是有效的对话栏编号。")
                                selected_conv_numbers = [] # Clear selection on error
                                break
                        else: # Only executed if the loop completes without a break
                            if selected_conv_numbers:
                                selected_conversations = [conversation_map[num] for num in selected_conv_numbers]
                                print(f"已选择 {len(selected_conversations)} 个对话栏:")
                                for conv in selected_conversations:
                                    print(f"  - {conv['title']}")
                                
                                confirm = input("\n确认选择? (y/n): ").strip().lower()
                                if confirm in ['y', 'yes', '是']:
                                    return selected_conversations
                                else:
                                    print("请重新选择")
                                    continue
                            else:
                                print("错误: 没有选择任何有效的对话栏。")
                                continue
                    else:
                        try:
                            conv_num = int(user_input)
                            if conv_num in conversation_map:
                                selected_conversation = conversation_map[conv_num]
                                print(f"已选择对话栏: {selected_conversation['title']}")
                                
                                confirm = input("\n确认选择? (y/n): ").strip().lower()
                                if confirm in ['y', 'yes', '是']:
                                    return [selected_conversation]
                                else:
                                    print("请重新选择")
                                    continue
                            else:
                                print(f"错误: 对话栏编号 '{user_input}' 不存在。")
                                continue
                        except ValueError:
                            print(f"错误: '{user_input}' 不是有效的对话栏编号。")
                            continue
                
                except KeyboardInterrupt:
                    print("\n\n用户中断程序")
                    return None
                except Exception as e:
                    print(f"输入处理错误: {str(e)}")
                    continue
                    
        except Exception as e:
            logger.error(f"选择对话栏时发生错误: {str(e)}")
            return []

    async def debug_page_images(self):
        """调试：查看页面上所有的图片元素"""
        try:
            all_images = await self.page.query_selector_all('img')
            logger.info(f"页面上总共有 {len(all_images)} 个img元素")
            
            target_images = await self.page.query_selector_all(f'img.{IMAGE_CLASS_NAME}')
            logger.info(f"其中有 {len(target_images)} 个使用{IMAGE_CLASS_NAME}类")
            
            for i, img in enumerate(target_images):
                src = await img.get_attribute('src')
                is_visible = await img.is_visible()
                bounding_box = await img.bounding_box()
                logger.info(f"  图片 {i+1}: visible={is_visible}, src={src[:50] if src else 'None'}...")
                if bounding_box:
                    logger.info(f"    位置: x={bounding_box['x']}, y={bounding_box['y']}, w={bounding_box['width']}, h={bounding_box['height']}")
        except Exception as e:
            logger.error(f"调试图片时出错: {str(e)}")

async def main():
    """主函数"""
    downloader = None
    try:
        # 创建下载器实例
        downloader = DoubaoImageDownloader()
        
        # 初始化浏览器
        success = await downloader.initialize_browser()
        if not success:
            print("错误：浏览器初始化失败")
            return
        
        # 导航到豆包页面
        success = await downloader.navigate_to_doubao()
        if not success:
            print("错误：无法导航到豆包页面")
            return
        
        print("开始下载图片...")
        
        # 处理所有对话并下载图片
        total_downloaded = await downloader.process_all_conversations()
        
        if total_downloaded > 0:
            print(f"\n图片保存在: {downloader.download_dir}")
        else:
            print("没有下载到任何图片")
            
        print("脚本执行完成，浏览器将保持打开状态")
        print("您可以继续在浏览器中操作，或手动关闭浏览器")
            
    except KeyboardInterrupt:
        print("\n用户中断脚本")
        print("浏览器将保持打开状态，您可以继续使用")
    except Exception as e:
        print(f"程序执行失败: {str(e)}")
        print("浏览器将保持打开状态")
    finally:
        # 停止防睡眠功能
        if downloader and hasattr(downloader, 'prevent_sleep'):
            downloader.prevent_sleep.stop()
        # 不关闭浏览器，让它独立运行
        print("脚本结束，浏览器保持运行中...")

if __name__ == "__main__":
    # 安装依赖包
    try:
        from playwright.async_api import async_playwright
    except ImportError:
        print("正在安装playwright包...")
        import subprocess
        subprocess.check_call(["pip", "install", "playwright"])
        subprocess.check_call(["playwright", "install", "chromium"])
        from playwright.async_api import async_playwright
    
    # 运行主程序
    asyncio.run(main()) 
```
