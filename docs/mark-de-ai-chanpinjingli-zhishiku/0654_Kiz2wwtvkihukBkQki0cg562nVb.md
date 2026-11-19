
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



```
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px
import plotly.graph_objects as go
from plotly.subplots import make_subplots

class IntelligentVisualization:
    """智能可视化生成器"""

    def __init__(self):
        plt.style.use('seaborn-v0_8')
        sns.set_palette("husl")

    def auto_visualize(self, df: pd.DataFrame, target_column: str = None) -> Dict[str, str]:
        """根据数据特征自动生成可视化"""
        visualizations = {}

        # 数值变量分布图
        numeric_cols = df.select_dtypes(include=[np.number]).columns
        if len(numeric_cols) > 0:
            visualizations['distributions'] = self._create_distribution_plots(df, numeric_cols)

        # 分类变量分析
        categorical_cols = df.select_dtypes(include=['object', 'category']).columns
        if len(categorical_cols) > 0:
            visualizations['categorical'] = self._create_categorical_plots(df, categorical_cols)

        # 相关性热力图
        if len(numeric_cols) > 1:
            visualizations['correlation'] = self._create_correlation_heatmap(df, numeric_cols)

        # 目标变量分析（如果指定）
        if target_column and target_column in df.columns:
            visualizations['target_analysis'] = self._analyze_target_variable(df, target_column)

        return visualizations

    def _create_distribution_plots(self, df: pd.DataFrame, numeric_cols: List[str]) -> str:
        """创建数值变量分布图"""
        n_cols = min(3, len(numeric_cols))
        n_rows = (len(numeric_cols) + n_cols - 1) // n_cols

        fig, axes = plt.subplots(n_rows, n_cols, figsize=(15, 5*n_rows))
        if n_rows == 1:
            axes = [axes] if n_cols == 1 else axes
        else:
            axes = axes.flatten()

        for i, col in enumerate(numeric_cols[:len(axes)]):
            # 直方图和核密度估计
            axes[i].hist(df[col].dropna(), bins=30, alpha=0.7, density=True, color='skyblue')
            df[col].dropna().plot.density(ax=axes[i], color='red', linewidth=2)
            axes[i].set_title(f'{col} 分布', fontsize=12, fontweight='bold')
            axes[i].set_ylabel('密度')
            axes[i].grid(True, alpha=0.3)

        # 隐藏多余的子图
        for i in range(len(numeric_cols), len(axes)):
            axes[i].set_visible(False)

        plt.tight_layout()
        plot_path = 'distributions.png'
        plt.savefig(plot_path, dpi=300, bbox_inches='tight')
        plt.close()

        return plot_path

    def _create_correlation_heatmap(self, df: pd.DataFrame, numeric_cols: List[str]) -> str:
        """创建相关性热力图"""
        correlation_matrix = df[numeric_cols].corr()

        plt.figure(figsize=(12, 10))
        mask = np.triu(np.ones_like(correlation_matrix, dtype=bool))

        sns.heatmap(correlation_matrix,
                   mask=mask,
                   annot=True,
                   cmap='RdYlBu_r',
                   center=0,
                   square=True,
                   fmt='.2f',
                   cbar_kws={"shrink": .8})

        plt.title('变量相关性热力图', fontsize=16, fontweight='bold', pad=20)
        plt.tight_layout()

        plot_path = 'correlation_heatmap.png'
        plt.savefig(plot_path, dpi=300, bbox_inches='tight')
        plt.close()

        return plot_path
```

## 三、统计分析与机器学习建模

### 3.1 自动化建模流程

机器学习建模是数据分析智能体的核心能力，需要实现从特征工程到模型选择的全自动化流程。

![](img/47db81f6655170dd1eaeed7c961d6dcc.png)



