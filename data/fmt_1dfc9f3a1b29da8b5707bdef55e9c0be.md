
```python
import datetime  # 导入日期时间模块，用于处理日期和时间相关的操作
import re  # 导入正则表达式模块，用于字符串匹配和处理
from openpyxl import load_workbook  # 导入openpyxl库中的load_workbook函数，用于加载Excel文件

def 转换列表到飞书格式(表头, 数据, 多维表字段信息):
    # 检查输入参数是否为空，如果为空则抛出异常
    if not 表头 or not 数据 or not 多维表字段信息:
        raise ValueError("表头、数据和多维表字段信息参数都是必需的")

    # 如果数据是一维列表，将其转换为二维列表（单行数据）
    if not isinstance(数据[0], (list, tuple)):
        数据 = [数据]

    # 如果多维表字段信息是字典且包含"items"键，则提取items数组
    if isinstance(多维表字段信息, dict) and "items" in 多维表字段信息:
        多维表字段信息 = 多维表字段信息["items"]

    # 创建表头到飞书字段的映射
    header_to_field = {}
    for field in 多维表字段信息:
        field_name = field.get("field_name", "")
        if field_name:
            header_to_field[field_name] = field

    # 初始化记录列表，用于存储转换后的飞书格式数据
    feishu_records = []

    # 遍历每一行数据
    for row in 数据:
        record_fields = {}

        # 遍历每一行中的每个单元格
        for i, cell_value in enumerate(row):
            # 如果索引超出表头范围，跳过该单元格
            if i >= len(表头):
                continue

            header = 表头[i]

            # 如果表头不在映射中，跳过该单元格
            if header not in header_to_field:
                continue

            field = header_to_field[header]
            field_name = field.get("field_name", "")

            # 如果没有field_name，跳过此字段
            if not field_name:
                continue

            # 如果单元格值为空或None，跳过此字段
            if cell_value is None or cell_value == "":
                continue

            # 获取字段类型和UI类型
            field_type = field.get("type")
            ui_type = field.get("ui_type")

            # 处理URL类型
            if field_type == 15 or ui_type == "Url":
                if isinstance(cell_value, str):
                    converted_value = {
                        "text": cell_value,
                        "link": cell_value,
                    }
                else:
                    cell_value_str = str(cell_value)
                    converted_value = {
                        "text": cell_value_str,
                        "link": cell_value_str,
                    }

            # 处理附件字段
            elif field_type == 17 or ui_type == "Attachment":
                if isinstance(cell_value, list):
                    converted_value = cell_value
                else:
                    if not isinstance(cell_value, str):
                        cell_value = str(cell_value)
                    cleaned_value = cell_value
                    for char in "[]'\"":
                        cleaned_value = cleaned_value.replace(char, "")

                    separators = [",", "，", ";", "；", "|", "丨", "\n"]
                    paths = [cleaned_value]
                    for sep in separators:
                        if sep in cleaned_value:
                            paths = [
                                p.strip() for p in cleaned_value.split(sep) if p.strip()
                            ]
                            break

                    if (
                        len(paths) == 1
                        and paths[0] == cleaned_value
                        and any(char in cell_value for char in "[]'\"")
                    ):
                        paths = [paths[0]]

                    converted_value = paths

            # 处理多选字段
            elif field_type == 4 or ui_type == "MultiSelect":
                if isinstance(cell_value, list):
                    # 确保列表中的每个值都是字符串
                    converted_value = [
                        str(item) if not isinstance(item, str) else item
                        for item in cell_value
                    ]
                else:
                    # 确保cell_value是字符串
                    if not isinstance(cell_value, str):
                        cell_value = str(cell_value)

                    # 检查是否类似Python列表的字符串 "[选项1, 选项2]"
                    # 移除可能的列表标记和引号
                    cleaned_value = cell_value
                    for char in "[]'\"":
                        cleaned_value = cleaned_value.replace(char, "")

                    # 处理可能的分隔符
                    separators = [",", "，", ";", "；", "|", "丨", "\n"]
                    options = [cleaned_value]
                    for sep in separators:
                        if sep in cleaned_value:
                            options = [
                                opt.strip()
                                for opt in cleaned_value.split(sep)
                                if opt.strip()
                            ]
                            break

                    # 如果只有一个选项且与原始值相同，尝试检查原始值是否类似于列表
                    if (
                        len(options) == 1
                        and options[0] == cleaned_value
                        and any(char in cell_value for char in "[]'\"")
                    ):
                        # 可能是形如 "[选项]" 的单元素列表字符串
                        options = [options[0]]

                    converted_value = options

            # 处理单选字段
            elif field_type == 3 or ui_type == "SingleSelect":
                # 确保是字符串
                if not isinstance(cell_value, str):
                    converted_value = str(cell_value)
                else:
                    converted_value = cell_value

            # 处理文本类型
            elif field_type == 1 or ui_type == "Text":
                # 将所有值转换为字符串
                converted_value = str(cell_value)

            # 处理日期时间字段
            elif field_type == 5 or ui_type == "DateTime":
                try:
                    # 获取datetime对象
                    dt_obj = None
                    # 如果是datetime对象，直接使用
                    if isinstance(cell_value, datetime.datetime):
                        dt_obj = cell_value
                    # 如果是日期对象，转换为datetime
                    elif isinstance(cell_value, datetime.date):
                        dt_obj = datetime.datetime.combine(
                            cell_value, datetime.time.min
                        )
                    # 如果是字符串，尝试解析
                    elif isinstance(cell_value, str):
                        formats = [
                            "%Y/%m/%d",
                            "%Y-%m-%d",
                            "%Y/%m/%d %H:%M:%S",
                            "%Y-%m-%d %H:%M:%S",
                            "%d/%m/%Y",
                            "%d-%m-%Y",
                            "%m/%d/%Y",
                            "%m-%d-%Y",
                        ]
                        for fmt in formats:
                            try:
                                dt_obj = datetime.datetime.strptime(cell_value, fmt)
                                break
                            except ValueError:
                                continue

                    # 如果成功获取到datetime对象，转换为毫秒时间戳
                    if dt_obj:
                        # 转换为毫秒时间戳
                        timestamp_ms = int(dt_obj.timestamp() * 1000)
                        converted_value = timestamp_ms
                    else:
                        # 如果无法解析为日期，转为字符串
                        converted_value = str(cell_value)
                except Exception:
                    # 出现异常，转为字符串
                    converted_value = str(cell_value)

            # 处理复选框字段
            elif field_type == 7 or ui_type == "Checkbox":
                # 将各种值转换为布尔值
                if isinstance(cell_value, bool):
                    converted_value = cell_value
                elif isinstance(cell_value, (int, float)):
                    converted_value = bool(cell_value)
                elif isinstance(cell_value, str):
                    value_lower = cell_value.lower()
                    if value_lower in ("true", "yes", "y", "1", "是", "√", "✓"):
                        converted_value = True
                    elif value_lower in ("false", "no", "n", "0", "否", "×", "✗"):
                        converted_value = False
                    else:
                        continue
                else:
                    continue

            # 处理数字类型
            elif field_type == 2 or ui_type == "Number":
                try:
                    if isinstance(cell_value, str):
                        if "%" in cell_value:
                            cleaned_value = cell_value.replace("%", "").strip()
                            cleaned_value = cleaned_value.replace(",", "")
                            converted_value = float(cleaned_value) / 100
                        else:
                            cleaned_value = cell_value.replace(",", "")
                            converted_value = (
                                float(cleaned_value)
                                if "." in cleaned_value
                                else int(cleaned_value)
                            )
                    else:
                        converted_value = (
                            float(cell_value)
                            if "." in str(cell_value)
                            else int(cell_value)
                        )
                except (ValueError, TypeError):
                    converted_value = str(cell_value)

            # 处理其他类型
            else:
                converted_value = str(cell_value)
            record_fields[field_name] = converted_value

        # 将记录添加到列表中
        if record_fields:
            feishu_records.append(record_fields)

    # 将记录分批，每批最多1000条（飞书API限制）
    批次大小 = 1000
    batched_records = []
    for i in range(0, len(feishu_records), 批次大小):
        batch = feishu_records[i : i + 批次大小]
        batched_records.append(batch)

    if not batched_records:
        batched_records = [[]]

    return batched_records
```
