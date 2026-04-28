# 05-API接口规范-v1.0

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Lh6UdsOWFoXXAexl73VcgeAwnKd](https://kq4b3vgg5b.feishu.cn/docx/Lh6UdsOWFoXXAexl73VcgeAwnKd)

# API 接口规范 v1.0

> 适用场景：企业内部知识库 / 客服智能问答 / 技术文档检索 / 行业资料分析

> 项目阶段：后端开发 → 前后端联调阶段

> 版本号：v1.0

> 更新日期：2026-04

> 来源：赋范大模型技术社区 · 多模态 RAG 项目实战课

---

## 基础信息

```
Base URL: http://localhost:8000/api/v1
Content-Type: application/json
认证方式: Bearer Token (JWT)
```

**统一返回格式**：

```
{
    &quot;code&quot;: 0,
    &quot;message&quot;: &quot;success&quot;,
    &quot;data&quot;: {},
    &quot;request_id&quot;: &quot;req_xxx&quot;
}
```

---

## 一、文档管理接口

### 1.1 上传文档

```
POST /documents/upload
Content-Type: multipart/form-data
```

**请求参数**：

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| file | File | ✅ | 支持 .pdf / .jpg / .png / .txt / .md |
| tags | string[] | ❌ | 文档标签，用于过滤检索 |
| description | string | ❌ | 文档描述 |

**响应**：

```
{
    &quot;code&quot;: 0,
    &quot;message&quot;: &quot;success&quot;,
    &quot;data&quot;: {
        &quot;file_id&quot;: &quot;doc_1714567890_abc123&quot;,
        &quot;file_name&quot;: &quot;architecture_spec.pdf&quot;,
        &quot;file_size&quot;: 1048576,
        &quot;file_type&quot;: &quot;pdf&quot;,
        &quot;task_id&quot;: &quot;task_1714567890_xyz789&quot;,
        &quot;status&quot;: &quot;pending&quot;,
        &quot;upload_time&quot;: &quot;2026-04-21T10:30:00Z&quot;
    }
}
```

**说明**：上传后自动触发 Bridge Pipeline 处理，通过 task_id 轮询处理状态。

---

### 1.2 获取文档信息

```
GET /documents/{file_id}
```

**响应**：

```
{
    &quot;code&quot;: 0,
    &quot;message&quot;: &quot;success&quot;,
    &quot;data&quot;: {
        &quot;file_id&quot;: &quot;doc_1714567890_abc123&quot;,
        &quot;file_name&quot;: &quot;architecture_spec.pdf&quot;,
        &quot;file_size&quot;: 1048576,
        &quot;file_type&quot;: &quot;pdf&quot;,
        &quot;description&quot;: &quot;后端架构规范文档&quot;,
        &quot;tags&quot;: [&quot;architecture&quot;, &quot;backend&quot;],
        &quot;upload_time&quot;: &quot;2026-04-21T10:30:00Z&quot;,
        &quot;processing_status&quot;: &quot;completed&quot;,
        &quot;stats&quot;: {
            &quot;chunk_count&quot;: 42,
            &quot;node_count&quot;: 15,
            &quot;edge_count&quot;: 23,
            &quot;image_count&quot;: 3
        }
    }
}
```

---

### 1.3 获取文档列表

```
GET /documents?page=1&amp;page_size=20&amp;tags=architecture&amp;status=completed
```

**Query 参数**：

| 参数 | 类型 | 默认值 | 说明 |
| --- | --- | --- | --- |
| page | int | 1 | 页码 |
| page_size | int | 20 | 每页数量，最大 100 |
| tags | string | - | 按标签过滤，多个用逗号分隔 |
| status | string | - | 按处理状态过滤 |
| file_type | string | - | 按文件类型过滤（pdf/image/text） |

**响应**：

```
{
    &quot;code&quot;: 0,
    &quot;message&quot;: &quot;success&quot;,
    &quot;data&quot;: {
        &quot;total&quot;: 56,
        &quot;page&quot;: 1,
        &quot;page_size&quot;: 20,
        &quot;items&quot;: [
            {
                &quot;file_id&quot;: &quot;doc_001&quot;,
                &quot;file_name&quot;: &quot;...&quot;,
                &quot;processing_status&quot;: &quot;completed&quot;,
                &quot;upload_time&quot;: &quot;2026-04-21T10:30:00Z&quot;
            }
        ]
    }
}
```

---

### 1.4 删除文档

```
DELETE /documents/{file_id}
```

**说明**：删除文档同时删除其所有关联数据（向量、知识图谱节点、任务记录）。此操作不可逆。

**响应**：

```
{
    &quot;code&quot;: 0,
    &quot;message&quot;: &quot;文档已删除&quot;,
    &quot;data&quot;: {
        &quot;file_id&quot;: &quot;doc_001&quot;,
        &quot;deleted_at&quot;: &quot;2026-04-21T11:00:00Z&quot;
    }
}
```

---

## 二、任务管理接口

### 2.1 手动启动 Pipeline 任务

```
POST /tasks/start
```

**请求体**：

```
{
    &quot;file_id&quot;: &quot;doc_001&quot;,
    &quot;force_reprocess&quot;: false
}
```

**说明**：通常上传后自动触发，force_reprocess=true 时强制重新处理（即使已 completed）。

---

### 2.2 查询任务状态

```
GET /tasks/{task_id}
```

**响应**：

```
{
    &quot;code&quot;: 0,
    &quot;message&quot;: &quot;success&quot;,
    &quot;data&quot;: {
        &quot;task_id&quot;: &quot;task_001&quot;,
        &quot;file_id&quot;: &quot;doc_001&quot;,
        &quot;status&quot;: &quot;embedding&quot;,
        &quot;progress&quot;: 0.65,
        &quot;current_step&quot;: &quot;正在生成文本向量 (28/42)&quot;,
        &quot;created_at&quot;: &quot;2026-04-21T10:30:00Z&quot;,
        &quot;updated_at&quot;: &quot;2026-04-21T10:31:30Z&quot;,
        &quot;estimated_remaining_seconds&quot;: 45,
        &quot;error_message&quot;: null
    }
}
```

---

### 2.3 取消任务

```
POST /tasks/{task_id}/cancel
```

**说明**：仅可取消 pending 或 parsing 状态的任务，其他状态返回 4001 错误。

---

### 2.4 获取任务结果

```
GET /tasks/{task_id}/result
```

**响应（任务完成时）**：

```
{
    &quot;code&quot;: 0,
    &quot;message&quot;: &quot;success&quot;,
    &quot;data&quot;: {
        &quot;task_id&quot;: &quot;task_001&quot;,
        &quot;status&quot;: &quot;completed&quot;,
        &quot;result&quot;: {
            &quot;chunk_count&quot;: 42,
            &quot;node_count&quot;: 15,
            &quot;edge_count&quot;: 23,
            &quot;image_count&quot;: 3,
            &quot;processing_time_seconds&quot;: 67.3,
            &quot;completed_at&quot;: &quot;2026-04-21T10:31:47Z&quot;
        }
    }
}
```

---

## 三、图谱管理接口

### 3.1 获取文档关联节点

```
GET /graph/nodes?file_id=doc_001&amp;label=TECH&amp;limit=50
```

**Query 参数**：

| 参数 | 类型 | 说明 |
| --- | --- | --- |
| file_id | string | 按来源文档过滤 |
| label | string | 按实体类型过滤（PERSON/ORG/TECH/CONCEPT） |
| limit | int | 返回数量上限，默认 50 |

**响应**：

```
{
    &quot;code&quot;: 0,
    &quot;message&quot;: &quot;success&quot;,
    &quot;data&quot;: {
        &quot;total&quot;: 15,
        &quot;nodes&quot;: [
            {
                &quot;node_id&quot;: &quot;entity_tech_qdrant_001&quot;,
                &quot;label&quot;: &quot;TECH&quot;,
                &quot;name&quot;: &quot;Qdrant&quot;,
                &quot;description&quot;: &quot;向量数据库&quot;,
                &quot;source_file_ids&quot;: [&quot;doc_001&quot;],
                &quot;relation_count&quot;: 4
            }
        ]
    }
}
```

---

### 3.2 获取图谱统计

```
GET /graph/stats
```

**响应**：

```
{
    &quot;code&quot;: 0,
    &quot;message&quot;: &quot;success&quot;,
    &quot;data&quot;: {
        &quot;total_nodes&quot;: 40,
        &quot;total_edges&quot;: 39,
        &quot;nodes_by_label&quot;: {
            &quot;TECH&quot;: 18,
            &quot;CONCEPT&quot;: 12,
            &quot;ORG&quot;: 6,
            &quot;PERSON&quot;: 4
        },
        &quot;edges_by_relation&quot;: {
            &quot;USES&quot;: 15,
            &quot;PART_OF&quot;: 12,
            &quot;RELATED_TO&quot;: 8,
            &quot;IMPLEMENTS&quot;: 4
        },
        &quot;most_connected_nodes&quot;: [
            {&quot;name&quot;: &quot;LangChain&quot;, &quot;degree&quot;: 12},
            {&quot;name&quot;: &quot;Qdrant&quot;, &quot;degree&quot;: 8}
        ]
    }
}
```

---

### 3.3 导出知识图谱

```
GET /graph/export?format=json&amp;file_id=doc_001
```

**Query 参数**：

| 参数 | 类型 | 说明 |
| --- | --- | --- |
| format | string | json / graphml / cytoscape |
| file_id | string | 可选，只导出指定文档的子图 |

**响应**：返回对应格式的文件下载。

---

## 四、QA 问答接口

### 4.1 提交问题

```
POST /qa/query
```

**请求体**：

```
{
    &quot;question&quot;: &quot;LangChain Agent 是怎么和知识图谱结合的？&quot;,
    &quot;session_id&quot;: &quot;session_abc123&quot;,    // 可选，传入则关联历史会话
    &quot;filters&quot;: {                        // 可选，限定检索范围
        &quot;file_ids&quot;: [&quot;doc_001&quot;, &quot;doc_002&quot;],
        &quot;tags&quot;: [&quot;architecture&quot;]
    },
    &quot;options&quot;: {
        &quot;include_sources&quot;: true,        // 是否返回来源引用
        &quot;include_graph_path&quot;: true,     // 是否返回图谱推理路径
        &quot;max_tokens&quot;: 500               // 限制回答最大 token 数
    }
}
```

**响应**：

```
{
    &quot;code&quot;: 0,
    &quot;message&quot;: &quot;success&quot;,
    &quot;data&quot;: {
        &quot;qa_id&quot;: &quot;qa_1714567890_xyz&quot;,
        &quot;session_id&quot;: &quot;session_abc123&quot;,
        &quot;question&quot;: &quot;LangChain Agent 是怎么和知识图谱结合的？&quot;,
        &quot;answer&quot;: &quot;LangChain Agent 通过 createAgent 装饰器注册图谱查询工具...&quot;,
        &quot;confidence&quot;: 0.92,
        &quot;sources&quot;: [
            {
                &quot;chunk_id&quot;: &quot;doc_001_chunk_007&quot;,
                &quot;file_name&quot;: &quot;architecture_spec.pdf&quot;,
                &quot;page_number&quot;: 3,
                &quot;relevant_content&quot;: &quot;Agent 通过 @tool 装饰器注册 graph_search 工具...&quot;
            }
        ],
        &quot;graph_path&quot;: [
            &quot;LangChain Agent → USES → createAgent&quot;,
            &quot;LangChain Agent → CALLS → graph_search&quot;
        ],
        &quot;processing_time_ms&quot;: 1823,
        &quot;created_at&quot;: &quot;2026-04-21T10:35:00Z&quot;
    }
}
```

---

### 4.2 批量问答

```
POST /qa/batch
```

**请求体**：

```
{
    &quot;questions&quot;: [
        {&quot;id&quot;: &quot;q1&quot;, &quot;question&quot;: &quot;问题一&quot;},
        {&quot;id&quot;: &quot;q2&quot;, &quot;question&quot;: &quot;问题二&quot;}
    ],
    &quot;options&quot;: {
        &quot;parallel&quot;: true,               // 并发处理
        &quot;max_concurrent&quot;: 5
    }
}
```

**响应**：返回 batch_id，通过 GET /qa/batch/{batch_id} 轮询结果。

---

### 4.3 获取历史会话

```
GET /qa/sessions/{session_id}?page=1&amp;page_size=20
```

**响应**：

```
{
    &quot;code&quot;: 0,
    &quot;message&quot;: &quot;success&quot;,
    &quot;data&quot;: {
        &quot;session_id&quot;: &quot;session_abc123&quot;,
        &quot;created_at&quot;: &quot;2026-04-21T10:00:00Z&quot;,
        &quot;qa_count&quot;: 8,
        &quot;items&quot;: [
            {
                &quot;qa_id&quot;: &quot;qa_001&quot;,
                &quot;question&quot;: &quot;...&quot;,
                &quot;answer&quot;: &quot;...&quot;,
                &quot;created_at&quot;: &quot;...&quot;
            }
        ]
    }
}
```

---

### 4.4 获取单条问答记录

```
GET /qa/records/{qa_id}
```

**响应**：返回完整的问答记录，包含 sources 和 graph_path。

---

## 五、健康检查

```
GET /health
```

**响应**：

```
{
    &quot;status&quot;: &quot;healthy&quot;,
    &quot;version&quot;: &quot;1.0.0&quot;,
    &quot;timestamp&quot;: &quot;2026-04-21T10:00:00Z&quot;,
    &quot;services&quot;: {
        &quot;vector_store&quot;: &quot;healthy&quot;,
        &quot;graph_store&quot;: &quot;healthy&quot;,
        &quot;llm&quot;: &quot;healthy&quot;,
        &quot;file_storage&quot;: &quot;healthy&quot;
    }
}
```

---

## 六、接口汇总

| 分组 | 方法 | 路径 | 说明 |
| --- | --- | --- | --- |
| 文档管理 | POST | /documents/upload | 上传文档 |
| 文档管理 | GET | /documents/{file_id} | 获取文档信息 |
| 文档管理 | GET | /documents | 获取文档列表 |
| 文档管理 | DELETE | /documents/{file_id} | 删除文档 |
| 任务管理 | POST | /tasks/start | 手动启动任务 |
| 任务管理 | GET | /tasks/{task_id} | 查询任务状态 |
| 任务管理 | POST | /tasks/{task_id}/cancel | 取消任务 |
| 任务管理 | GET | /tasks/{task_id}/result | 获取任务结果 |
| 图谱管理 | GET | /graph/nodes | 获取图谱节点 |
| 图谱管理 | GET | /graph/stats | 获取图谱统计 |
| 图谱管理 | GET | /graph/export | 导出知识图谱 |
| QA 问答 | POST | /qa/query | 提交问题 |
| QA 问答 | POST | /qa/batch | 批量问答 |
| QA 问答 | GET | /qa/sessions/{session_id} | 获取历史会话 |
| QA 问答 | GET | /qa/records/{qa_id} | 获取问答记录 |
| 系统 | GET | /health | 健康检查 |