
# Azure TTS 语音生成脚本

```python
import os
import asyncio
import logging
import subprocess
import signal
import atexit
import time
import re
import requests
import traceback
from docx import Document
from pathlib import Path

# 导入自定义模块
from azure_tts import AzureTTSGenerator

# ============ 语音配置开始 ============
# Azure TTS配置
AZURE_SUBSCRIPTION_KEY = ""
AZURE_REGION = "eastus"

# 新Brainrot模式角色配置
BRAINROT_CHARACTERS = {
    "卡布奇诺": {
        "voice": "en-US-AriaNeural",      # 温柔女性声音
        "style": "cheerful",              # 愉快风格（AriaNeural支持）
        "volume": "+10%",                 # 音量+10%
    },
    "幼年卡布奇诺": {
        "voice": "en-US-AriaNeural",      # 与卡布奇诺共用声音
        "style": "cheerful",              # 愉快风格（AriaNeural支持）
        "volume": "+10%",                 # 音量+10%
    },
    "黑化小姐": {
        "voice": "en-US-JennyNeural",     # 阴狠女性声音
        "style": "unfriendly",            # 不友好风格（JennyNeural支持）
        "volume": "+10%",                 # 音量+10%
    },
    "幼年黑化小姐": {
        "voice": "en-US-JennyNeural",     # 与黑化小姐共用声音
        "style": "unfriendly",            # 不友好风格（JennyNeural支持）
        "volume": "+10%",                 # 音量+10%
    },
    "棍哥": {
        "voice": "en-US-GuyNeural",       # 普通男性声音
        "style": None,                    # 无特殊风格
        "volume": "+10%",                 # 音量+10%
    },
    "幼年棍哥": {
        "voice": "en-US-GuyNeural",       # 与棍哥共用声音
        "style": None,                    # 无特殊风格
        "volume": "+10%",                 # 音量+10%
    },
    "警察大象": {
        "voice": "en-US-SteffanNeural",   # 威严男性声音
        "style": None,                    # 无特殊风格（SteffanNeural可能不支持serious）
        "volume": "+10%",                 # 音量+10%
    },
    "囚犯鲨鱼": {
        "voice": "en-US-DavisNeural",     # 罪犯男性声音
        "style": "angry",                 # 愤怒风格（DavisNeural支持）
        "volume": "+10%",                 # 音量+10%
    },
    # 新增的6个角色
    "绿哥": {
        "voice": "en-US-JasonNeural",     # 使用Jason的男性声音
        "style": "cheerful",              # 愉快风格
        "volume": "+10%",                 # 音量+10%
    },
    "绿姐": {
        "voice": "en-US-EmmaNeural",      # 使用Emma的女性声音
        "style": "cheerful",              # 愉快风格
        "volume": "+10%",                 # 音量+10%
    },
    "橘子哥": {
        "voice": "en-US-TonyNeural",      # 使用Tony的男性声音
        "style": "excited",               # 兴奋风格
        "volume": "+10%",                 # 音量+10%
    },
    "橘子姐": {
        "voice": "en-US-AvaNeural",       # 使用Ava的女性声音
        "style": "excited",               # 兴奋风格
        "volume": "+10%",                 # 音量+10%
    },
    "咖啡男": {
        "voice": "en-US-AndrewNeural",    # 使用Andrew的男性声音
        "style": "friendly",              # 友好风格
        "volume": "+10%",                 # 音量+10%
    },
    "咖啡女": {
        "voice": "en-US-MichelleNeural",  # 使用Michelle的女性声音
        "style": "friendly",              # 友好风格
        "volume": "+10%",                 # 音量+10%
    },
    # 新增的4个角色
    "男1": {
        "voice": "en-US-JasonNeural",     # 使用Jason的男性声音
        "style": "cheerful",              # 愉快风格
        "volume": "+10%",                 # 音量+10%
    },
    "男2": {
        "voice": "en-US-GuyNeural",       # 使用Guy的男性声音
        "style": None,                    # 默认风格（无特殊风格）
        "volume": "+10%",                 # 音量+10%
    },
    "女1": {
        "voice": "en-US-AriaNeural",      # 使用Aria的女性声音
        "style": "cheerful",              # 愉快风格
        "volume": "+10%",                 # 音量+10%
    },
    "女2": {
        "voice": "en-US-SaraNeural",      # 使用Sara的女性声音（新增的Azure好用女声）
        "style": "friendly",              # 友好风格
        "volume": "+10%",                 # 音量+10%
    },
    # 男1的别名 - 多种写法都映射到男1
    "男": "男1",
    "Male": "男1", 
    "male": "男1",
    "man1": "男1",
    "Man1": "男1",
    "MAN1": "男1",
    "Male1": "男1",  # 添加Male1格式
    "MALE1": "男1",
    "male1": "男1",  # 添加小写male1格式
    "male 1": "男1", # 添加带空格的小写格式
    # 男2的别名 - 多种写法都映射到男2
    "man2": "男2",
    "Man2": "男2", 
    "MAN2": "男2",
    "Man 2": "男2",  # 添加带空格的格式
    "Male2": "男2",  # 添加Male2格式
    "MALE2": "男2",
    "male2": "男2",  # 添加小写male2格式
    "male 2": "男2", # 添加带空格的小写格式
    # 女1的别名 - 多种写法都映射到女1
    "女": "女1",
    "Female": "女1",
    "female": "女1", 
    "woman1": "女1",
    "Woman1": "女1",
    "WOMAN1": "女1",
    "women1": "女1",
    "Women1": "女1",
    "WOMEN1": "女1",
    "Woman 1": "女1",  # 添加带空格的格式
    "Female1": "女1",  # 添加Female1格式
    "FEMALE1": "女1",
    "female1": "女1",  # 添加小写female1格式
    "female 1": "女1", # 添加带空格的小写格式
    # 女2的别名 - 多种写法都映射到女2
    "woman2": "女2",
    "Woman2": "女2",
    "WOMAN2": "女2", 
    "women2": "女2",
    "Women2": "女2",
    "WOMEN2": "女2",
    "Female2": "女2",  # 添加Female2格式
    "FEMALE2": "女2",
    "female2": "女2",  # 添加小写female2格式
    "female 2": "女2", # 添加带空格的小写格式
}

# 语音参数配置
VOICE_CONFIG = {
    # 全局配置参数
    "global": {
        "speech_rate": 1.3,         # 语速，1.0为正常速度
        "pitch": "+0Hz",            # 音调，+0Hz为正常音调
        "volume": "+25%",           # 音量，+25%比正常音量增加25%
        "range": "0%",              # 语调范围，0%为默认范围
        "style_degree": "1",        # 风格强度，1为默认强度
        "enable_global_trailing_silence": True,  # 全局尾部静音控制，True时所有角色都会添加0.2秒尾部静音
    },
    
    # 角色特定配置
    "characters": {
        "messi": {
            "voice": "en-US-JasonNeural",
            "style": "angry",           # 愤怒
            "keep_trailing_silence": False,
        },
        "ronaldo": {
            "voice": "en-US-GuyNeural",
            "style": "angry",             # 愤怒
            "keep_trailing_silence": False,
        },
        "neymar": {
            "voice": "en-US-TonyNeural",
            "style": "cheerful",          # 愉快
            "keep_trailing_silence": False,
        },
        "mbappé": {
            "voice": "en-US-DavisNeural",
            "style": "angry",             # 愤怒
            "keep_trailing_silence": True,   # 为Mbappé特别保留尾部静音
            "add_space": True,               # 在对白末尾添加一个空格，确保尾音不被截断
        },
        "mbappe": {
            "voice": "en-US-DavisNeural",
            "style": "angry",             # 愤怒
            "keep_trailing_silence": True,   # 为Mbappé特别保留尾部静音
            "add_space": True,               # 在对白末尾添加一个空格，确保尾音不被截断
        },
        "coach": {
            "voice": "en-US-DerekMultilingualNeural",
            "style": None,                # 无特殊风格
            "keep_trailing_silence": False,
        },
        "rock": {
            "voice": "en-US-DerekMultilingualNeural",
            "style": None,                # 无特殊风格
            "keep_trailing_silence": False,
        },
        "dwayne": {
            "voice": "en-US-DerekMultilingualNeural",
            "style": None,                # 无特殊风格
            "keep_trailing_silence": False,
        },
        "dwayne johnson": {
            "voice": "en-US-DerekMultilingualNeural",
            "style": None,                # 无特殊风格
            "keep_trailing_silence": False,
        },
        "therock": {
            "voice": "en-US-DerekMultilingualNeural",
            "style": None,                # 无特殊风格
            "keep_trailing_silence": False,
        },
        "jack": {
            "voice": "en-US-DerekMultilingualNeural",
            "style": "cheerful",          # 愉快
            "keep_trailing_silence": False,
        },
        "jackblack": {
            "voice": "en-US-DerekMultilingualNeural",
            "style": "cheerful",          # 愉快
            "keep_trailing_silence": False,
        },
        # 新增的6个角色 - 简化配置，每个角色只保留主要配置
        "gwen stacy": {
            "voice": "en-US-AvaNeural",
            "style": "cheerful",          # 活泼开朗的蜘蛛女侠
            "keep_trailing_silence": False,
        },
        "catwoman": {
            "voice": "en-US-JennyNeural",
            "style": "unfriendly",        # 神秘冷酷的猫女
            "keep_trailing_silence": False,
        },
        "wonder woman": {
            "voice": "en-US-EmmaNeural",
            "style": "hopeful",           # 充满希望的神奇女侠
            "keep_trailing_silence": False,
        },
        "spiderman": {
            "voice": "en-US-AndrewNeural",
            "style": "excited",           # 兴奋活跃的蜘蛛侠
            "keep_trailing_silence": False,
        },
        "thor": {
            "voice": "en-US-SteffanNeural",
            "style": "angry",             # 威严愤怒的雷神
            "keep_trailing_silence": False,
        },
        "loki": {
            "voice": "en-US-BrianNeural",
            "style": "unfriendly",        # 狡猾邪恶的洛基
            "keep_trailing_silence": False,
        },
        # 新增的11个角色配置
        "black widow": {
            "voice": "en-US-AriaNeural",
            "style": "unfriendly",        # 冷酷神秘的黑寡妇
            "keep_trailing_silence": False,
        },
        "scarlet witch": {
            "voice": "en-US-JennyNeural",
            "style": "angry",             # 愤怒强大的绯红女巫
            "keep_trailing_silence": False,
        },
        "captain marvel": {
            "voice": "en-US-EmmaNeural",
            "style": "hopeful",           # 充满希望的惊奇队长
            "keep_trailing_silence": False,
        },
        "iron man": {
            "voice": "en-US-GuyNeural",
            "style": "cheerful",          # 自信幽默的钢铁侠
            "keep_trailing_silence": False,
        },
        "captain america": {
            "voice": "en-US-SteffanNeural",
            "style": "hopeful",           # 充满正义感的美国队长
            "keep_trailing_silence": False,
        },
        "supergirl": {
            "voice": "en-US-AvaNeural",
            "style": "cheerful",          # 活泼开朗的超级女孩
            "keep_trailing_silence": False,
        },
        "harley quinn": {
            "voice": "en-US-MichelleNeural",
            "style": "excited",           # 疯狂兴奋的小丑女
            "keep_trailing_silence": False,
        },
        "superman": {
            "voice": "en-US-JasonNeural",
            "style": "hopeful",           # 充满希望的超人
            "keep_trailing_silence": False,
        },
        "batman": {
            "voice": "en-US-DavisNeural",
            "style": "unfriendly",        # 冷酷严肃的蝙蝠侠
            "keep_trailing_silence": False,
        },
        "the flash": {
            "voice": "en-US-AndrewNeural",
            "style": "excited",           # 兴奋活跃的闪电侠
            "keep_trailing_silence": False,
        },
        "aquaman": {
            "voice": "en-US-BrianNeural",
            "style": "angry",             # 威严愤怒的海王
            "keep_trailing_silence": False,
        },
    }
}
# ============ 语音配置结束 ============

# 配置日志
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')
logger = logging.getLogger(__name__)

# 防止系统睡眠的进程
caffeinate_process = None

def prevent_sleep():
    """启动caffeinate进程防止系统睡眠"""
    global caffeinate_process
    logger.info("启动防止系统睡眠功能...")
    try:
        # 使用caffeinate命令防止系统睡眠，-d参数防止显示器睡眠，-i防止系统空闲睡眠
        caffeinate_process = subprocess.Popen(["caffeinate", "-di"], 
                                           stdout=subprocess.DEVNULL, 
                                           stderr=subprocess.DEVNULL)
        logger.info(f"防止系统睡眠功能已启动 (PID: {caffeinate_process.pid})")
        
        # 注册退出处理函数，确保脚本结束时终止caffeinate进程
        atexit.register(allow_sleep)
        
        # 注册信号处理，确保在接收到中断信号时也能终止caffeinate进程
        signal.signal(signal.SIGINT, signal_handler)
        signal.signal(signal.SIGTERM, signal_handler)
    except Exception as e:
        logger.error(f"启动防止系统睡眠功能失败: {str(e)}")

def restore_sleep(caffeinate_process):
    """关闭防止系统睡眠功能，允许系统睡眠"""
    if caffeinate_process:
        try:
            logger.info("关闭防止系统睡眠功能...")
            caffeinate_process.terminate()
            caffeinate_process.wait(timeout=2)
            logger.info("系统可以正常睡眠了")
        except Exception as e:
            logger.error(f"关闭防止系统睡眠功能失败: {str(e)}")
            try:
                caffeinate_process.kill()  # 如果终止失败，尝试强制杀死进程
            except:
                pass
        return None
    return None

def allow_sleep():
    """兼容旧版本的函数，允许系统睡眠"""
    global caffeinate_process
    if caffeinate_process:
        caffeinate_process = restore_sleep(caffeinate_process)

def signal_handler(sig, frame):
    """处理中断信号"""
    global caffeinate_process
    logger.info(f"接收到信号 {sig}，正在清理...")
    
    # 关闭防止系统睡眠功能
    if caffeinate_process:
        caffeinate_process = restore_sleep(caffeinate_process)
    
    # 重新引发信号，让Python默认处理程序处理
    signal.default_int_handler(sig, frame)

def select_mode():
    """选择运行模式"""
    print("\n=== 语音生成模式选择 ===")
    print("1. 默认模式 (多角色对话，从Messi文档读取)")
    print("2. Brainrot模式 (多角色对话，从brainrot.txt读取)")
    print("========================")
    
    while True:
        try:
            choice = input("请选择模式 (1 或 2): ").strip()
            if choice == "1":
                print("已选择: 默认模式")
                return "default"
            elif choice == "2":
                print("已选择: Brainrot模式")
                return "brainrot"
            else:
                print("无效选择，请输入 1 或 2")
        except KeyboardInterrupt:
            print("\n\n程序已取消")
            exit(0)

def find_brainrot_file():
    """在桌面上查找brainrot.txt文件"""
    desktop_path = os.path.expanduser("~/Desktop")
    brainrot_file_path = os.path.join(desktop_path, "brainrot.txt")
    
    if os.path.exists(brainrot_file_path):
        logger.info(f"找到Brainrot文件: {brainrot_file_path}")
        print(f"使用Brainrot文件: {brainrot_file_path}")
        return brainrot_file_path
    else:
        logger.error("桌面上没有找到brainrot.txt文件")
        print("错误: 桌面上没有找到brainrot.txt文件")
        return None

def extract_brainrot_dialogues(file_path):
    """从brainrot.txt文件中提取对话内容"""
    try:
        # 读取文件内容
        with open(file_path, 'r', encoding='utf-8') as f:
            content = f.read()
        
        logger.info(f"成功读取Brainrot文件，文件大小: {len(content)} 字符")
        
        # 按行分割内容
        lines = content.strip().split('\n')
        
        dialogues = []
        
        # 定义故事分隔符模式
        chinese_story_pattern = re.compile(r'^\s*故事\s*\d+\s*[:：]')
        
        # 解析每一行，查找角色名：英文内容格式
        for line_num, line in enumerate(lines, 1):
            line = line.strip()
            if not line:
                continue
            
            # 首先检查是否是中文故事分隔符
            if chinese_story_pattern.match(line):
                # 添加故事分隔标记
                dialogues.append({
                    'character': '<STORY_BREAK>',
                    'dialogue': line,
                    'line_number': line_num
                })
                logger.info(f"检测到中文故事分隔符: {line[:50]}{'...' if len(line) > 50 else ''}")
                continue
                
            # 然后尝试匹配【角色名】：对话内容格式
            bracket_match = re.match(r'^【([^】]+)】[：:](.+)', line)
            
            if bracket_match:
                character = bracket_match.group(1).strip()
                dialogue = bracket_match.group(2).strip()
                
                # 检查角色是否在已知角色列表中，如果是别名则解析为主角色名
                actual_character = character
                if character in BRAINROT_CHARACTERS:
                    # 如果是别名，获取实际角色名
                    if isinstance(BRAINROT_CHARACTERS[character], str):
                        actual_character = BRAINROT_CHARACTERS[character]
                    
                    # 检查实际角色名是否存在
                    if actual_character in BRAINROT_CHARACTERS and isinstance(BRAINROT_CHARACTERS[actual_character], dict):
                        dialogues.append({
                            'character': actual_character,  # 使用实际角色名存储
                            'dialogue': dialogue,
                            'line_number': line_num
                        })
                        logger.info(f"提取对话 {len(dialogues)}: {character} -> {actual_character} - {dialogue[:50]}{'...' if len(dialogue) > 50 else ''}")
                    else:
                        logger.warning(f"第{line_num}行发现未知角色: {character}")
                        print(f"警告: 第{line_num}行发现未知角色: {character}")
                else:
                    logger.warning(f"第{line_num}行发现未知角色: {character}")
                    print(f"警告: 第{line_num}行发现未知角色: {character}")
                continue
            
            # 如果没有匹配到【】格式，尝试原有的角色名：英文内容格式
            # 支持中文冒号和英文冒号，支持中文角色名（如：男1、男2、女1、女2）
            match = re.match(r'^([^：:]+)[：:](.+)', line)
            
            if match:
                character = match.group(1).strip()
                dialogue = match.group(2).strip()
                
                # 检查角色是否在已知角色列表中，如果是别名则解析为主角色名
                actual_character = character
                if character in BRAINROT_CHARACTERS:
                    # 如果是别名，获取实际角色名
                    if isinstance(BRAINROT_CHARACTERS[character], str):
                        actual_character = BRAINROT_CHARACTERS[character]
                    
                    # 检查实际角色名是否存在
                    if actual_character in BRAINROT_CHARACTERS and isinstance(BRAINROT_CHARACTERS[actual_character], dict):
                        dialogues.append({
                            'character': actual_character,  # 使用实际角色名存储
                            'dialogue': dialogue,
                            'line_number': line_num
                        })
                        logger.info(f"提取对话 {len(dialogues)}: {character} -> {actual_character} - {dialogue[:50]}{'...' if len(dialogue) > 50 else ''}")
                    else:
                        logger.warning(f"第{line_num}行发现未知角色: {character}")
                        print(f"警告: 第{line_num}行发现未知角色: {character}")
                else:
                    logger.warning(f"第{line_num}行发现未知角色: {character}")
                    print(f"警告: 第{line_num}行发现未知角色: {character}")
            else:
                logger.warning(f"第{line_num}行格式不正确: {line[:50]}{'...' if len(line) > 50 else ''}")
                print(f"警告: 第{line_num}行格式不正确，应为【角色名】：对话内容或角色名：对话内容格式（支持男1、男2、女1、女2、男、女、Male、Female、male、female、women、woman等）")
        
        # 计算实际对话数量（不包括故事分隔符）
        actual_dialogues = [d for d in dialogues if d['character'] != '<STORY_BREAK>']
        story_breaks = [d for d in dialogues if d['character'] == '<STORY_BREAK>']
        
        logger.info(f"成功提取 {len(actual_dialogues)} 段对话，{len(story_breaks)} 个故事分隔符")
        print(f"成功提取 {len(actual_dialogues)} 段对话，{len(story_breaks)} 个故事分隔符")
        
        # 输出角色统计
        character_count = {}
        for dialogue in actual_dialogues:
            char = dialogue['character']
            character_count[char] = character_count.get(char, 0) + 1
        
        print("角色对话统计:")
        for char, count in character_count.items():
            print(f"  {char}: {count} 句")
        
        return dialogues
        
    except Exception as e:
        logger.error(f"提取Brainrot对话时发生错误: {str(e)}")
        traceback.print_exc()
        return []

def process_brainrot_audio_generation(dialogues):
    """处理Brainrot模式的语音生成"""
    try:
        # 创建输出目录
        output_dir = os.path.expanduser("~/Downloads")
        os.makedirs(output_dir, exist_ok=True)
        logger.info(f"创建输出目录: {output_dir}")
        
        # 初始化Azure TTS生成器
        tts_generator = AzureTTSGenerator(AZURE_SUBSCRIPTION_KEY, AZURE_REGION)
        
                # 获取当前时间，格式为HHMM（避免冒号）
        current_time = time.strftime("%H%M")
        
        # 输出Brainrot模式配置信息
        print("\n=== Brainrot模式语音配置 ===")
        print(f"任务时间: {current_time}")
        print(f"语音配置:")
        for char, config in BRAINROT_CHARACTERS.items():
            # 只显示主角色配置，跳过别名
            if isinstance(config, dict):
                print(f"  {char}: {config['voice']} (风格: {config['style'] or '默认'}, 音量: {config['volume']})")
        print("=============================\n")
        
        audio_files = []
        successful_count = 0
        
        # 分析故事结构（基于故事分隔符分组）
        stories = []
        current_story = []
        
        for dialogue_data in dialogues:
            if dialogue_data['character'] == '<STORY_BREAK>':
                # 故事分隔符，保存当前故事并开始新故事
                if current_story:
                    stories.append(current_story)
                    logger.info(f"根据故事分隔符添加故事，当前故事数: {len(stories)}")
                    current_story = []
            else:
                # 非分隔符，添加到当前故事
                current_story.append(dialogue_data)
        
        # 添加最后一个故事（如果有）
        if current_story:
            stories.append(current_story)
            logger.info(f"添加最后一个故事，总故事数: {len(stories)}")
        
        # 如果没有找到任何故事，就把所有对话当作一个故事
        if not stories and dialogues:
            # 过滤掉故事分隔符
            actual_dialogues = [d for d in dialogues if d['character'] != '<STORY_BREAK>']
            if actual_dialogues:
                stories.append(actual_dialogues)
                logger.info("没有找到故事分隔符，将所有对话作为一个故事处理")
        
        # 输出每个故事的详细信息
        for story_idx, story in enumerate(stories):
            logger.info(f"故事 {story_idx+1} 包含 {len(story)} 段对话")
            if len(story) > 0:
                logger.info(f"故事 {story_idx+1} 首段: {story[0]['character']} - {story[0]['dialogue'][:50]}{'...' if len(story[0]['dialogue']) > 50 else ''}")
        
        logger.info(f"共检测到 {len(stories)} 个故事")
        
        # 处理每个故事的对话
        for story_idx, story_dialogues in enumerate(stories):
            logger.info(f"开始处理故事 {story_idx+1}, 共 {len(story_dialogues)} 段对话")
            
            for i, dialogue_data in enumerate(story_dialogues):
                character = dialogue_data['character']
                dialogue = dialogue_data['dialogue']
                line_number = dialogue_data['line_number']
                
                # 生成文件名: 故事编号-对话编号-当前时间.mp3
                story_num = story_idx + 1  # 故事编号从1开始
                dialogue_num = i + 1      # 对话编号从1开始
                current_timestamp = time.strftime("%H%M%S")  # 当前时间格式：HHMMSS
                output_filename = f"{story_num}-{dialogue_num}-{current_timestamp}.mp3"
                output_file = os.path.join(output_dir, output_filename)
                
                logger.info(f"开始生成故事 {story_num} 第 {dialogue_num} 段对话的语音文件...")
                logger.info(f"角色: {character}")
                logger.info(f"对话内容: {dialogue[:100]}{'...' if len(dialogue) > 100 else ''}")
                logger.info(f"输出文件: {output_filename}")
                
                # 获取角色配置，确保使用主角色名
                actual_character = character
                if character in BRAINROT_CHARACTERS:
                    # 如果是别名，获取实际角色名
                    if isinstance(BRAINROT_CHARACTERS[character], str):
                        actual_character = BRAINROT_CHARACTERS[character]
                
                # 使用实际角色名获取配置
                char_config = BRAINROT_CHARACTERS[actual_character]
                voice_name = char_config['voice']
                voice_style = char_config['style']
                voice_volume = char_config['volume']
                
                # 生成语音，添加重试机制
                max_retries = 3
                retry_count = 0
                success = False
                
                while retry_count < max_retries and not success:
                    try:
                        if retry_count > 0:
                            logger.info(f"第 {retry_count} 次重试生成故事 {story_num} 第 {dialogue_num} 段对话的语音...")
                        
                        logger.info(f"使用语音配置: {voice_name}, 风格: {voice_style or '默认'}, 音量: {voice_volume}")
                        
                        success = tts_generator.generate_speech(
                            text=dialogue,
                            output_file=output_file,
                            voice_name=voice_name,
                            speech_rate=1.0,  # 使用默认语速
                            style=voice_style,
                            volume=voice_volume,
                            keep_trailing_silence=False
                        )
                        
                        if not success:
                            logger.error(f"生成故事 {story_num} 第 {dialogue_num} 段对话语音失败，但未抛出异常")
                            retry_count += 1
                            if retry_count < max_retries:
                                logger.info("将在 3 秒后重试...")
                                time.sleep(3)
                                
                    except Exception as e:
                        logger.error(f"生成故事 {story_num} 第 {dialogue_num} 段对话语音时发生异常: {str(e)}")
                        logger.error(f"异常详情: {traceback.format_exc()}")
                        retry_count += 1
                        if retry_count < max_retries:
                            logger.info("将在 3 秒后重试...")
                            time.sleep(3)
                
                if success:
                    audio_files.append(output_file)
                    successful_count += 1
                    logger.info(f"成功生成故事 {story_num} 第 {dialogue_num} 段对话的音频文件: {output_filename}")
                    print(f"✓ 故事 {story_num} 第 {dialogue_num} 段对话 ({character}) 音频生成完成")
                else:
                    logger.error(f"生成故事 {story_num} 第 {dialogue_num} 段对话音频文件失败，已尝试 {max_retries} 次重试")
                    print(f"✗ 故事 {story_num} 第 {dialogue_num} 段对话 ({character}) 音频生成失败")
                
                # 添加延迟，避免API限制
                time.sleep(0.5)
        
        if audio_files:
            logger.info(f"Brainrot模式成功生成 {len(audio_files)} 个语音文件")
            return True, audio_files
        else:
            logger.error("Brainrot模式未能生成任何语音文件")
            return False, []
            
    except Exception as e:
        logger.error(f"Brainrot模式处理语音生成时发生错误: {str(e)}")
        logger.error(f"错误详情: {traceback.format_exc()}")
        return False, []

def find_word_document(keyword="Messi"):
    """在桌面上查找指定的文本文档"""
    desktop_path = os.path.expanduser("~/Desktop")
    
    # 优先查找Messi.rtf文件
    rtf_file_path = os.path.join(desktop_path, "Messi.rtf")
    if os.path.exists(rtf_file_path):
        logger.info(f"找到RTF文本文件: {rtf_file_path}")
        print(f"使用RTF文本文件: {rtf_file_path}")
        return rtf_file_path
    
    # 其次查找Messi.txt文件
    txt_file_path = os.path.join(desktop_path, "Messi.txt")
    if os.path.exists(txt_file_path):
        logger.info(f"找到文本文件: {txt_file_path}")
        print(f"使用文本文件: {txt_file_path}")
        return txt_file_path
    else:
        logger.warning("桌面上没有找到 Messi.txt 或 Messi.rtf 文件，将尝试查找 docx 文件")
        print("桌面上没有文本文件，将使用 docx 文件")
    
    # 如果找不到txt或rtf文件，则查找 docx 文件
    for file in os.listdir(desktop_path):
        if file.endswith(".docx") and keyword in file:
            docx_path = os.path.join(desktop_path, file)
            logger.info(f"找到Word文档: {docx_path}")
            print(f"使用Word文档: {docx_path}")
            return docx_path
    
    logger.error("桌面上没有找到相关文件")
    print("错误: 桌面上没有找到相关文件")
    return None

def preprocess_word_document(doc):
    """预处理Word文档，合并对话和场景描述"""
    processed_paragraphs = []
    i = 0
    while i < len(doc.paragraphs):
        current_text = doc.paragraphs[i].text.strip()
        
        # 检查是否是对话行（以角色名开头）
        if (current_text.startswith(("Messi:", "Neymar:", "Rockjohnson:", "Mbappé:", "Ronaldo:", "coach:", "Coach:", "the rock:", "The rock:", "the Rock:", "The Rock:", "rock:", "Rock:", "Coach Rock:")) and 
            i + 1 < len(doc.paragraphs)):
            
            next_text = doc.paragraphs[i + 1].text.strip()
            # 检查下一行是否是场景描述（包含"比例 「9:16」"或"比例「9:16」"）
            if "比例 「9:16」" in next_text or "比例「9:16」" in next_text:
                # 合并对话和场景描述
                merged_text = current_text + " " + next_text
                processed_paragraphs.append(merged_text)
                i += 2  # 跳过已处理的两行
                continue
        
        # 如果不需要合并，不添加到处理后的段落中
        i += 1
    
    return processed_paragraphs

def extract_dialogue_from_word(file_path):
    """从文本文件中提取英文对白，只提取English Translations之后的内容"""
    try:
        # 确保导入re模块
        import re
        
        # 判断文件类型
        if file_path.endswith('.docx'):
            # 如果是docx文件，使用原来的方法
            doc = Document(file_path)
            lines = [para.text.strip() for para in doc.paragraphs]
        elif file_path.endswith('.rtf'):
            # 如果是rtf文件，直接使用textutil命令转换
            logger.info("使用textutil将RTF文件转换为纯文本")
            import subprocess
            import tempfile
            import os
            
            # 创建临时文件
            temp_txt = tempfile.mktemp(suffix='.txt')
            
            try:
                # 使用textutil命令将RTF转换为纯文本
                subprocess.run(['textutil', '-convert', 'txt', '-output', temp_txt, file_path], check=True)
                
                # 读取转换后的文本文件
                with open(temp_txt, 'r', encoding='utf-8') as f:
                    lines = [line.strip() for line in f.readlines()]
                    
                logger.info(f"成功转换RTF文件，共读取 {len(lines)} 行")
                
                # 如果行数很少，可能是因为文件格式问题，尝试将整个文本按空行分割
                if len(lines) < 10:
                    with open(temp_txt, 'r', encoding='utf-8') as f:
                        content = f.read()
                    # 将文本按空行分割
                    lines = re.split(r'\n\s*\n', content)
                    lines = [line.strip() for line in lines]
                    logger.info(f"重新处理文本，分割为 {len(lines)} 段")
            except Exception as e:
                logger.error(f"转换RTF文件失败: {str(e)}")
                # 如果转换失败，尝试直接读取
                with open(file_path, 'r', encoding='utf-8', errors='ignore') as f:
                    content = f.read()
                # 将文本按行分割
                lines = content.split('\n')
                lines = [line.strip() for line in lines]
                logger.warning(f"直接读取RTF文件，共 {len(lines)} 行")
            
            finally:
                # 删除临时文件
                if os.path.exists(temp_txt):
                    os.remove(temp_txt)
        else:
            # 如果是txt文件，直接读取行
            with open(file_path, 'r', encoding='utf-8') as f:
                lines = [line.strip() for line in f.readlines()]
            logger.info(f"读取文本文件，共 {len(lines)} 行")
        
        # 查找English Translations部分
        english_section_start = -1
        for i, line in enumerate(lines):
            if "English Translations" in line:
                english_section_start = i + 1
                logger.info(f"找到英文对白标记: '{line}'")
                break
        
        if english_section_start == -1:
            logger.error("未找到English Translations标记，将尝试直接解析整个文件")
            english_section_start = 0
            
        logger.info(f"英文对白部分从第 {english_section_start+1} 行开始")
        
        # 收集所有对白，并根据新规则添加故事分隔标记
        dialogues = []
        
        # 跳过空行
        i = english_section_start
        while i < len(lines) and not lines[i].strip():
            i += 1
            
        # 定义发言人格式的正则表达式
        # 匹配格式：角色名 + 冒号（中文或英文）+ 内容
        # 但排除"Story X:"格式的行
        speaker_pattern = re.compile(r'^[^:：]+[:：].+$')
        story_pattern = re.compile(r'^\s*Story\s*\d+\s*[:：]')
        # 新增：匹配"故事 X：标题"格式的分隔符
        chinese_story_pattern = re.compile(r'^\s*故事\s*\d+\s*[:：]')
        
        # 处理每一行
        for line_idx in range(i, len(lines)):
            line = lines[line_idx].strip()
            
            # 跳过完全空行
            if not line:
                continue
            
            # 首先检查是否是Story标记（优先级最高）
            if story_pattern.match(line):
                # Story标记视为故事分隔符
                dialogues.append("<STORY_BREAK>")
                logger.info(f"检测到英文故事分隔符: {line[:50]}{'...' if len(line) > 50 else ''}")
            # 然后检查是否是中文故事标记
            elif chinese_story_pattern.match(line):
                # 中文故事标记视为故事分隔符
                dialogues.append("<STORY_BREAK>")
                logger.info(f"检测到中文故事分隔符: {line[:50]}{'...' if len(line) > 50 else ''}")
            # 然后检查是否符合发言人格式
            elif speaker_pattern.match(line):
                # 符合发言人格式，添加到对话列表
                dialogues.append(line)
                logger.info(f"添加对话: {line[:50]}{'...' if len(line) > 50 else ''}")
            else:
                # 不符合发言人格式，视为故事分隔符
                dialogues.append("<STORY_BREAK>")
                logger.info(f"检测到故事分隔符: {line[:50]}{'...' if len(line) > 50 else ''}")
        
        # 将所有对白合并为单一文本，保留故事分隔标记
        dialogue_text = '\n'.join(dialogues)
        
        # 计算实际对白数量（不包括故事分隔标记）
        dialogue_count = sum(1 for d in dialogues if d != "<STORY_BREAK>")
        story_break_count = sum(1 for d in dialogues if d == "<STORY_BREAK>")
        
        logger.info("成功从文件中提取到英文对白")
        logger.info(f"共提取到 {dialogue_count} 段对白，{story_break_count} 个故事分隔标记")
        
        # 输出故事分组信息
        if story_break_count > 0:
            stories = []
            current_story = []
            
            for dialogue in dialogues:
                if dialogue == "<STORY_BREAK>":
                    if current_story:
                        stories.append(current_story)
                        current_story = []
                else:
                    current_story.append(dialogue)
            
            # 添加最后一个故事
            if current_story:
                stories.append(current_story)
            
            logger.info(f"检测到 {len(stories)} 个故事:")
            for i, story in enumerate(stories):
                logger.info(f"  故事 {i+1}: {len(story)} 段对话")
                if story:
                    logger.info(f"    首句: {story[0][:50]}{'...' if len(story[0]) > 50 else ''}")
        
        return dialogue_text
    except Exception as e:
        logger.error(f"提取对白时发生错误: {str(e)}")
        traceback.print_exc()
        return None

def process_audio_generation(dialogue_text):
    """处理对白文本，生成语音文件"""
    try:
        # 创建输出目录
        output_dir = os.path.expanduser("~/Downloads")
        os.makedirs(output_dir, exist_ok=True)
        logger.info(f"创建输出目录: {output_dir}")
        
        # 初始化Azure TTS生成器
        tts_generator = AzureTTSGenerator(AZURE_SUBSCRIPTION_KEY, AZURE_REGION)
        
        # 输出当前配置信息
        config = tts_generator.get_current_config()
        print("\n=== 语音生成配置信息 ===")
        print(f"地区: {config['subscription_region']}")
        print(f"音频格式: {config['audio_format']}")
        print(f"全局尾部静音: {'启用 (所有角色+0.2秒)' if VOICE_CONFIG['global'].get('enable_global_trailing_silence', False) else '禁用'}")
        print("\n角色语音映射:")
        for character, voice in config['voices'].items():
            print(f"- {character}: {voice}")
        print("\n角色情感风格映射:")
        for character, style in config['styles'].items():
            print(f"- {character}: {style}")
        print("========================\n")
        
        # 分割对话行并处理每一行
        lines = dialogue_text.strip().split('\n')
        audio_files = []
        successful_count = 0  # 初始化计数器
        
        # 分析故事结构（基于特殊标记分隔不同故事）
        stories = []
        current_story = []
        
        # 根据特殊标记分组
        for line in lines:
            if line == "<STORY_BREAK>":
                # 故事分隔标记，保存当前故事并开始新故事
                if current_story:
                    stories.append(current_story)
                    logger.info(f"根据故事分隔标记添加故事，当前故事数: {len(stories)}")
                    current_story = []
            else:
                # 非分隔标记，添加到当前故事
                current_story.append(line.strip())
        
        # 添加最后一个故事（如果有）
        if current_story:
            stories.append(current_story)
            logger.info(f"添加最后一个故事，总故事数: {len(stories)}")
        
        # 如果没有找到任何故事，就把所有行当作一个故事
        if not stories and lines:
            # 过滤掉特殊标记和空行
            non_empty_lines = [line for line in lines if line.strip() and line != "<STORY_BREAK>"]
            if non_empty_lines:
                stories.append(non_empty_lines)
                logger.info("没有找到故事分隔，将所有对白作为一个故事处理")
        
        # 输出每个故事的详细信息
        for story_idx, story in enumerate(stories):
            logger.info(f"故事 {story_idx+1} 包含 {len(story)} 行对白")
            if len(story) > 0:
                logger.info(f"故事 {story_idx+1} 首行: {story[0]}")
                logger.info(f"故事 {story_idx+1} 末行: {story[-1]}")
        
        # 打印每个故事的第一行，帮助调试
        for i, story in enumerate(stories):
            if story:
                logger.info(f"故事 {i+1} 首行: {story[0][:50]}{'...' if len(story[0]) > 50 else ''}")
        
        logger.info(f"共检测到 {len(stories)} 个故事")
        
        # 处理每个故事的对白
        for story_idx, story_lines in enumerate(stories):
            logger.info(f"开始处理故事 {story_idx+1}, 共 {len(story_lines)} 行对白")
            
            for i, line in enumerate(story_lines):
                # 提取角色名和对白内容
                match = re.match(r'^([A-Za-z\u00e9\u00c9\s]+)\s*:\s*(.+)$', line)
                
                # 如果匹配不到英文角色名，尝试匹配中文角色名
                if not match:
                    match = re.match(r'^([\u4e00-\u9fa5]+)\s*[:|：]\s*(.+)$', line)
                    
                # 如果还是匹配不到，尝试更宽松的匹配方式，包括支持Coach Rock:等复合名称
                if not match:
                    match = re.match(r'^([A-Za-z\u00e9\u00c9\s\-0-9]+)[:|：]\s*(.+)$', line)
                
                if match:
                    character = match.group(1).strip()
                    dialogue = match.group(2).strip()
                    logger.info(f"提取到角色: '{character}', 对白: '{dialogue}'")
                    
                    # 删除括号及括号内的内容
                    dialogue = re.sub(r'\([^)]*\)', '', dialogue).strip()
                    
                    # 根据角色名选择语音
                    voice_name = "en-US-JasonNeural"  # 默认语音
                    char_lower = character.lower()
                    
                    # 特别处理Jack Black角色 - 使用更宽松的匹配方式
                    if ("jack" in char_lower and "black" in char_lower) or "jackblack" in char_lower.replace(" ", "") or char_lower == "jack":
                        voice_name = "en-US-DerekMultilingualNeural"
                        logger.info(f"检测到Jack Black角色: '{character}', 使用语音: {voice_name}")
                    elif "messi" in char_lower:
                        voice_name = "en-US-JasonNeural"
                    elif "ronaldo" in char_lower:
                        voice_name = "en-US-GuyNeural"
                    elif "neymar" in char_lower:
                        voice_name = "en-US-TonyNeural"
                    elif "mbapp" in char_lower or "mbappe" in char_lower:
                        voice_name = "en-US-DavisNeural"
                    elif char_lower == "rock" or "rock:" in line.lower() or "rock" in char_lower or "johnson" in char_lower or "dwayne" in char_lower or "therock" in char_lower.replace(" ", "") or "coach" in char_lower or "coach rock" in char_lower:
                        voice_name = "en-US-DerekMultilingualNeural"
                        logger.info(f"检测到Rock Johnson或Coach角色: '{character}', 使用语音: {voice_name}")
                    # 新增角色识别逻辑
                    elif ("gwen" in char_lower and "stacy" in char_lower) or "gwenstacy" in char_lower.replace(" ", "") or char_lower == "gwen":
                        voice_name = "en-US-AvaNeural"
                        logger.info(f"检测到Gwen Stacy角色: '{character}', 使用语音: {voice_name}")
                    elif ("cat" in char_lower and "woman" in char_lower) or "catwoman" in char_lower.replace(" ", ""):
                        voice_name = "en-US-JennyNeural" # 修改为en-US-JennyNeural
                        logger.info(f"检测到Catwoman角色: '{character}', 使用语音: {voice_name}")
                    elif ("wonder" in char_lower and "woman" in char_lower) or "wonderwoman" in char_lower.replace(" ", "") or char_lower == "diana" or char_lower == "wonder":
                        voice_name = "en-US-EmmaNeural"
                        logger.info(f"检测到Wonder Woman角色: '{character}', 使用语音: {voice_name}")
                    elif ("spider" in char_lower and "man" in char_lower) or "spiderman" in char_lower.replace(" ", "") or ("peter" in char_lower and "parker" in char_lower):
                        voice_name = "en-US-AndrewNeural"
                        logger.info(f"检测到Spider-Man角色: '{character}', 使用语音: {voice_name}")
                    elif char_lower == "thor" or ("god" in char_lower and "thunder" in char_lower):
                        voice_name = "en-US-SteffanNeural"
                        logger.info(f"检测到Thor角色: '{character}', 使用语音: {voice_name}")
                    elif char_lower == "loki" or ("god" in char_lower and "mischief" in char_lower):
                        voice_name = "en-US-BrianNeural"
                        logger.info(f"检测到Loki角色: '{character}', 使用语音: {voice_name}")
                    # 新增的11个角色识别逻辑
                    elif ("black" in char_lower and "widow" in char_lower) or "blackwidow" in char_lower.replace(" ", "") or char_lower == "natasha":
                        voice_name = "en-US-AriaNeural"
                        logger.info(f"检测到Black Widow角色: '{character}', 使用语音: {voice_name}")
                    elif ("scarlet" in char_lower and "witch" in char_lower) or "scarletwitch" in char_lower.replace(" ", "") or char_lower == "wanda":
                        voice_name = "en-US-JennyNeural"
                        logger.info(f"检测到Scarlet Witch角色: '{character}', 使用语音: {voice_name}")
                    elif ("captain" in char_lower and "marvel" in char_lower) or "captainmarvel" in char_lower.replace(" ", "") or char_lower == "carol":
                        voice_name = "en-US-EmmaNeural"
                        logger.info(f"检测到Captain Marvel角色: '{character}', 使用语音: {voice_name}")
                    elif ("iron" in char_lower and "man" in char_lower) or "ironman" in char_lower.replace(" ", "") or char_lower == "tony" or char_lower == "stark":
                        voice_name = "en-US-GuyNeural"
                        logger.info(f"检测到Iron Man角色: '{character}', 使用语音: {voice_name}")
                    elif ("captain" in char_lower and "america" in char_lower) or "captainamerica" in char_lower.replace(" ", "") or char_lower == "steve" or char_lower == "rogers":
                        voice_name = "en-US-SteffanNeural"
                        logger.info(f"检测到Captain America角色: '{character}', 使用语音: {voice_name}")
                    elif "supergirl" in char_lower or char_lower == "kara":
                        voice_name = "en-US-AvaNeural"
                        logger.info(f"检测到Supergirl角色: '{character}', 使用语音: {voice_name}")
                    elif ("harley" in char_lower and "quinn" in char_lower) or "harleyquinn" in char_lower.replace(" ", "") or char_lower == "harley":
                        voice_name = "en-US-MichelleNeural"
                        logger.info(f"检测到Harley Quinn角色: '{character}', 使用语音: {voice_name}")
                    elif "superman" in char_lower or char_lower == "clark" or char_lower == "kent":
                        voice_name = "en-US-JasonNeural"
                        logger.info(f"检测到Superman角色: '{character}', 使用语音: {voice_name}")
                    elif "batman" in char_lower or char_lower == "bruce" or char_lower == "wayne":
                        voice_name = "en-US-DavisNeural"
                        logger.info(f"检测到Batman角色: '{character}', 使用语音: {voice_name}")
                    elif ("the" in char_lower and "flash" in char_lower) or "flash" in char_lower or char_lower == "barry" or char_lower == "allen":
                        voice_name = "en-US-AndrewNeural"
                        logger.info(f"检测到The Flash角色: '{character}', 使用语音: {voice_name}")
                    elif "aquaman" in char_lower or char_lower == "arthur" or char_lower == "curry":
                        voice_name = "en-US-BrianNeural"
                        logger.info(f"检测到Aquaman角色: '{character}', 使用语音: {voice_name}")
                    
                    # 生成文件名 - 新的命名规则: 故事编号-对白编号 (例如: 1-1, 1-2, 2-1, 2-2...)
                    story_num = story_idx + 1  # 故事编号从1开始
                    dialogue_num = i + 1      # 对白编号从1开始
                    output_file = os.path.join(output_dir, f"{story_num}-{dialogue_num}.mp3")
                    
                    # 显示要生成的实际对白内容
                    logger.info(f"为角色 {character} 生成语音: {dialogue[:25]}...")
                    
                    # 生成语音，添加重试机制
                    max_retries = 3  # 最大重试次数
                    retry_count = 0
                    success = False
                    
                    while retry_count < max_retries and not success:
                        try:
                            if retry_count > 0:
                                logger.info(f"第 {retry_count} 次重试为角色 {character} 生成语音...")
                            logger.info(f"尝试为角色 {character} 生成语音，使用语音: {voice_name}，对白内容: {dialogue[:50]}...")
                            
                            # 根据角色选择不同的语音风格
                            voice_style = None  # 默认没有特殊风格
                            
                            # 使用配置中的参数
                            char_configs = {}
                            for key in VOICE_CONFIG["characters"]:
                                # 特别处理包含空格的角色名，如"dwayne johnson"
                                if key == "dwayne johnson" and "dwayne" in char_lower and "johnson" in char_lower:
                                    char_configs = VOICE_CONFIG["characters"][key]
                                    voice_name = char_configs.get("voice", voice_name)
                                    voice_style = char_configs.get("style", None)
                                    should_keep_silence = char_configs.get("keep_trailing_silence", False)
                                    should_add_space = char_configs.get("add_space", False)
                                    break
                                # 特别处理Wonder Woman的简写形式
                                elif key == "wonder woman" and char_lower == "wonder":
                                    char_configs = VOICE_CONFIG["characters"][key]
                                    voice_name = char_configs.get("voice", voice_name)
                                    voice_style = char_configs.get("style", None)
                                    should_keep_silence = char_configs.get("keep_trailing_silence", False)
                                    should_add_space = char_configs.get("add_space", False)
                                    break
                                # 特别处理Gwen Stacy的简写形式
                                elif key == "gwen stacy" and char_lower == "gwen":
                                    char_configs = VOICE_CONFIG["characters"][key]
                                    voice_name = char_configs.get("voice", voice_name)
                                    voice_style = char_configs.get("style", None)
                                    should_keep_silence = char_configs.get("keep_trailing_silence", False)
                                    should_add_space = char_configs.get("add_space", False)
                                    break
                                elif key in char_lower:
                                    char_configs = VOICE_CONFIG["characters"][key]
                                    voice_name = char_configs.get("voice", voice_name)
                                    voice_style = char_configs.get("style", None)
                                    should_keep_silence = char_configs.get("keep_trailing_silence", False)
                                    should_add_space = char_configs.get("add_space", False)
                                    break
                                    
                            # 全局设置
                            speech_rate = VOICE_CONFIG["global"]["speech_rate"]
                            
                            # 检查全局尾部静音设置
                            global_trailing_silence = VOICE_CONFIG["global"].get("enable_global_trailing_silence", False)
                            
                            # 如果需要在对白末尾添加空格，确保尾音不被截断
                            if char_configs.get("add_space", False):
                                # 对于Mbappé，只在整个对白的最后一句话添加空格
                                # 检测对白中的句子分隔符
                                sentences = re.split(r'[.!?]+', dialogue.strip())
                                # 过滤掉空字符串
                                sentences = [s.strip() for s in sentences if s.strip()]
                                
                                if len(sentences) > 0:
                                    # 重新构建对白，只在最后一句添加空格
                                    # 先找到原始对白中的所有句子分隔符
                                    sentence_endings = re.findall(r'[.!?]+', dialogue)
                                    
                                    # 重新组合对白
                                    rebuilt_dialogue = ""
                                    for i, sentence in enumerate(sentences):
                                        rebuilt_dialogue += sentence
                                        if i < len(sentence_endings):
                                            rebuilt_dialogue += sentence_endings[i]
                                        # 只在最后一句话后添加空格
                                        if i == len(sentences) - 1:
                                            rebuilt_dialogue += " "
                                    
                                    dialogue = rebuilt_dialogue
                                    logger.info(f"为{character}角色在最后一句话后增加尾部空间，保留尾音")
                                else:
                                    # 如果没有检测到句子分隔符，直接在末尾添加空格
                                    dialogue = dialogue + " "
                                    logger.info(f"为{character}角色在对白末尾增加尾部空间，保留尾音")
                            
                            # 记录使用的语音参数
                            logger.info(f"为角色 {character} 使用语音配置:")
                            logger.info(f"  - 语音: {voice_name}")
                            logger.info(f"  - 语速: {speech_rate}")
                            logger.info(f"  - 风格: {voice_style if voice_style else '默认风格'}")
                            logger.info(f"  - 保留尾部静音: {char_configs.get('keep_trailing_silence', False) or global_trailing_silence}")
                            if global_trailing_silence:
                                logger.info("  - 全局尾部静音已启用: 所有角色都会添加0.2秒尾部静音")
                            
                            success = tts_generator.generate_speech(
                                text=dialogue,
                                output_file=output_file,
                                voice_name=voice_name,
                                speech_rate=speech_rate,  # 使用配置中的语速
                                style=voice_style,  # 使用配置中的风格
                                keep_trailing_silence=char_configs.get("keep_trailing_silence", False) or global_trailing_silence  # 使用角色配置或全局配置
                            )
                            
                            if not success:
                                logger.error("生成语音失败，但未抛出异常")
                                retry_count += 1
                                if retry_count < max_retries:
                                    logger.info("将在 3 秒后重试...")
                                    time.sleep(3)  # 重试前等待一段时间
                        except Exception as e:
                            logger.error(f"生成语音时发生异常: {str(e)}")
                            logger.error(f"异常详情: {traceback.format_exc()}")
                            retry_count += 1
                            if retry_count < max_retries:
                                logger.info("将在 3 秒后重试...")
                                time.sleep(3)  # 重试前等待一段时间
                    
                    if success:
                        audio_files.append(output_file)
                        successful_count += 1
                        logger.info(f"已生成故事 {story_idx+1} 的第 {i+1} 个音频文件，命名为 {story_num}-{dialogue_num}.mp3")
                    else:
                        logger.error(f"生成故事 {story_idx+1} 的第 {i+1} 个音频文件失败，文件名 {story_num}-{dialogue_num}.mp3，已尝试 {max_retries} 次重试")
                    
                    # 添加延迟，避免API限制
                    time.sleep(0.5)
        
        if audio_files:
            logger.info(f"成功生成 {len(audio_files)} 个语音文件")
            return True, audio_files
        else:
            logger.error("未能生成任何语音文件")
            return False, []
    except Exception as e:
        logger.error(f"处理对白生成语音时发生错误: {str(e)}")
        return False, []

async def main():
    # 设置信号处理器以捕获Ctrl+C
    signal.signal(signal.SIGINT, signal_handler)
    
    # 防止系统睡眠
    caffeinate_process = prevent_sleep()
    
    # 记录开始时间
    start_time = time.time()
    
    try:
        # 选择运行模式
        mode = select_mode()
        
        if mode == "brainrot":
            # Brainrot模式处理流程
            print("\n=== Brainrot模式 ===")
            
            # 找到brainrot.txt文件
            file_path = find_brainrot_file()
            if not file_path:
                logger.error("无法找到brainrot.txt文件")
                restore_sleep(caffeinate_process)
                return
            
            # 从文件中提取对话内容
            dialogues = extract_brainrot_dialogues(file_path)
            
            if not dialogues:
                logger.error("无法从文件中提取对话内容")
                restore_sleep(caffeinate_process)
                return
            
            print(f"文档中共有 {len(dialogues)} 段对话")
            
            # 处理语音生成
            print("\n=== 开始处理Brainrot语音生成 ===")
            audio_start_time = time.time()
            success, audio_files = process_brainrot_audio_generation(dialogues)
            audio_end_time = time.time()
            
            if success:
                processed_audio_count = len(audio_files)
                print(f"Brainrot语音生成完成！共生成 {processed_audio_count} 个语音文件，耗时 {audio_end_time - audio_start_time:.2f} 秒")
            else:
                print("Brainrot语音生成失败")
                
        else:
            # 默认模式处理流程
            print("\n=== 默认模式 ===")
            
            # 找到文本文件
            file_path = find_word_document()
            if not file_path:
                logger.error("无法找到文本文件")
                restore_sleep(caffeinate_process)
                return
            
            # 从文本文件中提取英文对白
            dialogue_text = extract_dialogue_from_word(file_path)
            
            if not dialogue_text:
                logger.error("无法从文件中提取英文对白")
                restore_sleep(caffeinate_process)
                return
            
            dialogue_lines = dialogue_text.split('\n') if dialogue_text else []
            dialogue_count = len(dialogue_lines)
            
            print(f"文档中共有 {dialogue_count} 个对白")
            
            # 处理语音生成
            print("\n=== 开始处理语音生成 ===")
            audio_start_time = time.time()
            success, audio_files = process_audio_generation(dialogue_text)
            audio_end_time = time.time()
            
            if success:
                processed_audio_count = len(audio_files)
                print(f"语音生成完成！共生成 {processed_audio_count} 个语音文件，耗时 {audio_end_time - audio_start_time:.2f} 秒")
            else:
                print("语音生成失败")
                
        # 显示处理总结
        print(f"\n=== {mode.upper()}模式处理总结 ===")
        print(f"总处理时间: {time.time() - start_time:.2f} 秒")
        print(f"语音生成: {len(audio_files) if 'audio_files' in locals() and success else 0} 个文件")
        print(f"=== {mode.upper()}模式处理完成 ===")
        
    except Exception as e:
        logger.error(f"处理过程中发生错误: {str(e)}")
        traceback.print_exc()
    finally:
        # 关闭防止系统睡眠功能
        logger.info("关闭防止系统睡眠功能...")
        restore_sleep(caffeinate_process)
        logger.info("系统可以正常睡眠了")

if __name__ == "__main__":
    # 安装依赖包
    try:
        from docx import Document
    except ImportError:
        print("正在安装python-docx包...")
        subprocess.check_call(["pip", "install", "python-docx"])
        from docx import Document
        
    try:
        import requests
    except ImportError:
        print("正在安装requests包...")
        subprocess.check_call(["pip", "install", "requests"])
        import requests
    
    try:
        import azure.cognitiveservices.speech
    except ImportError:
        print("正在安装azure-cognitiveservices-speech包...")
        subprocess.check_call(["pip", "install", "azure-cognitiveservices-speech"])
    
    try:
        import pydub
    except ImportError:
        print("正在安装pydub包...")
        subprocess.check_call(["pip", "install", "pydub"])
    
    # 运行主程序
    asyncio.run(main())
```
