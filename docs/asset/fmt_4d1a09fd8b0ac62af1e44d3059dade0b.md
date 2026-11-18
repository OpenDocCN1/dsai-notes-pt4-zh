
```
from prometheus_client import Counter, Histogram, Gauge, start_http_server
import time
import threading

class MetricsCollector:
    """指标收集器"""

    def __init__(self):
        # 定义监控指标
        self.message_counter = Counter(
            'customer_service_messages_total',
            'Total number of customer service messages',
            ['channel', 'intent']
        )

        self.response_time_histogram = Histogram(
            'customer_service_response_time_seconds',
            'Response time of customer service agent',
            ['channel']
        )

        self.active_sessions_gauge = Gauge(
            'customer_service_active_sessions',
            'Number of active customer sessions'
        )

        self.success_rate_gauge = Gauge(
            'customer_service_success_rate',
            'Success rate of problem resolution',
            ['channel']
        )

    def record_message(self, channel: str, intent: str):
        """记录消息指标"""
        self.message_counter.labels(channel=channel, intent=intent).inc()

    def record_response_time(self, channel: str, response_time: float):
        """记录响应时间"""
        self.response_time_histogram.labels(channel=channel).observe(response_time)

    def update_active_sessions(self, count: int):
        """更新活跃会话数"""
        self.active_sessions_gauge.set(count)

    def update_success_rate(self, channel: str, rate: float):
        """更新成功率"""
        self.success_rate_gauge.labels(channel=channel).set(rate)


class HealthChecker:
    """健康检查器"""

    def __init__(self, maas_client: HuaweiMaaSClient):
        self.maas_client = maas_client
        self.health_status = {
            "overall": "healthy",
            "components": {
                "maas_api": "unknown",
                "database": "unknown",
                "cache": "unknown"
            },
            "last_check": time.time()
        }

    async def check_maas_api(self) -> bool:
        """检查MaaS API健康状态"""
        try:
            result = self.maas_client.get_model_info()
            if "error" not in result:
                self.health_status["components"]["maas_api"] = "healthy"
                return True
            else:
                self.health_status["components"]["maas_api"] = "unhealthy"
                return False
        except Exception:
            self.health_status["components"]["maas_api"] = "unhealthy"
            return False

    async def check_database(self) -> bool:
        """检查数据库连接"""
        try:
            # 模拟数据库检查
            # 实际实现中应该执行简单的数据库查询
            self.health_status["components"]["database"] = "healthy"
            return True
        except Exception:
            self.health_status["components"]["database"] = "unhealthy"
            return False

    async def check_cache(self) -> bool:
        """检查缓存系统"""
        try:
            # 模拟Redis检查
            self.health_status["components"]["cache"] = "healthy"
            return True
        except Exception:
            self.health_status["components"]["cache"] = "unhealthy"
            return False

    async def perform_health_check(self) -> Dict[str, Any]:
        """执行全面健康检查"""
        checks = [
            self.check_maas_api(),
            self.check_database(),
            self.check_cache()
        ]

        results = await asyncio.gather(*checks, return_exceptions=True)

        # 更新整体健康状态
        if all(results):
            self.health_status["overall"] = "healthy"
        elif any(results):
            self.health_status["overall"] = "degraded"
        else:
            self.health_status["overall"] = "unhealthy"

        self.health_status["last_check"] = time.time()
        return self.health_status


# 启动监控服务
def start_monitoring():
    """启动监控服务"""
    # 启动Prometheus指标服务器
    start_http_server(8001)
    print("✅ Prometheus指标服务器已启动，端口: 8001")

    # 定期健康检查
    def periodic_health_check():
        health_checker = HealthChecker(maas_client)
        while True:
            asyncio.run(health_checker.perform_health_check())
            time.sleep(60)  # 每分钟检查一次

    health_thread = threading.Thread(target=periodic_health_check)
    health_thread.daemon = True
    health_thread.start()

    print("✅ 健康检查服务已启动")
```

## 技术难点与解决方案

### 9.1 长文本处理优化

在处理超长企业文档时，我们采用了分层检索策略：

```
class HierarchicalRetriever:
    """分层检索器"""

    def __init__(self, chunk_sizes=[500, 1000, 2000]):
        self.chunk_sizes = chunk_sizes
        self.retrievers = {}

        for size in chunk_sizes:
            # 为不同粒度创建独立的检索器
            self.retrievers[size] = self._create_retriever(size)

    def retrieve_multi_granularity(self, query: str) -> List[Dict]:
        """多粒度检索"""
        all_results = []

        for chunk_size in self.chunk_sizes:
            results = self.retrievers[chunk_size].retrieve_documents(query, top_k=3)

            # 添加粒度标识
            for result in results:
                result['chunk_size'] = chunk_size
                result['granularity'] = self._get_granularity_level(chunk_size)

            all_results.extend(results)

        # 融合和去重
        return self._merge_and_deduplicate(all_results)

    def _get_granularity_level(self, chunk_size: int) -> str:
        """获取粒度级别"""
        if chunk_size <= 500:
            return "细粒度"
        elif chunk_size <= 1000:
            return "中粒度"
        else:
            return "粗粒度"
```

### 9.2 多模态内容处理

![](img/881cfca61ca98d1e6e930339f7bbc0ac.png)