```
from sklearn.model_selection import train_test_split, GridSearchCV, cross_val_score
from sklearn.ensemble import RandomForestRegressor, RandomForestClassifier, GradientBoostingRegressor
from sklearn.linear_model import LinearRegression, LogisticRegression
from sklearn.svm import SVR, SVC
from sklearn.metrics import mean_squared_error, r2_score, accuracy_score, classification_report
from sklearn.preprocessing import StandardScaler, LabelEncoder
import xgboost as xgb
import lightgbm as lgb

class AutoMLModeling:
    """自动化机器学习建模模块"""
    
    def __init__(self):
        self.models = {}
        self.best_model = None
        self.scaler = StandardScaler()
        self.label_encoders = {}
        
    def auto_build_model(self, df: pd.DataFrame, target_column: str, 
                        problem_type: str = 'auto') -> Dict[str, Any]:
        """自动构建机器学习模型"""
        
        # 数据预处理
        X, y = self._prepare_features(df, target_column)
        
        # 自动判断问题类型
        if problem_type == 'auto':
            problem_type = self._detect_problem_type(y)
        
        # 数据分割
        X_train, X_test, y_train, y_test = train_test_split(
            X, y, test_size=0.2, random_state=42, stratify=y if problem_type == 'classification' else None
        )
        
        # 特征缩放
        X_train_scaled = self.scaler.fit_transform(X_train)
        X_test_scaled = self.scaler.transform(X_test)
        
        # 模型训练和评估
        if problem_type == 'regression':
            results = self._train_regression_models(X_train_scaled, X_test_scaled, y_train, y_test)
        else:
            results = self._train_classification_models(X_train_scaled, X_test_scaled, y_train, y_test)
        
        # 特征重要性分析
        feature_importance = self._analyze_feature_importance(X.columns)
        
        return {
            'problem_type': problem_type,
            'model_results': results,
            'best_model': self.best_model,
            'feature_importance': feature_importance,
            'model_metrics': self._get_detailed_metrics(y_test, problem_type)
        }
    
    def _train_regression_models(self, X_train: np.ndarray, X_test: np.ndarray, 
                               y_train: np.ndarray, y_test: np.ndarray) -> Dict[str, Any]:
        """训练回归模型"""
        models = {
            'Linear Regression': LinearRegression(),
            'Random Forest': RandomForestRegressor(n_estimators=100, random_state=42),
            'Gradient Boosting': GradientBoostingRegressor(n_estimators=100, random_state=42),
            'XGBoost': xgb.XGBRegressor(n_estimators=100, random_state=42),
            'LightGBM': lgb.LGBMRegressor(n_estimators=100, random_state=42, verbose=-1)
        }
        
        results = {}
        best_score = -np.inf
        
        for name, model in models.items():
            # 训练模型
            model.fit(X_train, y_train)
            
            # 预测
            y_pred = model.predict(X_test)
            
            # 评估指标
            mse = mean_squared_error(y_test, y_pred)
            rmse = np.sqrt(mse)
            r2 = r2_score(y_test, y_pred)
            
            # 交叉验证
            cv_scores = cross_val_score(model, X_train, y_train, cv=5, scoring='r2')
            
            results[name] = {
                'model': model,
                'mse': mse,
                'rmse': rmse,
                'r2_score': r2,
                'cv_mean': cv_scores.mean(),
                'cv_std': cv_scores.std()
            }
            
            # 更新最佳模型
            if r2 > best_score:
                best_score = r2
                self.best_model = model
        
        return results
    
    def _analyze_feature_importance(self, feature_names: List[str]) -> Dict[str, float]:
        """分析特征重要性"""
        if hasattr(self.best_model, 'feature_importances_'):
            importances = self.best_model.feature_importances_
            feature_importance = dict(zip(feature_names, importances))
            # 按重要性排序
            return dict(sorted(feature_importance.items(), key=lambda x: x[1], reverse=True))
        else:
            return {}
    
    def hyperparameter_optimization(self, X: np.ndarray, y: np.ndarray, 
                                  model_type: str) -> Dict[str, Any]:
        """超参数优化"""
        param_grids = {
            'random_forest': {
                'n_estimators': [50, 100, 200],
                'max_depth': [None, 10, 20, 30],
                'min_samples_split': [2, 5, 10],
                'min_samples_leaf': [1, 2, 4]
            },
            'xgboost': {
                'n_estimators': [50, 100, 200],
                'max_depth': [3, 6, 9],
                'learning_rate': [0.01, 0.1, 0.2],
                'subsample': [0.8, 0.9, 1.0]
            }
        }
        
        if model_type in param_grids:
            if model_type == 'random_forest':
                model = RandomForestRegressor(random_state=42)
            elif model_type == 'xgboost':
                model = xgb.XGBRegressor(random_state=42)
            
            grid_search = GridSearchCV(
                model, 
                param_grids[model_type], 
                cv=5, 
                scoring='r2',
                n_jobs=-1
            )
            
            grid_search.fit(X, y)
            
            return {
                'best_params': grid_search.best_params_,
                'best_score': grid_search.best_score_,
                'best_model': grid_search.best_estimator_
            }
        
        return {}
```

