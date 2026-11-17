
# 平面图分析器实现

```
@dataclass
class AnalysisResult:
    """图像分析结果数据类"""
    answer: str  # VLM的回答
    extracted_info: Dict[str, Any]  # 提取的结构化信息
    raw_response: str  # 原始响应内容


class FloorPlanAnalyzer:
    """室内平面图分析器"""
    
    # 平面图专业提示词模板
    FLOOR_PLAN_PROMPT = """你是一位专业的建筑/室内平面图分析专家。请仔细分析这张室内平面布置图。

**用户问题：**
{question}

**重要说明：**
- 这是一张室内平面布置图，包含房间、尺寸标注、家具布置、动线等信息
- 图中尺寸单位通常为毫米(mm)或米(m)，请根据数值大小推断
- 请仔细识别所有可见的房间、标注、符号和空间关系

**分析维度（根据用户问题选择性回答）：**

1. **房间/功能区识别**：
   - 识别所有房间名称（客厅、卧室、厨房、卫生间、阳台等）
   - 标注每个房间的位置（左上/右下/中央等方位）
   - 识别特殊功能区（储藏室、玄关、衣帽间等）

2. **尺寸与面积**：
   - 提取图中所有可见尺寸标注
   - 推断单位并统一换算为米(m)
   - 计算房间的长、宽、面积
   - 标注整体平面外墙尺寸

3. **符号与标注**：
   - 解释符号含义（虚线、箭头、红点/红线、轴线等）
   - 识别文字标注（房间编号、面积、备注）
   - 说明墙体类型、门窗位置和开启方向

4. **家具布局**：
   - 列出所有可见家具及其位置
   - 判断空间利用率（拥挤/适中/空旷）
   - 识别家具尺寸

5. **动线与连通性**：
   - 标出主入口、次入口位置
   - 描述主要动线路径（如："入口→玄关→客厅→...")
   - 列出房间连通关系（哪些房间相连）
   - 判断布局类型（开放式/分隔式）

6. **设计评估**（如果问题涉及）：
   - 动线合理性、是否有绕行或死角
   - 采光/朝向分析
   - 空间优化建议

**回答方式：**
- 首先直接、简洁地回答用户的具体问题
- 然后提供相关的详细信息（如果用户问某个房间，重点描述该房间）
- 如果用户问整体布局，提供全局分析
- 如果涉及尺寸计算，请说明推理过程（如："标注22720mm = 22.72m"）

**输出格式（JSON）：**
{
    "answer": "直接回答用户问题的核心内容（简洁明了）",
    "extracted_info": {
        "total_dimensions": {
            "length": 22.72,
            "width": 12.5,
            "unit": "m",
            "total_area": 284.0
        },
        "rooms": [
            {
                "name": "客厅",
                "position": "中央偏右",
                "dimensions": {"length": 5.79, "width": 4.2, "area": 24.3, "unit": "m"},
                "furniture": ["沙发", "茶几"],
                "connected_to": ["餐厅", "卧室1"],
                "windows": 2,
                "doors": 1
            }
        ],
        "annotations": [
            {"type": "dimension", "value": "22720", "parsed_value": 22.72, "unit": "m", "description": "外墙总长"}
        ],
        "symbols": [
            {"type": "door", "count": 5, "positions": ["客厅-餐厅", "卧室1入口"]}
        ],
        "circulation": {
            "main_entrance": "底部中央",
            "main_path": "主入口 → 玄关 → 客厅 → 餐厅",
            "layout_type": "开放式客餐厅"
        },
        "design_notes": ["主卧带独立卫生间", "动线流畅"]
    }
}

**注意事项：**
- 如果标注不清晰，标注为"不可读"或给出估算值并说明
- 优先回答用户的具体问题，不要罗列所有信息
- 如果用户问"有几个卧室"，就重点回答卧室数量和位置
- 如果用户问"客厅面积"，就重点回答客厅的尺寸和面积
- 保持答案简洁、针对性强"""
    
    def __init__(self, client: OpenAI, model_name: str):
        """初始化分析器"""
        self.client = client
        self.model_name = model_name
    
    def load_image(self, image_path: str) -> Image.Image:
        """加载本地图片"""
        image_path = Path(image_path)
        if not image_path.exists():
            raise FileNotFoundError(f"图片文件不存在: {image_path}")
        
        image = Image.open(image_path)
        print(f"图片加载成功: {image.size}")
        return image
    
    def image_to_base64(self, image: Image.Image, max_size: int = 2000) -> str:
        """将PIL Image转换为base64字符串"""
        # 如果图片过大，进行压缩
        if image.width > max_size or image.height > max_size:
            image = image.copy()
            image.thumbnail((max_size, max_size), Image.Resampling.LANCZOS)
            print(f"图片已压缩到: {image.size}")
        
        # 转换为JPEG格式的base64
        buffer = io.BytesIO()
        if image.mode == 'RGBA':
            image = image.convert('RGB')
        image.save(buffer, format='JPEG', quality=85)
        buffer.seek(0)
        
        base64_str = base64.b64encode(buffer.read()).decode('utf-8')
        print(f"图片转换为base64: {len(base64_str) / 1024:.1f} KB")
        return base64_str
    
    def analyze(self, image_path: str, question: str) -> AnalysisResult:
        """
        分析平面图
        
        Args:
            image_path: 图片路径
            question: 用户问题
            
        Returns:
            AnalysisResult对象
        """
        
        # 1. 加载图片
        image = self.load_image(image_path)
        
        # 2. 转换为base64
        image_base64 = self.image_to_base64(image)
        
        # 3. 构建提示词
        prompt = self.FLOOR_PLAN_PROMPT.format(question=question)
        
        # 4. 调用VLM API
        print("正在调用VLM模型...")
        messages = [
            {
                "role": "system",
                "content": "你是一位专业的建筑平面图分析专家。请仔细分析图像并按照要求的JSON格式返回结果。"
            },
            {
                "role": "user",
                "content": [
                    {
                        "type": "image_url",
                        "image_url": {
                            "url": f"data:image/jpeg;base64,{image_base64}"
                        }
                    },
                    {
                        "type": "text",
                        "text": prompt
                    }
                ]
            }
        ]
        
        response = self.client.chat.completions.create(
            model=self.model_name,
            messages=messages,
            max_tokens=4096,
            temperature=0.1,
            response_format={"type": "json_object"}
        )
        
        # 5. 解析响应
        content = response.choices[0].message.content
        parsed = self._parse_json_response(content)
        
        print(f"分析完成！Token使用: {response.usage.total_tokens}")
        print("="*60 + "\n")
        
        return AnalysisResult(
            answer=parsed.get('answer', ''),
            extracted_info=parsed.get('extracted_info', {}),
            raw_response=content
        )
    
    def _parse_json_response(self, content: str) -> Dict[str, Any]:
        """解析JSON响应"""
        try:
            # 清理可能的markdown代码块标记
            content = content.strip()
            if content.startswith('```json'):

                content = content[7:]

            elif content.startswith('```'):
                content = content[3:]
            if content.endswith('```'):

                content = content[:-3]

            content = content.strip()

            return json.loads(content)

        except json.JSONDecodeError as e:

            print(f"JSON解析失败: {e}")

            return {

                'answer': content,

                'extracted_info': {}

            }

# 创建平面图分析器

analyzer = FloorPlanAnalyzer(vlm_client, MODEL_NAME)
```

这段代码是平面图分析系统的核心。让我们理解几个关键点：

### 1. 提示词针对平面图优化

平面图的提示词更加注重：

+ **房间识别**：客厅、卧室、厨房等功能区

+ **尺寸推断**：自动判断单位是mm还是m（如22720mm→22.72m）

+ **动线分析**：入口→玄关→客厅的流动路径

+ **空间关系**：房间之间的连通性和位置关系

### 2. 结构化输出的重要性

输出的JSON结构包含了完整的户型元数据：

+ `total_dimensions`：整体尺寸和总面积

+ `rooms`：每个房间的详细信息（名称、位置、面积、家具）

+ `circulation`：动线设计和布局类型

+ `design_notes`：设计特点和建议

这些结构化信息将成为后续智能问答的核心数据源！

## 5.4 测试平面图分析

接下来让我们测试分析器的功能。

> **提示**：请准备一张平面图（户型图），替换下面的路径后运行。

```
# ========== 测试平面图分析 ==========

# 指定平面图路径（请替换为你的平面图路径）

FLOOR_PLAN_PATH = "./test_data/house1.png"  # 示例路径

# 用户问题
```
