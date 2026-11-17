
# 字段说明

## 模型信息

模型元信息文件中描述了模型基础能力和模型连接信息。

基础模型元信息模板见 `backend/conf/model/template/model_template_basic.yaml`。

各模型的元信息模板见 `backend/conf/model/template` 下的对应模型名称文件。

以下为完整的模型配置字段，其中标明了各字段的含义：

| 字段名称 | 是否必选 | 示例 | 参数描述 |
| --- | --- | --- | --- |
| `id` | 是 | `0` | 模型 id |
| `name` | 是 | `test_model` | 模型平台展示名称 |
| `icon_uri` | 否 | `default_icon/doubao_v2.png` | 模型展示图片 uri，uri 为 `docker/volumes/minio` 下的相对路径；模板文件中填写的 uri 已经默认上传，可直接使用。 |
| `icon_url` | 否 | `test_icon_url` | 模型展示图片 url，可配置静态链接，展示时优先使用 url 配置，url 为空时使用 uri |
| `description` | 否 | `-` | 模型默认描述 |
| `description.zh` | 否 | `这是模型描述信息` | 中文版模型描述，用于平台展示 |
| `description.en` | 否 | `This is model description` | 英文版模型描述，用于平台展示 |
| `default_parameters` | 否 | `-` | 模型参数列表，列表中元素整体可以参考模板文件 |
| `default_parameters.name` | 是 | `temperature` | 模型参数名称，枚举值：`temperature`, `top_p`, `top_k`, `max_tokens`, `response_format`, `frequency_penalty`, `presence_penalty` |
| `default_parameters.label` | 是 | `-` | 模型参数平台展示名称 |
| `default_parameters.label.zh` | 是 | `生成随机性` | 模型参数平台展示名称-中文 |
| `default_parameters.label.en` | 是 | `Temperature` | 模型参数平台展示名称-英文 |
| `default_parameters.desc` | 是 | `-` | 模型参数平台展示描述 |
| `default_parameters.desc.zh` | 是 | `temperature`: 调高温度会使得模型的输出更多样性和创新性 | 模型参数平台展示描述-中文 |
| `default_parameters.desc.en` | 是 | `Temperature`: When you increase this value, the model outputs more diverse and innovative content | 模型参数平台展示描述-英文 |
| `default_parameters.type` | 是 | `int` | 字段值类型，枚举值：`int`, `float`, `boolean`, `string` |
| `default_parameters.min` | 否 | `'0'` | （数值类型时）字段最小值 |
| `default_parameters.max` | 否 | `'1'` | （数值类型时）字段最大值 |
| `default_parameters.default_val` | 是 | `-` | 精准 / 平衡 / 创意 / 自定义模式下的默认值 |
| `default_parameters.default_val.default_val` | 是 | `'1.0'` | 自定义模式下的默认值 |
| `default_parameters.default_val.creative` | 否 | `'1.0'` | 创意模式下的默认值 |
| `default_parameters.default_val.balance` | 否 | `'0.8'` | 平衡模式下的默认值 |
| `default_parameters.default_val.precise` | 否 | `'0.3'` | 精准模式下的默认值 |
| `default_parameters.precision` | 否 | `2` | （类型为 `float` 时）精度 |
| `default_parameters.style` | 是 | `-` | 展示类型 |
| `default_parameters.style.widget` | 是 | `slider` | 展示样式，枚举值：`slider`（滑块）, `radio_button`（按钮） |
| `default_parameters.style.label` | 是 | `-` | 归类 |
| `default_parameters.style.label.zh` | 是 | `生成多样性` | 归类标签名称-中文 |
| `default_parameters.style.label.en` | 是 | `Generation diversity` | 归类标签名称-英文 |
| `meta` | 是 | `-` | 模型元信息 |
| `meta.name` | 是 | `test_model_name` | 模型名称，用于记录，不展示 |
| `meta.protocol` | 是 | `test_protocol` | 模型连接协议 |
| `meta.capability` | 是 | `-` | 模型基础能力 |
| `meta.capability.function_call` | 否 | `true` | 模型是否支持 function call |
| `meta.capability.input_modal` | 否 | `["text", "image", "audio", "video"]` | 模型输入支持模态 |
| `meta.capability.input_tokens` | 否 | `1024` | 输入 token 上限 |
| `meta.capability.output_modal` | 否 | `["text", "image", "audio", "video"]` | 模型输出支持模态 |
| `meta.capability.output_tokens` | 否 | `1024` | 输出 token 上限 |
| `meta.capability.max_tokens` | 否 | `2048` | 最大 token 数量 |
| `meta.capability.json_mode` | 否 | `true` | 是否支持 json mode |
| `meta.capability.prefix_caching` | 否 | `false` | 是否支持 prefix caching |
| `meta.capability.reasoning` | 否 | `false` | 是否支持 reasoning |
| `meta.conn_config` | 是 | `-` | 模型连接参数 |
| `meta.conn_config.base_url` | 是 | `https://localhost:1234/chat/completion` | 模型服务基础URL |
| `meta.conn_config.api_key` | 是 | `qweasdzxc` | API密钥 |
| `meta.conn_config.timeout` | 否 | `100` | 超时时间（纳秒） |
| `meta.conn_config.model` | 是 | `model_name` | 模型名称 |
| `meta.conn_config.temperature` | 否 | `0.7` | 默认 `temperature` |
| `meta.conn_config.frequency_penalty` | 否 | `0` | 默认 `frequency_penalty` |
| `meta.conn_config.presence_penalty` | 否 | `0` | 默认 `presence_penalty` |
| `meta.conn_config.max_tokens` | 否 | `2048` | 默认 `max_tokens` |
| `meta.conn_config.top_p` | 否 | `0` | 默认 `top_p` |
| `meta.conn_config.top_k` | 否 | `0` | 默认 `top_k` |
| `meta.conn_config.enable_thinking` | 否 | `false` | 是否启用思考过程 |
| `meta.conn_config.stop` | 否 | `["bye"]` | 停止词列表 |
| `meta.conn_config.openai` | 否 | `-` | OpenAI专用配置 |
| `meta.conn_config.openai.by_azure` | 否 | `true` | 是否使用Azure |
| `meta.conn_config.openai.api_version` | 否 | `2024-10-21` | API版本 |
| `meta.conn_config.openai.response_format.type` | 否 | `text` | 响应格式类型 |
| `meta.conn_config.claude` | 否 | `-` | Claude专用配置 |
| `meta.conn_config.claude.by_bedrock` | 否 | `true` | 是否使用Bedrock |
| `meta.conn_config.claude.access_key` | 否 | `bedrock_ak` | Bedrock访问密钥 |
| `meta.conn_config.claude.secret_access_key` | 否 | `bedrock_secret_ak` | Bedrock密钥 |
| `meta.conn_config.claude.session_token` | 否 | `bedrock_session_token` | Bedrock会话令牌 |
| `meta.conn_config.claude.region` | 否 | `bedrock_region` | Bedrock地区 |
| `meta.conn_config.ark` | 否 | `-` | Ark专用配置 |
| `meta.conn_config.ark.region` | 否 | `region` | 地区 |
| `meta.conn_config.ark.access_key` | 否 | `ak` | 访问密钥 |
| `meta.conn_config.ark.secret_key` | 否 | `sk` | 密钥 |
| `meta.conn_config.ark.retry_times` | 否 | `123` | 重试次数 |
| `meta.conn_config.ark.custom_header` | 否 | `{"key": "val"}` | 自定义请求头 |
| `meta.conn_config.deepseek` | 否 | `-` | Deepseek专用配置 |
| `meta.conn_config.deepseek.response_format_type` | 否 | `text` | 响应格式类型 |
| `meta.conn_config.qwen` | 否 | `-` | Qwen专用配置 |
| `meta.conn_config.qwen.response_format` | 否 | `-` | 返回格式化 |
| `meta.conn_config.gemini` | 否 | `-` | Gemini专用配置 |
| `meta.conn_config.gemini.backend` | 否 | `0` | Gemini 后端配置，`0`：默认，`1`：GeminiAPI，`2`：VertexAI |
| `meta.conn_config.gemini.project` | 否 | `test_project` | GCP Project ID for Vertex AI，`backend=2` 时必填 |
| `meta.conn_config.gemini.location` | 否 | `test_loc` | GCP Location/Region for Vertex AI，`backend=2` 时必填 |
| `meta.conn_config.gemini.api_version` | 否 | `v1beta` | api 版本 |
| `meta.conn_config.headers` | 否 | `-` | http headers |
| `meta.conn_config.timeout_ms` | 否 | `-` | http 超时时间 |
| `meta.conn_config.include_thoughts` | 否 | `true` | 返回是否包含 thinking 内容 |
| `meta.conn_config.thinking_budget` | 否 | `123` | thinking token 消耗预算 |
| `meta.status` | 否 | `1` | 模型状态，`0`: 未配置时的默认状态，等同于 `1`，`1`: 应用中，可使用可新建，`5`: 待下线，可使用不可新建，`10`: 已下线，不可使用不可新建 |

## 配置文件示例

各模型的完整配置示例及字段说明可参考 `backend/conf/model/template/model_template_basic.yaml`。

你也可以参考以下最简配置，大部分字段配置基本相同，差异主要在于 `protocol` 与 `conn_config`。

### Ark 火山方舟
