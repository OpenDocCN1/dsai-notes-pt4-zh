
此脚本做「轻量数据工程」，方便 `Agent` 获得更干净的分析输入。[5]

```
#!/usr/bin/env python
# coding: utf-8

"""
数据清洗与聚合脚本示例

用法：
  python scripts/clean_and_aggregate.py \
    --input /tmp/result.csv \
    --group-by event_date,channel \
    --metrics new_users,retention_rate \
    --output /tmp/cleaned.csv
"""


import argparse
import sys
from pathlib import Path

import pandas as pd


def clean_data(df: pd.DataFrame) -> pd.DataFrame:
    # 去掉完全空的行
    df = df.dropna(how="all")

    # 去掉全重复行
    df = df.drop_duplicates()

    # 可以在这里添加一些通用清洗逻辑，例如去掉非法日期等
    return df


def aggregate_data(df: pd.DataFrame, group_by_cols, metric_cols):
    if not group_by_cols:
        return df, {}

    # 检查列是否存在
    missing_cols = [c for c in group_by_cols + metric_cols if c not in df.columns]
    if missing_cols:
        print(f"警告：以下列在数据中不存在，将跳过聚合：{missing_cols}", file=sys.stderr)
        # 尽量返回原始数据
        return df, {}

    grouped = df.groupby(group_by_cols, dropna=False)[metric_cols].sum().reset_index()

    stats = {
        "rows_before": len(df),
        "rows_after": len(grouped),
        "group_by": group_by_cols,
        "metrics": metric_cols,
    }
    return grouped, stats


def main():
    parser = argparse.ArgumentParser()
    parser.add_argument("--input", type=str, required=True, help="输入 CSV 文件路径")
    parser.add_argument("--group-by", type=str, default="", help="按哪些列分组，逗号分隔")
    parser.add_argument("--metrics", type=str, default="", help="需要聚合的指标列，逗号分隔")
    parser.add_argument("--output", type=str, required=True, help="输出 CSV 文件路径")

    args = parser.parse_args()

    input_path = Path(args.input)
    if not input_path.exists():
        print(f"输入文件不存在: {input_path}", file=sys.stderr)
        sys.exit(1)

    df = pd.read_csv(input_path)
    df = clean_data(df)

    group_by_cols = [c.strip() for c in args.group_by.split(",") if c.strip()]
    metric_cols = [c.strip() for c in args.metrics.split(",") if c.strip()]

    if group_by_cols and not metric_cols:
        print("提示：提供了 group-by 但没有 metrics，将只做去重/清洗。", file=sys.stderr)

    if group_by_cols:
        df_agg, stats = aggregate_data(df, group_by_cols, metric_cols)
    else:
        df_agg, stats = df, {"rows_before": len(df), "rows_after": len(df)}

    output_path = Path(args.output)
    df_agg.to_csv(output_path, index=False, encoding="utf-8")

    print(f"清洗与聚合完成，结果已写入: {output_path}")
    print(f"统计信息: {stats}")


if __name__ == "__main__":
    main()
```