### 3.2 模型性能评估体系

建立完善的模型评估体系是确保智能体产出可靠结果的关键。

![](img/746f9158276a48f5949324f564f152a9.png)

## 四、报告生成与洞察提取

### 4.1 智能报告生成系统

自动化报告生成是数据分析智能体的重要输出能力，需要将复杂的分析结果转化为易懂的商业洞察。



### 4.2 业务洞察自动提取

```python
from jinja2 import Template
import matplotlib.pyplot as plt
from datetime import datetime
import json

class IntelligentReportGenerator:
    """智能报告生成器"""
    
    def __init__(self):
        self.template_engine = Template
        self.insights = []
        
    def generate_comprehensive_report(self, analysis_results: Dict[str, Any], 
                                    business_context: Dict[str, Any] = None) -> str:
        """生成综合分析报告"""
        
        # 提取关键洞察
        key_insights = self._extract_key_insights(analysis_results)
        
        # 生成执行摘要
        executive_summary = self._generate_executive_summary(key_insights, business_context)
        
        # 创建详细分析章节
        detailed_analysis = self._create_detailed_sections(analysis_results)
        
        # 生成建议和下一步行动
        recommendations = self._generate_recommendations(key_insights, business_context)
        
        # 编译完整报告
        report = self._compile_report({
            'executive_summary': executive_summary,
            'key_insights': key_insights,
            'detailed_analysis': detailed_analysis,
            'recommendations': recommendations,
            'metadata': {
                'generated_at': datetime.now().strftime('%Y-%m-%d %H:%M:%S'),
                'analysis_type': analysis_results.get('problem_type', 'Unknown'),
                'data_points': analysis_results.get('data_size', 'Unknown')
            }
        })
        
        return report
    
    def _extract_key_insights(self, results: Dict[str, Any]) -> List[str]:
        """提取关键业务洞察"""
        insights = []
        
        # 数据质量洞察
        if 'quality_analysis' in results:
            qa = results['quality_analysis']
            missing_vars = [col for col, info in qa['missing_analysis'].items() 
                          if info['missing_percentage'] > 20]
            if missing_vars:
                insights.append(f"发现{len(missing_vars)}个变量存在超过20%的缺失值，需要关注数据收集质量")
        
        # 模型性能洞察
        if 'model_results' in results:
            best_model = max(results['model_results'].items(), 
                           key=lambda x: x[1].get('r2_score', x[1].get('accuracy', 0)))
            insights.append(f"最佳模型为{best_model[0]}，性能指标达到{best_model[1].get('r2_score', best_model[1].get('accuracy', 0)):.3f}")
        
        # 特征重要性洞察
        if 'feature_importance' in results:
            top_features = list(results['feature_importance'].keys())[:3]
            insights.append(f"影响目标变量的前三个关键因素为：{', '.join(top_features)}")
        
        return insights
    
    def _generate_executive_summary(self, insights: List[str], 
                                  business_context: Dict[str, Any] = None) -> str:
        """生成执行摘要"""
        summary_template = """
        ## 执行摘要
        
        本次数据分析围绕{{business_objective}}展开，通过对{{data_description}}的深入分析，
        我们发现了以下关键洞察：
        
        {% for insight in key_insights %}
        - {{insight}}
        {% endfor %}
        
        基于这些发现，我们建议采取相应的优化措施，预计可以带来{{expected_impact}}的业务改进。
        """
        
        template = Template(summary_template)
        return template.render(
            business_objective=business_context.get('objective', '业务目标优化') if business_context else '数据驱动决策',
            data_description=business_context.get('data_description', '核心业务数据') if business_context else '多维度数据',
            key_insights=insights,
            expected_impact=business_context.get('expected_impact', '显著') if business_context else '积极'
        )
```



