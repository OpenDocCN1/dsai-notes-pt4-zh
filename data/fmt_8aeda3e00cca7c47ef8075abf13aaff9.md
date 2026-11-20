
# 基于Agent的数据分析实战项目

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/B2LUdsig8oRxOCxLGPbctEsdnUg](https://qqs7y1hozd1.feishu.cn/docx/B2LUdsig8oRxOCxLGPbctEsdnUg)

## 一、数据分析智能体核心架构

### 1.1 整体架构设计

数据分析智能体的核心架构采用模块化设计，包含数据接入、智能分析、模型管理和输出呈现四个主要层次。

![](img/8b88461f581e804d0f85bd306430e6dd.png)

### 1.2 核心模块实现

```
import pandas as pd
import numpy as np
from typing import Dict, List, Any, Optional
from dataclasses import dataclass
import logging

@dataclass
class AnalysisTask:
    """数据分析任务定义"""
    task_id: str
    task_type: str  # 'exploration', 'modeling', 'prediction', 'monitoring'
    data_source: str
    requirements: Dict[str, Any]
    priority: int = 1

class DataAnalysisAgent:
    """数据分析智能体核心类"""

    def __init__(self, config: Dict[str, Any]):
        self.config = config
        self.logger = logging.getLogger(__name__)
        self.data_processor = DataProcessor()
        self.model_manager = ModelManager()
        self.visualization_engine = VisualizationEngine()
        self.report_generator = ReportGenerator()

    async def process_task(self, task: AnalysisTask) -> Dict[str, Any]:
        """处理数据分析任务的主要入口"""
        try:
            # 1. 数据加载和预处理
            data = await self._load_and_preprocess_data(task.data_source)

            # 2. 根据任务类型执行相应分析
            if task.task_type == 'exploration':
                results = await self._perform_eda(data, task.requirements)
            elif task.task_type == 'modeling':
                results = await self._build_model(data, task.requirements)
            elif task.task_type == 'prediction':
                results = await self._make_predictions(data, task.requirements)
            elif task.task_type == 'monitoring':
                results = await self._monitor_metrics(data, task.requirements)
            else:
                raise ValueError(f"不支持的任务类型: {task.task_type}")

            # 3. 生成报告和洞察
            report = await self._generate_report(results, task)

            return {
                'task_id': task.task_id,
                'status': 'completed',
                'results': results,
                'report': report,
                'insights': await self._extract_insights(results)
            }

        except Exception as e:
            self.logger.error(f"任务处理失败: {str(e)}")
            return {
                'task_id': task.task_id,
                'status': 'failed',
                'error': str(e)
            }
```

## 二、数据探索与可视化自动化

### 2.1 自动化数据探索流程

数据探索是数据分析的第一步，智能体需要能够自动识别数据特征、发现异常值和分布模式。

![](img/82b5ab118f1729f7fedab82ea78a840e.png)

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from scipy import stats
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans
import warnings
warnings.filterwarnings('ignore')

class AutomatedEDA:
    """自动化探索性数据分析模块"""

    def __init__(self):
        self.report_data = {}

    def analyze_dataset(self, df: pd.DataFrame) -> Dict[str, Any]:
        """执行完整的自动化数据探索"""

        # 基础信息分析
        basic_info = self._get_basic_info(df)

        # 数据质量分析
        quality_analysis = self._analyze_data_quality(df)

        # 统计特征分析
        statistical_analysis = self._perform_statistical_analysis(df)

        # 相关性分析
        correlation_analysis = self._analyze_correlations(df)

        # 异常值检测
        outlier_analysis = self._detect_outliers(df)

        # 生成可视化
        visualizations = self._generate_visualizations(df)

        return {
            'basic_info': basic_info,
            'quality_analysis': quality_analysis,
            'statistical_analysis': statistical_analysis,
            'correlation_analysis': correlation_analysis,
            'outlier_analysis': outlier_analysis,
            'visualizations': visualizations
        }

    def _analyze_data_quality(self, df: pd.DataFrame) -> Dict[str, Any]:
        """数据质量分析"""
        quality_metrics = {}

        # 缺失值分析
        missing_analysis = {}
        for col in df.columns:
            missing_count = df[col].isnull().sum()
            missing_percentage = (missing_count / len(df)) * 100
            missing_analysis[col] = {
                'missing_count': missing_count,
                'missing_percentage': round(missing_percentage, 2)
            }

        # 重复值分析
        duplicate_count = df.duplicated().sum()
        duplicate_percentage = (duplicate_count / len(df)) * 100

        # 数据类型一致性检查
        type_consistency = self._check_type_consistency(df)

        quality_metrics = {
            'missing_analysis': missing_analysis,
            'duplicate_count': duplicate_count,
            'duplicate_percentage': round(duplicate_percentage, 2),
            'type_consistency': type_consistency
        }

        return quality_metrics

    def _detect_outliers(self, df: pd.DataFrame) -> Dict[str, Any]:
        """异常值检测"""
        outlier_results = {}
        numeric_columns = df.select_dtypes(include=[np.number]).columns

        for col in numeric_columns:
            # IQR方法检测异常值
            Q1 = df[col].quantile(0.25)
            Q3 = df[col].quantile(0.75)
            IQR = Q3 - Q1
            lower_bound = Q1 - 1.5 * IQR
            upper_bound = Q3 + 1.5 * IQR

            outliers = df[(df[col] < lower_bound) | (df[col] > upper_bound)]

            # Z-score方法
            z_scores = np.abs(stats.zscore(df[col].dropna()))
            z_outliers = len(z_scores[z_scores > 3])

            outlier_results[col] = {
                'iqr_outliers': len(outliers),
                'z_score_outliers': z_outliers,
                'outlier_percentage': round((len(outliers) / len(df)) * 100, 2)
            }

        return outlier_results
```

### 2.2 智能可视化生成

智能体需要根据数据特征自动选择最合适的可视化方法。
