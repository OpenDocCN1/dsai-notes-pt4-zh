
![](img/d22db9bec14439113dfc72cbb124dc34.png)

# 使用 Ragas 评估

存储在 MyScaleDB 中的跨度数据可以用来分析和评估 RAG pipeline 与 Ragas，这是一个流行的开源 RAG 评估框架。

以下 Python 代码演示了如何通过评估检索的上下文和生成的答案来使用 Ragas 评分跟踪：

```
import os
from datasets import Dataset
from ragas import evaluate
from ragas.metrics import faithfulness, answer_relevancy, context_utilization
from clickhouse_connect import get_client
 
 
def score_with_ragas(query, chunks, answer):
  test_dataset = Dataset.from_dict({&quot;question&quot;: [query], &quot;contexts&quot;: [chunks], &quot;answer&quot;: [answer]})
  result = evaluate(test_dataset, metrics=[faithfulness, answer_relevancy, context_utilization])
  return result
 
 
def evaluate_trace(question, topk, client, database_name, table_name):
  trace_id, answer = client.query(
    f&quot;SELECT TraceId, SpanAttributes[&#x27;output&#x27;] as Answer FROM {database_name}.{table_name} WHERE SpanAttributes[&#x27;question&#x27;] = &#x27;{question}&#x27; AND ParentSpanId = &#x27;&#x27;&quot;
  ).result_rows[0]
 
  span_dict = client.query(
    f&quot;SELECT SpanAttributes FROM {database_name}.{table_name} WHERE TraceId = &#x27;{trace_id}&#x27; AND SpanKind = &#x27;retriever&#x27;&quot;
  ).result_rows[0][0]
 
  contexts = [span_dict.get(f&quot;documents.{i}.content&quot;) for i in range(topk)]
 
  print(score_with_ragas(question, contexts, answer))
 
 
test_question = &quot;where did harrison work&quot;
client = get_client(
  host=os.getenv(&quot;MYSCALE_HOST&quot;),
  port=int(os.getenv(&quot;MYSCALE_PORT&quot;)),
  username=os.getenv(&quot;MYSCALE_USERNAME&quot;),
  password=os.getenv(&quot;MYSCALE_PASSWORD&quot;),
)
evaluate_trace(test_question, 4, client, &quot;otel&quot;, &quot;otel_traces&quot;)
```

运行示例后，你可以使用 Ragas 提供的分数评估你的 RAG pipeline 性能。

![](img/94b377a233a74e596516150df81d1aeb.png)

# 结论

MyScale Telemetry 为改善 LLM 应用程序的可观测性和评估提供了一个强大、开源的解决方案。通过与 LangChain Callbacks 无缝集成，它捕获详细的跟踪数据并将其存储在 MyScaleDB 中，使诊断问题、优化性能和理解应用程序行为变得更加容易。

Grafana 中的 MyScale 跟踪仪表板提供了这些跟踪数据的清晰可视化，帮助您有效地监控和调试你的 LLM 应用程序。关键洞察包括函数执行时间、输入和输出跟踪、DB 检索器返回、提示信息、`ChatOpenAI` 输出和令牌使用情况。

此外，将 Ragas 与 MyScale Telemetry 集成允许全面评估 RAG pipeline。使用存储在 MyScaleDB 中的跟踪数据，Ragas 可以评估指标，如忠实度、答案相关性和上下文利用，确保高质量的结果和持续改进。

为什么不尝试使用 MyScale Telemetry 和 Grafana 仪表板，充分利用这些强大的工具呢？如果有任何问题或需要进一步的帮助，请随时联系我们的 MyScale Discord支持团队。
