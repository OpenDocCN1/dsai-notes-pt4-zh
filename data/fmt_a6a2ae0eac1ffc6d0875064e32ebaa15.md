
```
import os
import json
import uuid
import random
import base64
from glob import glob
from datetime import datetime

# 最小图片展示时间（秒）
MIN_IMAGE_DURATION = 5  # 5秒

def get_latest_draft_folder():
    """
    获取最新的剪映草稿文件夹
    
    Returns:
        最新草稿文件夹的路径
    """
    # 剪映草稿文件夹路径模式（根据实际位置调整）
    draft_pattern = os.path.expanduser("~/Desktop/Youtube/剪映draft/JianyingPro Drafts/*/draft_content.json")
    
    # 获取所有草稿文件
    draft_files = glob(draft_pattern)
    
    if not draft_files:
        raise Exception("未找到剪映草稿文件")
    
    # 按修改时间排序，获取最新的
    latest_draft_file = max(draft_files, key=os.path.getmtime)
    
    # 返回包含draft_content.json的文件夹路径
    latest_draft_folder = os.path.dirname(latest_draft_file)
    
    return latest_draft_folder

def microsec_to_time(microseconds):
    """将微秒转换为时间字符串(MM:SS:MS)"""
    # 剪映中的时间单位是微秒
    total_seconds = microseconds / 1000000
    minutes = int(total_seconds // 60)
    seconds = int(total_seconds % 60)
    ms = int((total_seconds % 1) * 1000)
    return f"{minutes:02d}:{seconds:02d}:{ms:03d}"

def format_duration(microseconds):
    """格式化持续时间为秒"""
    return f"{microseconds/1000000:.2f}秒"

def find_images_in_folder(folder_path=None):
    """
    在指定文件夹中查找图片文件
    
    Args:
        folder_path: 图片文件夹路径，如果为None，则使用默认路径
    
    Returns:
        图片文件路径列表
    """
    if folder_path is None:
        # 默认在桌面上查找图片文件夹
        folder_path = os.path.expanduser("~/Desktop/Youtube/images")
    
    # 支持的图片格式
    image_extensions = ['jpg', 'jpeg', 'png', 'gif', 'webp']
    
    # 查找所有图片文件
    image_files = []
    for ext in image_extensions:
        pattern = os.path.join(folder_path, f"*.{ext}")
        image_files.extend(glob(pattern))
    
    if not image_files:
        # 如果没有找到图片，尝试在当前目录查找
        current_folder = os.path.dirname(os.path.abspath(__file__))
        for ext in image_extensions:
            pattern = os.path.join(current_folder, f"*.{ext}")
            image_files.extend(glob(pattern))
    
    if not image_files:
        raise Exception(f"未找到图片文件，请确保图片文件夹中包含图片")
    
    return image_files

def import_images_to_draft(draft, image_files):
    """
    将图片导入到草稿中
    
    Args:
        draft: 剪映草稿JSON对象
        image_files: 图片文件路径列表
    
    Returns:
        更新后的草稿JSON对象，以及导入的图片材料列表
    """
    # 确保materials中有videos键
    if 'materials' not in draft:
        draft['materials'] = {}
    
    if 'videos' not in draft['materials']:
        draft['materials']['videos'] = []
    
    # 导入图片
    for image_file in image_files:
        # 生成唯一ID
        image_id = str(uuid.uuid4())
        
        # 获取文件名（不包含扩展名）
        file_name = os.path.basename(image_file)
        name, _ = os.path.splitext(file_name)
        
        # 创建图片材料（作为视频材料存储）
        image_material = {
            "aigc_type": "none",
            "audio_fade": None,
            "cartoon_path": "",
            "category_id": "",
            "category_name": "",
            "check_flag": 63487,
            "crop": {
                "lower_left_x": 0.0,
                "lower_left_y": 1.0,
                "lower_right_x": 1.0,
                "lower_right_y": 1.0,
                "upper_left_x": 0.0,
                "upper_left_y": 0.0,
                "upper_right_x": 1.0,
                "upper_right_y": 0.0
            },
            "crop_ratio": "free",
            "crop_scale": 1.0,
            "duration": 10800000000,
            "extra_type_option": 0,
            "formula_id": "",
            "freeze": None,
            "has_audio": False,
            "height": 1080,
            "id": image_id,
            "intensifies_audio_path": "",
            "intensifies_path": "",
            "is_ai_generate_content": False,
            "is_copyright": False,
            "is_text_edit_overdub": False,
            "is_unified_beauty_mode": False,
            "local_id": "",
            "local_material_id": "",
            "material_id": "",
            "material_name": file_name,
            "material_url": "",
            "matting": {
                "flag": 0,
                "has_use_quick_brush": False,
                "has_use_quick_eraser": False,
                "interactiveTime": [],
                "path": "",
                "strokes": []
            },
            "media_path": "",
            "object_locked": None,
            "origin_material_id": "",
            "path": image_file,
            "picture_from": "none",
            "picture_set_category_id": "",
            "picture_set_category_name": "",
            "request_id": "",
            "reverse_intensifies_path": "",
            "reverse_path": "",
            "smart_motion": None,
            "source": 0,
            "source_platform": 0,
            "stable": {
                "matrix_path": "",
                "stable_level": 0,
                "time_range": {
                    "duration": 0,
                    "start": 0
                }
            },
            "team_id": "",
            "type": "photo",
            "video_algorithm": {
                "algorithms": [],
                "complement_frame_config": None,
                "deflicker": None,
                "gameplay_configs": [],
                "motion_blur_config": None,
                "noise_reduction": None,
                "path": "",
                "quality_enhance": None,
                "time_range": None
            },
            "width": 1920
        }
        
        # 添加到草稿中
        draft['materials']['videos'].append(image_material)
    
    print(f"已导入 {len(image_files)} 个图片到草稿")
    return draft

def create_common_keyframes(start_time, duration, movement_type=None):
    """创建关键帧数据"""
    if movement_type is None:
        movement_type = random.choice(['left', 'right', 'up', 'down'])
    
    if movement_type in ['left', 'right']:
        # X轴移动，Y轴保持0
        x_start = -0.21 if movement_type == 'left' else 0.21
        x_end = -x_start  # 反向移动
        return [
            {
                "id": str(uuid.uuid4()),
                "keyframe_list": [
                    {"time_offset": 0, "values": [x_start]},
                    {"time_offset": duration, "values": [x_end]}
                ],
                "property_type": "KFTypePositionX"
            },
            {
                "id": str(uuid.uuid4()),
                "keyframe_list": [
                    {"time_offset": 0, "values": [0]},
                    {"time_offset": duration, "values": [0]}
                ],
                "property_type": "KFTypePositionY"
            }
        ]
    else:
        # Y轴移动，X轴保持0
        y_start = -0.21 if movement_type == 'up' else 0.21
        y_end = -y_start  # 反向移动
        return [
            {
                "id": str(uuid.uuid4()),
                "keyframe_list": [
                    {"time_offset": 0, "values": [0]},
                    {"time_offset": duration, "values": [0]}
                ],
                "property_type": "KFTypePositionX"
            },
            {
                "id": str(uuid.uuid4()),
                "keyframe_list": [
                    {"time_offset": 0, "values": [y_start]},
                    {"time_offset": duration, "values": [y_end]}
                ],
                "property_type": "KFTypePositionY"
            }
        ]

def create_fade_animation():
    """创建渐显动画"""
    animation_id = str(uuid.uuid4())
    request_id = datetime.now().strftime("%Y%m%d%H%M%S") + str(uuid.uuid4())[:8].upper()
    return {
        "animations": [
            {
                "anim_adjust_params": None,
                "category_id": "in",
                "category_name": "入场",
                "duration": 700000,  # 0.7秒
                "id": "624705",
                "material_type": "video",
                "name": "渐显",
                "panel": "video",
                "path": "/Users/a47/Library/Containers/com.lemon.lvpro/Data/Movies/JianyingPro/User Data/Cache/effect/624705/00162fa64b6f03d941fea852a0054bc7",
                "platform": "all",
                "request_id": request_id,
                "resource_id": "6798320778182922760",
                "start": 0,
                "type": "in"
            }
        ],
        "id": animation_id,
        "type": "sticker_animation"
    }

def create_scale_animation():
    """创建缩放动画（向右甩入）"""
    animation_id = str(uuid.uuid4())
    request_id = datetime.now().strftime("%Y%m%d%H%M%S") + str(uuid.uuid4())[:8].upper()
    return {
        "animations": [
            {
                "anim_adjust_params": None,
                "category_id": "in",
                "category_name": "入场",
                "duration": 700000,
                "id": "431636",
                "material_type": "video",
                "name": "向右甩入",
                "panel": "video",
                "path": "/Users/a47/Library/Containers/com.lemon.lvpro/Data/Movies/JianyingPro/User Data/Cache/effect/431636/c83f7d144853d115a9e8572e667c6bfe",
                "platform": "all",
                "request_id": request_id,
                "resource_id": "6739338727866241539",
                "start": 0,
                "type": "in"
            }
        ],
        "id": animation_id,
        "multi_language_current": "none",
        "type": "sticker_animation"
    }

def create_bounce_animation():
    """创建弹跳动画（向左滑动）"""
    animation_id = str(uuid.uuid4())
    request_id = datetime.now().strftime("%Y%m%d%H%M%S") + str(uuid.uuid4())[:8].upper()
    return {
        "animations": [
            {
                "anim_adjust_params": None,
                "category_id": "in",
                "category_name": "入场",
                "duration": 700000,
                "id": "624747",
                "material_type": "video",
                "name": "向左滑动",
                "panel": "video",
                "path": "/Users/a47/Library/Containers/com.lemon.lvpro/Data/Movies/JianyingPro/User Data/Cache/effect/624747/4f9fea978322067343df1b0fdb80798c",
                "platform": "all",
                "request_id": request_id,
                "resource_id": "6798332871267324423",
                "start": 0,
                "type": "in"
            }
        ],
        "id": animation_id,
        "multi_language_current": "none",
        "type": "sticker_animation"
    }

def create_wiper_animation():
    """创建雨刷动画"""
    animation_id = str(uuid.uuid4())
    request_id = datetime.now().strftime("%Y%m%d%H%M%S") + str(uuid.uuid4())[:8].upper()
    return {
        "animations": [
            {
                "anim_adjust_params": None,
                "category_id": "in",
                "category_name": "入场",
                "duration": 700000,
                "id": "640101",
                "material_type": "video",
                "name": "雨刷 II",
                "panel": "video",
                "path": "/Users/a47/Library/Containers/com.lemon.lvpro/Data/Movies/JianyingPro/User Data/Cache/effect/640101/1241565fd010e8221475def8ab1a2f57",
                "platform": "all",
                "request_id": request_id,
                "resource_id": "6805748897768542727",
                "start": 0,
                "type": "in"
            }
        ],
        "id": animation_id,
        "multi_language_current": "none",
        "type": "sticker_animation"
    }

def create_shake_animation():
    """创建抖动动画"""
    animation_id = str(uuid.uuid4())
    request_id = datetime.now().strftime("%Y%m%d%H%M%S") + str(uuid.uuid4())[:8].upper()
    return {
        "animations": [
            {
                "anim_adjust_params": None,
                "category_id": "in",
                "category_name": "入场",
                "duration": 700000,
                "id": "431654",
                "material_type": "video",
                "name": "左右抖动",
                "panel": "video",
                "path": "/Users/a47/Library/Containers/com.lemon.lvpro/Data/Movies/JianyingPro/User Data/Cache/effect/431654/267653b22765bd8348dda092f8de3cfe",
                "platform": "all",
                "request_id": request_id,
                "resource_id": "6739418540421419524",
                "start": 0,
                "type": "in"
            }
        ],
        "id": animation_id,
        "multi_language_current": "none",
        "type": "sticker_animation"
    }

def create_zoom_in_animation():
    """创建动感放大动画"""
    animation_id = str(uuid.uuid4())
    request_id = datetime.now().strftime("%Y%m%d%H%M%S") + str(uuid.uuid4())[:8].upper()
    return {
        "animations": [
            {
                "anim_adjust_params": None,
                "category_id": "in",
                "category_name": "入场",
                "duration": 500000,
                "id": "431662",
                "material_type": "video",
                "name": "动感放大",
                "panel": "video",
                "path": "/Users/a47/Library/Containers/com.lemon.lvpro/Data/Movies/JianyingPro/User Data/Cache/effect/431662/8fb560c01e4ccffbc4dc084f9c418838",
                "platform": "all",
                "request_id": request_id,
                "resource_id": "6740867832570974733",
                "start": 0,
                "type": "in"
            }
        ],
        "id": animation_id,
        "multi_language_current": "none",
        "type": "sticker_animation"
    }

def create_zoom_out_animation():
    """创建缩小动画"""
    animation_id = str(uuid.uuid4())
    request_id = datetime.now().strftime("%Y%m%d%H%M%S") + str(uuid.uuid4())[:8].upper()
    return {
        "animations": [
            {
                "anim_adjust_params": None,
                "category_id": "in",
                "category_name": "入场",
                "duration": 500000,
                "id": "624755",
                "material_type": "video",
                "name": "缩小",
                "panel": "video",
                "path": "/Users/a47/Library/Containers/com.lemon.lvpro/Data/Movies/JianyingPro/User Data/Cache/effect/624755/7cc60a6c2411194697a2b6e923844bca",
                "platform": "all",
                "request_id": request_id,
                "resource_id": "6798332584276267527",
                "start": 0,
                "type": "in"
            }
        ],
        "id": animation_id,
        "multi_language_current": "none",
        "type": "sticker_animation"
    }

def create_gentle_shake_animation():
    """创建轻微抖动动画"""
    animation_id = str(uuid.uuid4())
    request_id = datetime.now().strftime("%Y%m%d%H%M%S") + str(uuid.uuid4())[:8].upper()
    return {
        "animations": [
            {
                "anim_adjust_params": None,
                "category_id": "in",
                "category_name": "入场",
                "duration": 500000,
                "id": "431664",
                "material_type": "video",
                "name": "轻微抖动",
                "panel": "video",
                "path": "/Users/a47/Library/Containers/com.lemon.lvpro/Data/Movies/JianyingPro/User Data/Cache/effect/431664/944c5561f3d23baa068cee2bba4f15f5",
                "platform": "all",
                "request_id": request_id,
                "resource_id": "6739418227031413256",
                "start": 0,
                "type": "in"
            }
        ],
        "id": animation_id,
        "multi_language_current": "none",
        "type": "sticker_animation"
    }

def create_pendulum_animation():
    """创建钟摆动画"""
    animation_id = str(uuid.uuid4())
    request_id = datetime.now().strftime("%Y%m%d%H%M%S") + str(uuid.uuid4())[:8].upper()
    return {
        "animations": [
            {
                "anim_adjust_params": None,
                "category_id": "in",
                "category_name": "入场",
                "duration": 500000,
                "id": "636115",
                "material_type": "video",
                "name": "钟摆",
                "panel": "video",
                "path": "/Users/a47/Library/Containers/com.lemon.lvpro/Data/Movies/JianyingPro/User Data/Cache/effect/636115/fb435c67390b89386c35e8b84f50faba",
                "platform": "all",
                "request_id": request_id,
                "resource_id": "6803260897117606414",
                "start": 0,
                "type": "in"
            }
        ],
        "id": animation_id,
        "multi_language_current": "none",
        "type": "sticker_animation"
    }

def create_flip_in_animation():
    """创建翻入动画"""
    animation_id = str(uuid.uuid4())
    request_id = datetime.now().strftime("%Y%m%d%H%M%S") + str(uuid.uuid4())[:8].upper()
    return {
        "animations": [
            {
                "anim_adjust_params": None,
                "category_id": "in",
                "category_name": "入场",
                "duration": 1170000,
                "id": "98430529",
                "material_type": "video",
                "name": "翻入",
                "panel": "video",
                "path": "/Users/a47/Library/Containers/com.lemon.lvpro/Data/Movies/JianyingPro/User Data/Cache/effect/98430529/bd0a96cbfbb58eb67c5f4e86f537b858",
                "platform": "all",
                "request_id": request_id,
                "resource_id": "7452407076417966619",
                "start": 0,
                "type": "in"
            }
        ],
        "id": animation_id,
        "multi_language_current": "none",
        "type": "sticker_animation"
    }

# 全局变量，用于记录上一次使用的动画
last_animation = None

def get_random_animation():
    """随机选择一个动画效果，确保不会连续使用相同的动画"""
    global last_animation
    
    # 所有可用的动画效果函数列表
    animations = [
        create_fade_animation,
        create_scale_animation,
        create_bounce_animation,
        create_wiper_animation,
        create_shake_animation,
        create_zoom_in_animation,
        create_zoom_out_animation,
        create_gentle_shake_animation,
        create_pendulum_animation,
        create_flip_in_animation
    ]
    
    # 从列表中移除上一次使用的动画
    if last_animation and last_animation in animations:
        available_animations = [anim for anim in animations if anim != last_animation]
    else:
        available_animations = animations
    
    # 随机选择一个动画
    chosen_animation = random.choice(available_animations)
    last_animation = chosen_animation
    
    return chosen_animation()

def find_next_subtitle_time(subtitle_segments, current_start_time):
    """
    找到当前图片应该结束的时间点:
    1. 先确定最早可能结束的时间点 = 当前开始时间 + 5秒
    2. 然后找到在这个时间点之后的第一个字幕开始时间
    
    Args:
        subtitle_segments: 字幕片段列表
        current_start_time: 当前图片的开始时间（微秒）
        
    Returns:
        下一个字幕的开始时间，或者当前开始时间+5秒
    """
    min_possible_end = current_start_time + 5000000  # 当前图片最早可能结束的时间(5秒)
    
    print(f"当前图片开始时间: {microsec_to_time(current_start_time)}")
    print(f"当前图片最早可能结束时间(+5秒): {microsec_to_time(min_possible_end)}")
    
    # 找到第一个开始时间大于min_possible_end的字幕
    for subtitle in subtitle_segments:
        subtitle_start = subtitle['target_timerange']['start']
        if subtitle_start > min_possible_end:  # 只关心字幕的开始时间
            print(f"找到下一个字幕时间: {microsec_to_time(subtitle_start)}")
            return subtitle_start
    
    # 如果没找到合适的字幕,就用最小结束时间
    print(f"没找到更晚的字幕,使用最小结束时间: {microsec_to_time(min_possible_end)}")
    return min_possible_end

def add_effects_to_segment(segment):
    """
    为片段添加特效
    
    Args:
        segment: 片段对象
    
    Returns:
        更新后的片段对象
    """
    # 添加一些基本特效
    effects = [
        # 可以在这里添加其他特效ID
        "E312DFCC-91BF-47D1-831A-999CE06AF820",  # 速度特效
        "BAE4FFEE-2A02-4D91-9C10-1CFA0B5A9FA7"   # 画布特效
    ]
    
    # 添加特效引用
    segment["extra_material_refs"] = effects
    
    return segment

def create_star_effect():
    """创建星火特效"""
    effect_id = str(uuid.uuid4())
    return {
        "adjust_params": [
            {"default_value": 0.33, "name": "effects_adjust_speed", "value": 0.33},
            {"default_value": 1.0, "name": "effects_adjust_background_animation", "value": 1.0}
        ],
        "algorithm_artifact_path": "",
        "apply_target_type": 2,
        "apply_time_range": None,
        "category_id": "39654",
        "category_name": "热门",
        "common_keyframes": [],
        "disable_effect_faces": [],
        "effect_id": "634103",
        "formula_id": "",
        "id": effect_id,
        "name": "星火",
        "path": "/Users/a47/Library/Containers/com.lemon.lvpro/Data/Movies/JianyingPro/User Data/Cache/effect/634103/c2811a4bdeb4394ae95f3aa9875dc4dc",
        "platform": "all",
        "render_index": 0,
        "request_id": "",
        "resource_id": "6715209198109463054",
        "source_platform": 0,
        "time_range": None,
        "track_render_index": 0,
        "type": "video_effect",
        "value": 1.0,
        "version": ""
    }

def create_firefly_effect():
    """创建萤火特效"""
    effect_id = str(uuid.uuid4())
    return {
        "adjust_params": [
            {"default_value": 0.33, "name": "effects_adjust_speed", "value": 0.33},
            {"default_value": 1.0, "name": "effects_adjust_background_animation", "value": 1.0}
        ],
        "algorithm_artifact_path": "",
        "apply_target_type": 2,
        "apply_time_range": None,
        "category_id": "39654",
        "category_name": "热门",
        "common_keyframes": [],
        "disable_effect_faces": [],
        "effect_id": "1357502",
        "formula_id": "",
        "id": effect_id,
        "name": "萤火",
        "path": "/Users/a47/Library/Containers/com.lemon.lvpro/Data/Movies/JianyingPro/User Data/Cache/effect/1357502/d473ca7fa127312a7651ec1523a6e880",
        "platform": "all",
        "render_index": 0,
        "request_id": "",
        "resource_id": "7006265184050221576",
        "source_platform": 0,
        "time_range": None,
        "track_render_index": 0,
        "type": "video_effect",
        "value": 1.0,
        "version": ""
    }

def create_gold_dust_effect():
    """创建金粉闪闪特效"""
    effect_id = str(uuid.uuid4())
    request_id = datetime.now().strftime("%Y%m%d%H%M%S") + str(uuid.uuid4())[:8].upper()
    return {
        "adjust_params": [
            {"default_value": 0.3361999988555908, "name": "effects_adjust_speed", "value": 0.3361999988555908},
            {"default_value": 0.5, "name": "effects_adjust_filter", "value": 0.5},
            {"default_value": 1.0, "name": "effects_adjust_background_animation", "value": 1.0}
        ],
        "algorithm_artifact_path": "",
        "apply_target_type": 2,
        "apply_time_range": None,
        "category_id": "107",
        "category_name": "收藏",
        "common_keyframes": [],
        "disable_effect_faces": [],
        "effect_id": "1453820",
        "formula_id": "",
        "id": effect_id,
        "name": "金粉闪闪",
        "path": "/Users/a47/Library/Containers/com.lemon.lvpro/Data/Movies/JianyingPro/User Data/Cache/effect/1453820/a552dfa820b5aba27e4f09e3d83b8643",
        "platform": "all",
        "render_index": 0,
        "request_id": request_id,
        "resource_id": "7034048554318434830",
        "source_platform": 0,
        "time_range": None,
        "track_render_index": 0,
        "type": "video_effect",
        "value": 1.0,
        "version": ""
    }

def add_effect_track(draft, segments):
    """添加特效轨道"""
    # 创建特效轨道
    effect_track = {
        "attribute": 0,
        "flag": 0,
        "id": str(uuid.uuid4()),
        "is_default_name": True,
        "name": "",
        "segments": [],
        "type": "effect"
    }

    # 为每个视频片段创建对应的特效
    for i, segment in enumerate(segments):
        start_time = segment['target_timerange']['start']
        duration = segment['target_timerange']['duration']
        
        # 随机选择一个特效
        effect_funcs = [create_star_effect, create_firefly_effect, create_gold_dust_effect]
        effect = random.choice(effect_funcs)()
        
        # 添加特效到materials
        if 'video_effects' not in draft['materials']:
            draft['materials']['video_effects'] = []
        draft['materials']['video_effects'].append(effect)
        
        # 创建特效片段
        effect_segment = {
            "caption_info": None,
            "cartoon": False,
            "clip": None,
            "common_keyframes": [],
            "enable_adjust": False,
            "enable_color_curves": True,
            "enable_color_match_adjust": False,
            "enable_color_wheels": True,
            "enable_lut": False,
            "enable_smart_color_adjust": False,
            "extra_material_refs": [],
            "group_id": "",
            "id": str(uuid.uuid4()),
            "intensifies_audio": False,
            "is_placeholder": False,
            "is_tone_modify": False,
            "keyframe_refs": [],
            "last_nonzero_volume": 1.0,
            "material_id": effect['id'],
            "render_index": 11000 + i,
            "responsive_layout": {
                "enable": False,
                "horizontal_pos_layout": 0,
                "size_layout": 0,
                "target_follow": "",
                "vertical_pos_layout": 0
            },
            "reverse": False,
            "source_timerange": None,
            "speed": 1.0,
            "target_timerange": {
                "duration": duration,
                "start": start_time
            },
            "template_id": "",
            "template_scene": "default",
            "track_attribute": 0,
            "track_render_index": 1,
            "uniform_scale": None,
            "visible": True,
            "volume": 1.0
        }
        effect_track['segments'].append(effect_segment)
    
    return effect_track

def sync_images_with_subtitles_in_draft(draft):
    """
    在草稿中将图片与字幕同步
    
    Args:
        draft: 剪映草稿JSON对象
    
    Returns:
        更新后的草稿JSON对象
    """
    # 查找文本轨道
    text_track = None
    for track in draft.get('tracks', []):
        if track.get('type') == 'text':
            text_track = track
            print("找到text轨道")
            break
    
    if not text_track:
        print("未找到text轨道，无法同步图片与字幕")
        return draft
    
    # 获取所有字幕片段
    subtitle_segments = text_track.get('segments', [])
    print(f"找到 {len(subtitle_segments)} 个字幕片段")
    
    # 打印字幕时间范围
    print("字幕时间范围:")
    for i, subtitle in enumerate(subtitle_segments):
        subtitle_start = subtitle['target_timerange']['start']
        subtitle_end = subtitle_start + subtitle['target_timerange']['duration']
        subtitle_duration = subtitle['target_timerange']['duration'] / 1000000
        print(f"  字幕 {i}: 开始={microsec_to_time(subtitle_start)}, 结束={microsec_to_time(subtitle_end)}, 持续={subtitle_duration:.2f}秒秒")
    
    # 查找图片轨道
    image_track = None
    for track in draft.get('tracks', []):
        if track.get('type') == 'video':
            # 检查是否有图片材料
            has_image = False
            for segment in track.get('segments', []):
                material_id = segment.get('material_id', '')
                material = None
                for mat in draft.get('materials', {}).get('videos', []):
                    if mat.get('id') == material_id:
                        material = mat
                        break
                
                # 在剪映中，图片类型可能是'photo'或'image'
                if material and (material.get('type') == 'photo' or material.get('type') == 'image'):
                    has_image = True
                    break
            
            if has_image:
                image_track = track
                print("使用现有的图片轨道")
                break
    
    if not image_track:
        # 创建新的图片轨道
        image_track = {
            "attribute": 0,
            "flag": 0,
            "id": str(uuid.uuid4()),
            "is_default_name": True,
            "name": "",
            "segments": [],
            "type": "video"
        }
        draft['tracks'].append(image_track)
        print("创建了新的图片轨道")
    
    # 获取所有图片材料 - 在剪映中，图片类型是'photo'或'image'
    image_materials = []
    for video in draft.get('materials', {}).get('videos', []):
        if video.get('type') == 'photo' or video.get('type') == 'image':
            image_materials.append(video)
    
    print(f"找到 {len(image_materials)} 个图片材料")
    
    # 如果没有找到图片材料，创建一个默认的图片材料
    if not image_materials:
        print("未找到图片材料，将创建默认图片材料")
        # 创建一个默认的图片材料
        default_image_id = str(uuid.uuid4())
        default_image = {
            "duration": 10800000000,  # 使用与剪映草稿中相同的默认持续时间
            "height": 1080,
            "id": default_image_id,
            "local_material_id": default_image_id,
            "material_name": "默认图片",
            "path": "",  # 这里应该填入一个实际的图片路径，但我们暂时留空
            "type": "photo",  # 使用'photo'类型与剪映一致
            "width": 1920
        }
        
        # 添加到materials
        if 'materials' not in draft:
            draft['materials'] = {}
        if 'videos' not in draft['materials']:
            draft['materials']['videos'] = []
        
        draft['materials']['videos'].append(default_image)
        image_materials.append(default_image)
        print("已创建默认图片材料")
    
    if len(image_materials) < len(subtitle_segments):
        print(f"警告: 图片数量({len(image_materials)})少于字幕数量({len(subtitle_segments)})")
    
    # 清空图片轨道的片段
    image_track['segments'] = []
    
    # 确保 materials 中有 material_animations 列表
    if 'materials' not in draft:
        draft['materials'] = {}
    if 'material_animations' not in draft['materials']:
        draft['materials']['material_animations'] = []
    
    # 按照字幕顺序创建图片片段
    last_end_time = 0  # 第一张图片从0开始
    created_segments = []
    
    for i in range(len(subtitle_segments)):
        # 当前图片的开始时间就是上一张图片的结束时间
        start_time = last_end_time
        
        # 找到这张图片应该结束的时间
        # 必须是某个字幕的开始时间,且至少要在start_time+5秒之后
        end_time = find_next_subtitle_time(subtitle_segments, start_time)
        
        print(f"\n当前图片开始时间: {microsec_to_time(start_time)}")
        print(f"当前图片最早可能结束时间(+5秒): {microsec_to_time(start_time + 5000000)}")
        print(f"找到下一个字幕时间: {microsec_to_time(end_time)}")
        
        # 计算持续时间
        duration = end_time - start_time
        
        # 选择图片材料 (如果图片不够，则循环使用)
        image_index = i % len(image_materials)
        image_material = image_materials[image_index]
        
        print(f"\n图片 {i}:")
        print(f"开始时间: {microsec_to_time(start_time)}")
        print(f"结束时间: {microsec_to_time(end_time)}")
        print(f"持续时间: {duration / 1000000:.2f}秒秒")
        print(f"对应字幕: {microsec_to_time(start_time)} - {microsec_to_time(end_time)}")
        
        # 创建图片片段
        image_segment = {
            "cartoon": False,
            "clip": {
                "alpha": 1.0,
                "flip": {"horizontal": False, "vertical": False},
                "rotation": 0.0,
                "scale": {"x": 1.25, "y": 1.25},  # 使用1.25的缩放比例，与英语字幕对齐.py一致
                "transform": {"x": 0.0, "y": 0.0}
            },
            "common_keyframes": [],
            "enable_adjust": False,
            "enable_color_curves": True,
            "enable_color_wheels": True,
            "extra_material_refs": [],
            "group_id": "",
            "id": str(uuid.uuid4()),
            "intensifies_audio": False,
            "is_placeholder": False,
            "material_id": image_material['id'],
            "render_index": i,
            "source_timerange": {
                "duration": duration,
                "start": 0
            },
            "speed": 1.0,
            "target_timerange": {
                "duration": duration,
                "start": start_time
            },
            "template_id": "",
            "template_scene": "default",
            "track_attribute": 0,
            "track_render_index": 0,
            "uniform_scale": {"on": True, "value": 1.0},
            "visible": True,
            "volume": 1.0
        }
        
        # 创建随机动画效果
        animation = get_random_animation()
        draft['materials']['material_animations'].append(animation)
        
        # 添加动画引用到 extra_material_refs
        image_segment['extra_material_refs'].append(animation['id'])
        
        # 设置关键帧
        image_segment['common_keyframes'] = create_common_keyframes(start_time, duration)
        
        # 添加到图片轨道
        image_track['segments'].append(image_segment)
        created_segments.append(image_segment)
        
        # 更新下一张图片的开始时间
        last_end_time = end_time
    
    # 添加特效轨道
    effect_track = add_effect_track(draft, created_segments)
    draft['tracks'].append(effect_track)
    
    print(f"已添加 {len(created_segments)} 个图片片段，并应用了关键帧和特效")
    return draft

def process_draft_automatically():
    """
    自动处理最新的剪映草稿，同步图片与字幕
    
    Returns:
        None
    """
    try:
        # 获取最新的草稿文件夹
        latest_draft_folder = get_latest_draft_folder()
        print(f"找到最新草稿文件夹: {latest_draft_folder}")
        
        # 读取draft_content.json
        draft_content_path = os.path.join(latest_draft_folder, 'draft_content.json')
        with open(draft_content_path, 'r', encoding='utf-8') as f:
            draft = json.load(f)
        
        # 同步图片与字幕
        draft = sync_images_with_subtitles_in_draft(draft)
        
        # 保存修改后的draft_content.json
        with open(draft_content_path, 'w', encoding='utf-8') as f:
            json.dump(draft, f, ensure_ascii=False)
        
        print(f"成功将图片与字幕同步到草稿: {latest_draft_folder}")
        
    except Exception as e:
        print(f"处理草稿时出错: {str(e)}")

if __name__ == "__main__":
    # 直接处理草稿，不需要用户输入
    process_draft_automatically()
```