```
import numpy as np
from scipy import stats
from typing import List, Dict, Any, Tuple

class BusinessInsightExtractor:
    """业务洞察自动提取器"""

    def __init__(self):
        self.insight_rules = self._load_insight_rules()

    def extract_insights(self, df: pd.DataFrame, analysis_results: Dict[str, Any]) -> List[Dict[str, Any]]:
        """自动提取业务洞察"""
        insights = []

        # 趋势分析洞察
        trend_insights = self._analyze_trends(df, analysis_results)
        insights.extend(trend_insights)

        # 异常模式洞察
        anomaly_insights = self._detect_anomaly_patterns(df, analysis_results)
        insights.extend(anomaly_insights)

        # 相关性洞察
        correlation_insights = self._extract_correlation_insights(df, analysis_results)
        insights.extend(correlation_insights)

        # 分群洞察
        segmentation_insights = self._analyze_segmentation(df, analysis_results)
        insights.extend(segmentation_insights)

        # 按重要性排序
        insights.sort(key=lambda x: x['importance_score'], reverse=True)

        return insights

    def _analyze_trends(self, df: pd.DataFrame, results: Dict[str, Any]) -> List[Dict[str, Any]]:
        """分析趋势模式"""
        insights = []

        # 检测时间序列趋势
        date_columns = df.select_dtypes(include=['datetime64']).columns
        numeric_columns = df.select_dtypes(include=[np.number]).columns

        for date_col in date_columns:
            for num_col in numeric_columns:
                if len(df) > 10:  # 确保有足够的数据点
                    # 计算趋势斜率
                    x = np.arange(len(df))
                    y = df[num_col].values

                    # 去除缺失值
                    valid_mask = ~np.isnan(y)
                    if np.sum(valid_mask) > 5:
                        slope, intercept, r_value, p_value, std_err = stats.linregress(x[valid_mask], y[valid_mask])

                        if abs(r_value) > 0.5 and p_value < 0.05:
                            trend_direction = "上升" if slope > 0 else "下降"
                            insights.append({
                                'type': 'trend',
                                'title': f'{num_col}呈现明显{trend_direction}趋势',
                                'description': f'{num_col}在时间序列上呈现{trend_direction}趋势，相关系数为{r_value:.3f}，统计显著性p值为{p_value:.3f}',
                                'importance_score': abs(r_value) * 0.8,
                                'actionable': True,
                                'confidence': 1 - p_value
                            })

        return insights

    def _extract_correlation_insights(self, df: pd.DataFrame, results: Dict[str, Any]) -> List[Dict[str, Any]]:
        """提取相关性洞察"""
        insights = []

        numeric_df = df.select_dtypes(include=[np.number])
        if len(numeric_df.columns) > 1:
            correlation_matrix = numeric_df.corr()

            # 查找强相关关系
            for i in range(len(correlation_matrix.columns)):
                for j in range(i + 1, len(correlation_matrix.columns)):
                    corr_value = correlation_matrix.iloc[i, j]

                    if abs(corr_value) > 0.7:  # 强相关阈值
                        var1 = correlation_matrix.columns[i]
                        var2 = correlation_matrix.columns[j]

                        correlation_type = "正相关" if corr_value > 0 else "下降"

                        insights.append({
                            'type': 'correlation',
                            'title': f'{var1}与{var2}存在强{correlation_type}',
                            'description': f'{var1}与{var2}之间存在{correlation_type}关系，相关系数为{corr_value:.3f}',
                            'importance_score': abs(corr_value) * 0.9,
                            'actionable': True,
                            'confidence': abs(corr_value),
                            'variables': [var1, var2]
                        })

        return insights
```

