
# 使用此指令前，请确保安装必要的 `Python` 库

## 安装依赖

例如使用以下命令安装：

```bash
pip install pandas openpyxl
```

## 代码实现

```python
import pandas as pd
from openpyxl import load_workbook
from openpyxl.styles import Font, Alignment, Border, Side
from typing import *

try:
    from xbot.app.logging import trace as print
except:
    from xbot import print


def create_sheets_by_category_with_format(excel_path):
    """title: 在原文件中创建统一格式的 Excel 工作表
    description: 读取 Excel 文件中 "分类" 列的内容，为每个分类在原文件中创建新的工作表，对所有工作表（除 "分类" 外）设置统一格式，列名使用方正仿宋_GBK 加粗，数据行使用等线字体。将 Sheet1 重命名为 "分类"。
    inputs:
      - excel_path (file): Excel 文件路径，eg: "data.xlsx"
    outputs: None
    """

    def _read_excel_and_get_categories(file_path):
        """读取 Excel 文件并获取分类列的唯一值"""
        try:
            df = pd.read_excel(file_path)
            if '分类' not in df.columns:
                raise ValueError("Excel文件中未找到'分类'列")
            categories = df['分类'].dropna().unique().tolist()
            return categories
        except Exception as e:
            raise Exception(f"读取Excel文件失败: {str(e)}")

    def _check_sheet_exists_with_columns(workbook, sheet_name, required_columns):
        """检查工作表是否存在且包含指定列名"""
        if sheet_name not in workbook.sheetnames:
            return False

        worksheet = workbook[sheet_name]
        if worksheet.max_row == 0:
            return False

        # 获取第一行的值作为列名
        first_row = [cell.value for cell in worksheet[1]]
        existing_columns = [col for col in first_row if col is not None]

        # 检查是否包含所有必需的列
        return all(col in existing_columns for col in required_columns)

    def _clean_sheet_name(category_name):
        """清理工作表名称，确保不包含 'sheet' 并符合 Excel 命名规则"""
        sheet_name = str(category_name).replace('sheet', '').replace('Sheet', '').replace('SHEET', '')
        # 移除 Excel 不允许的字符
        invalid_chars = ['\\', '/', '*', '[', ']', ':', '?']
        for char in invalid_chars:
            sheet_name = sheet_name.replace(char, '_')
        # 确保名称不为空且不超过 31 个字符
        sheet_name = sheet_name.strip()
        if not sheet_name:
            sheet_name = "分类表"
        return sheet_name[:31]

    def _rename_sheet1_to_category(workbook):
        """将 Sheet1 重命名为分类"""
        if 'Sheet1' in workbook.sheetnames:
            sheet1 = workbook['Sheet1']
            if '分类' not in workbook.sheetnames:
                sheet1.title = '分类'
            else:
                # 如果分类已存在，删除 Sheet1
                workbook.remove(sheet1)

    def _format_worksheet(worksheet, column_names):
        """设置工作表统一格式：固定行高、列宽和预设格式，确保后续新增列也能保持统一"""
        # 定义每列的固定宽度
        column_widths = {
            '联盟类型': 12,
            '商品链接': 20,
            '佣金': 10,
            '佣金比例': 12,
            '推广链接': 20,
            '商品名称': 18,
            '到手价': 10,
            '好评数': 10
        }

        # 统一的字体和样式设置
        header_font = Font(name='方正仿宋_GBK', size=11, bold=True, color='000000')  # 列名字体不变
        data_font = Font(name='等线', size=10, color='000000')  # 数据行字体改为等线
        header_alignment = Alignment(horizontal='center', vertical='center')
        data_alignment = Alignment(horizontal='left', vertical='center')
        border = Border(
            left=Side(border_style='thin', color='000000'),
            right=Side(border_style='thin', color='000000'),
            top=Side(border_style='thin', color='000000'),
            bottom=Side(border_style='thin', color='000000')
        )

        # 清空工作表内容并添加列名
        worksheet.delete_rows(1, worksheet.max_row)

        # 设置所有列的默认宽度和格式（预设到 Z 列，确保后续新增列也有统一格式）
        for col_idx in range(1, 27):  # A 到 Z 列
            column_letter = worksheet.cell(row=1, column=col_idx).column_letter
            worksheet.column_dimensions[column_letter].width = 15
            worksheet.column_dimensions[column_letter].auto_size = False

        # 设置指定的列名和其对应的列宽
        for col_idx, col_name in enumerate(column_names, 1):
            cell = worksheet.cell(row=1, column=col_idx)
            cell.value = col_name
            cell.font = header_font
            cell.alignment = header_alignment
            cell.border = border

            # 设置特定列的宽度
            column_letter = cell.column_letter
            width = column_widths.get(col_name, 15)
            worksheet.column_dimensions[column_letter].width = width

        # 为标题行的所有列（A 到 Z）预设格式，确保后续新增列名也有统一格式
        for col_idx in range(1, 27):  # A 到 Z 列
            cell = worksheet.cell(row=1, column=col_idx)
            if not cell.value:  # 如果单元格为空，不设置值
                pass
            cell.font = header_font
            cell.alignment = header_alignment
            cell.border = border

        # 设置固定行高（禁用自适应）
        # 标题行
        worksheet.row_dimensions[1].height = 24
        worksheet.row_dimensions[1].auto_size = False

        # 预设数据行的固定行高和格式（预设 500 行，确保后续新增数据也有统一格式）
        for row_num in range(2, 501):
            worksheet.row_dimensions[row_num].height = 20
            worksheet.row_dimensions[row_num].auto_size = False

            # 为数据行的所有列预设格式（使用等线字体）
            for col_idx in range(1, 27):  # A 到 Z 列
                cell = worksheet.cell(row=row_num, column=col_idx)
                cell.font = data_font  # 数据行使用等线字体
                cell.alignment = data_alignment
                cell.border = border

        # 冻结首行
        worksheet.freeze_panes = 'A2'

    def _create_and_format_sheets(file_path, categories):
        """创建工作表并格式化所有工作表（除分类外）"""
        column_names = ['联盟类型', '商品链接', '佣金', '佣金比例', '推广链接', '商品名称', '到手价', '好评数']

        try:
            workbook = load_workbook(file_path)
        except Exception as e:
            raise Exception(f"无法打开 Excel 文件: {str(e)}")

        # 将 Sheet1 重命名为分类
        _rename_sheet1_to_category(workbook)

        # 创建新的分类工作表
        for category in categories:
            sheet_name = _clean_sheet_name(category)

            # 跳过 "分类" 工作表
            if sheet_name == '分类':
                continue

            # 检查工作表是否已存在且包含指定列名
            if _check_sheet_exists_with_columns(workbook, sheet_name, column_names):
                continue

            # 创建或清理工作表
            if sheet_name not in workbook.sheetnames:
                workbook.create_sheet(title=sheet_name)

        # 格式化所有工作表（除了 "分类"）
        for sheet_name in workbook.sheetnames:
            if sheet_name != '分类':
                worksheet = workbook[sheet_name]
                _format_worksheet(worksheet, column_names)

        # 保存文件
        workbook.save(file_path)
        workbook.close()

    # 获取分类列表
    categories = _read_excel_and_get_categories(excel_path)

    if not categories:
        raise ValueError("分类列中没有找到有效数据")

    # 创建工作表并格式化
    _create_and_format_sheets(excel_path, categories)
```