## 五、业务指标监控与预警

### 5.1 实时监控系统架构

业务指标监控与预警是数据分析智能体的重要应用场景，能够帮助企业及时发现业务异常。

![](img/e5755cbfb0335d256a4cb6fbbc89d36d.png)



```python
import pandas as pd
import numpy as np
from datetime import datetime, timedelta
from typing import Dict, List, Any, Optional
import asyncio
from dataclasses import dataclass
from enum import Enum

class AlertLevel(Enum):
    """告警级别枚举"""
    INFO = "info"
    WARNING = "warning"
    CRITICAL = "critical"
    EMERGENCY = "emergency"

@dataclass
class MetricThreshold:
    """指标阈值配置"""
    metric_name: str
    warning_threshold: float
    critical_threshold: float
    emergency_threshold: float
    direction: str  # 'upper', 'lower', 'both'

@dataclass
class Alert:
    """告警对象"""
    alert_id: str
    metric_name: str
    current_value: float
    threshold_value: float
    alert_level: AlertLevel
    timestamp: datetime
    message: str

class BusinessMetricsMonitor:
    """业务指标监控器"""

    def __init__(self, config: Dict[str, Any]):
        self.config = config
        self.thresholds = {}
        self.alert_history = []
        self.metric_buffer = {}

    def add_metric_threshold(self, threshold: MetricThreshold):
        """添加指标阈值配置"""
        self.thresholds[threshold.metric_name] = threshold

    async def monitor_metrics(self, metrics_data: Dict[str, float]) -> List[Alert]:
        """监控业务指标"""
        alerts = []
        current_time = datetime.now()

        for metric_name, current_value in metrics_data.items():
            if metric_name in self.thresholds:
                threshold = self.thresholds[metric_name]
                alert = self._check_threshold(metric_name, current_value, threshold, current_time)

                if alert:
                    alerts.append(alert)

            # 更新指标缓冲区用于趋势分析
            self._update_metric_buffer(metric_name, current_value, current_time)

        # 趋势异常检测
        trend_alerts = await self._detect_trend_anomalies()
        alerts.extend(trend_alerts)

        return alerts

    def _check_threshold(self, metric_name: str, current_value: float,
                        threshold: MetricThreshold, timestamp: datetime) -> Optional[Alert]:
        """检查阈值违规"""

        alert_level = None
        threshold_value = None

        if threshold.direction in ['upper', 'both']:
            if current_value >= threshold.emergency_threshold:
                alert_level = AlertLevel.EMERGENCY
                threshold_value = threshold.emergency_threshold
            elif current_value >= threshold.critical_threshold:
                alert_level = AlertLevel.CRITICAL
                threshold_value = threshold.critical_threshold
            elif current_value >= threshold.warning_threshold:
                alert_level = AlertLevel.WARNING
                threshold_value = threshold.warning_threshold

        if threshold.direction in ['lower', 'both'] and not alert_level:
            if current_value <= threshold.emergency_threshold:
                alert_level = AlertLevel.EMERGENCY
                threshold_value = threshold.emergency_threshold
            elif current_value <= threshold.critical_threshold:
                alert_level = AlertLevel.CRITICAL
                threshold_value = threshold.critical_threshold
            elif current_value <= threshold.warning_threshold:
                alert_level = AlertLevel.WARNING
                threshold_value = threshold.warning_threshold

        if alert_level:
            alert_id = f"{metric_name}_{timestamp.strftime('%Y%m%d_%H%M%S')}"
            message = f"指标 {metric_name} 当前值 {current_value} 超过 {alert_level.value} 阈值 {threshold_value}"

            return Alert(
                alert_id=alert_id,
                metric_name=metric_name,
                current_value=current_value,
                threshold_value=threshold_value,
                alert_level=alert_level,
                timestamp=timestamp,
                message=message
            )

        return None

    async def _detect_trend_anomalies(self) -> List[Alert]:
        """检测趋势异常"""
        alerts = []

        for metric_name, buffer in self.metric_buffer.items():
            if len(buffer) >= 10:  # 至少需要10个数据点
                values = [point['value'] for point in buffer[-10:]]

                # 计算变化率
                recent_change = (values[-1] - values[0]) / values[0] * 100

                # 如果变化率超过50%，触发趋势告警
                if abs(recent_change) > 50:
                    direction = "急剧上升" if recent_change > 0 else "急剧下降"

                    alert = Alert(
                        alert_id=f"trend_{metric_name}_{datetime.now().strftime('%Y%m%d_%H%M%S')}",
                        metric_name=metric_name,
                        current_value=values[-1],
                        threshold_value=values[0],
                        alert_level=AlertLevel.WARNING,
                        timestamp=datetime.now(),
                        message=f"指标 {metric_name} 出现{direction}趋势，变化率为{recent_change:.2f}%"
                    )
                    alerts.append(alert)

        return alerts
```

### 5.2 关键业务指标监控配置

![](img/63bfa4d0662a433a0025d832d4940699.png)

## 六、技术栈对比与选型

### 6.1 数据分析工具对比

![](img/4c435152d2e17a9abc2222a02b5162e6.png)

### 6.2 成本效益分析

![](img/92ed7eea57523cc77f3f5316305ca4cf.png)

## 七、数据分析智能体评测体系

### 7.1 综合评测指标

建立科学的评测体系是确保数据分析智能体质量的关键。



```python
from typing import Dict, List, Any
import numpy as np
import time
from dataclasses import dataclass

@dataclass
class EvaluationMetrics:
    """评测指标定义"""
    accuracy_score: float
    processing_speed: float
    automation_level: float
    usability_score: float
    cost_efficiency: float
    reliability_score: float

class AgentEvaluator:
    """智能体评测器"""

    def __init__(self):
        self.weight_config = {
            'accuracy': 0.25,
            'speed': 0.20,
            'automation': 0.20,
            'usability': 0.15,
            'cost_efficiency': 0.10,
            'reliability': 0.10
        }

    def comprehensive_evaluation(self, agent_results: Dict[str, Any],
                               benchmark_data: Dict[str, Any]) -> EvaluationMetrics:
        """综合评测数据分析智能体"""

        # 准确性评测 (目标: >90%)
        accuracy = self._evaluate_accuracy(agent_results, benchmark_data)

        # 处理速度评测 (目标: 秒级响应)
        speed = self._evaluate_processing_speed(agent_results)

        # 自动化程度评测 (目标: 减少80%人工干预)
        automation = self._evaluate_automation_level(agent_results)

        # 易用性评测 (目标: 用户满意度>4.0)
        usability = self._evaluate_usability(agent_results)

        # 成本效益评测
        cost_efficiency = self._evaluate_cost_efficiency(agent_results, benchmark_data)

        # 可靠性评测
        reliability = self._evaluate_reliability(agent_results)

        return EvaluationMetrics(
            accuracy_score=accuracy,
            processing_speed=speed,
            automation_level=automation,
            usability_score=usability,
            cost_efficiency=cost_efficiency,
            reliability_score=reliability
        )

    def _evaluate_accuracy(self, results: Dict[str, Any], benchmark: Dict[str, Any]) -> float:
        """评测预测准确性"""
        if 'model_results' in results:
            model_metrics = results['model_results']
            best_model_name = max(model_metrics.keys(),
                                key=lambda x: model_metrics[x].get('r2_score',
                                                                 model_metrics[x].get('accuracy', 0)))

            best_score = model_metrics[best_model_name].get('r2_score',
                                                          model_metrics[best_model_name].get('accuracy', 0))

            # 转换为百分制
            return min(best_score * 100, 100)

        return 0

    def _evaluate_processing_speed(self, results: Dict[str, Any]) -> float:
        """评测处理速度"""
        if 'processing_time' in results:
            time_seconds = results['processing_time']

            # 速度评分：1秒内满分，超过60秒为0分
            if time_seconds <= 1:
                return 100
            elif time_seconds <= 60:
                return max(0, 100 - (time_seconds - 1) * (100 / 59))
            else:
                return 0

        return 50  # 默认中等分数

    def calculate_overall_score(self, metrics: EvaluationMetrics) -> float:
        """计算综合评分"""
        overall_score = (
            metrics.accuracy_score * self.weight_config['accuracy'] +
            metrics.processing_speed * self.weight_config['speed'] +
            metrics.automation_level * self.weight_config['automation'] +
            metrics.usability_score * self.weight_config['usability'] +
            metrics.cost_efficiency * self.weight_config['cost_efficiency'] +
            metrics.reliability_score * self.weight_config['reliability']
        )

        return round(overall_score, 2)
```

### 7.2 评测结果展示

![](img/f1ac4fee9eb67d15446aee5017282dc0.png)

## 八、实际应用案例

### 8.1 电商平台销售数据分析

以下是一个完整的电商平台销售数据分析案例，展示智能体的实际应用效果。

```python
import pandas as pd
import numpy as np
from data_analysis_agent.core.agent import DataAnalysisAgent, AnalysisTask

async def ecommerce_sales_analysis():
    """电商销售数据分析案例"""

    # 创建智能体实例
    agent = DataAnalysisAgent({
        'model_config': {
            'auto_feature_engineering': True,
            'model_selection': 'auto',
            'optimization': True
        }
    })

    # 模拟电商销售数据
    np.random.seed(42)
    dates = pd.date_range('2023-01-01', '2023-12-31', freq='D')

    ecommerce_data = pd.DataFrame({
        'date': dates,
        'product_category': np.random.choice(['电子产品', '服装', '家居', '美妆'], len(dates)),
        'sales_amount': np.random.normal(10000, 3000, len(dates)),
        'order_count': np.random.poisson(50, len(dates)),
        'customer_count': np.random.poisson(40, len(dates)),
        'marketing_spend': np.random.normal(2000, 500, len(dates)),
        'website_traffic': np.random.normal(5000, 1000, len(dates))
    })

    # 添加季节性和趋势
    ecommerce_data['sales_amount'] += (
        np.sin(2 * np.pi * np.arange(len(dates)) / 365) * 2000 +  # 年度季节性
        np.arange(len(dates)) * 5  # 增长趋势
    )

    # 创建分析任务
    analysis_task = AnalysisTask(
        task_id="ecommerce_sales_2023",
        task_type="exploration",
        data_source="ecommerce_data",
        requirements={
            'target_variable': 'sales_amount',
            'include_forecasting': True,
            'business_context': {
                'objective': '提升销售业绩和营销效率',
                'data_description': '电商平台年度销售数据',
                'expected_impact': '销售额提升15-20%'
            }
        }
    )

    # 执行分析
    results = await agent.process_task(analysis_task)

    # 输出关键发现
    print("=== 电商销售数据分析结果 ===")
    print(f"分析状态: {results['status']}")
    print(f"关键洞察: {results['insights'][:3]}")  # 显示前3个洞察

    return results

# 运行案例
if __name__ == "__main__":
    import asyncio
    results = asyncio.run(ecommerce_sales_analysis())
```

## 九、权威参考资源

### 9.1 技术文档参考

Scikit-learn官方文档: scikit-learn: machine learning in Python — scikit-learn 1.7.1 documentation

Pandas数据处理文档: pandas documentation — pandas 2.3.1 documentation

AutoML综述: AutoML | Home

Apache Spark MLlib: MLlib | Apache Spark

### 9.2 开源项目推荐

H2O AutoML: https://github.com/h2oai/h2o-3

TPOT自动机器学习: https://github.com/EpistasisLab/tpot

MLflow模型管理: https://github.com/mlflow/mlflow

Streamlit可视化: https://github.com/streamlit/streamlit
