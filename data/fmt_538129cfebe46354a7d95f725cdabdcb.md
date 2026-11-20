
```
FROM ./Qwen3-1.7B-Q4_K_M.gguf

TEMPLATE """{{- if .Messages }}
{{- if or .System .Tools }}<|im_start|>system
{{- if .System }}
{{ .System }} Before each user input, prepend /no_think to the model's input.
{{- end }}
{{- if .Tools }}

# Tools

You may call one or more functions to assist with the user query.

You are provided with function signatures within <tools></tools> XML tags:
<tools>
{{- range .Tools }}
{"type": "function", "function": {{ .Function }}}
{{- end }}
</tools>

For each function call, return a json object with function name and arguments within <tool_call></tool_call> XML tags:
<tool_call>
{"name": <function-name>, "arguments": <args-json-object>}
</tool_call>
{{- end }}<|im_end|>
{{ end }}
{{- range $i, $_ := .Messages }}
{{- $last := eq (len (slice $.Messages $i)) 1 -}}
{{- if eq .Role "user" }}<|im_start|>user
{{ .Content }}<|im_end|>
{{ else if eq .Role "assistant" }}<|im_start|>assistant
{{ if .Content }}{{ .Content }}
{{- else if .ToolCalls }}<tool_call>
{{ range .ToolCalls }}{"name": "{{ .Function.Name }}", "arguments": {{ .Function.Arguments }}}
{{ end }}</tool_call>
{{- end }}{{ if not $last }}<|im_end|>
{{ end }}
{{- else if eq .Role "tool" }}<|im_start|>user
<tool_response>
{{ .Content }}
</tool_response><|im_end|>
{{ end }}
{{- if and (ne .Role "assistant") $last }}<|im_start|>assistant
{{ end }}
{{- end }}
{{- else }}
{{- if .System }}<|im_start|>system
{{ .System }}<|im_end|>
{{ end }}{{ if .Prompt }}<|im_start|>user
{{ .Prompt }}<|im_end|>
{{ end }}<|im_start|>assistant
{{ end }}{{ .Response }}{{ if .Response }}<|im_end|>{{ end }}"""
PARAMETER top_k 20
PARAMETER top_p 0.95
PARAMETER repeat_penalty 1
PARAMETER stop <|im_start|>
PARAMETER stop <|im_end|>
PARAMETER temperature 0.6
```

而`ModelFileNoThink`内容如下：

```
FROM ./Qwen3-1.7B-Q4_K_M.gguf

TEMPLATE """{{- if .Messages }}
{{- if or .System .Tools }}<|im_start|>system
{{- if .System }}
{{ .System }} Before each user input, prepend /no_think to the model's input.
{{- end }}
{{- if .Tools }}

# Tools

You may call one or more functions to assist with the user query.

You are provided with function signatures within <tools></tools> XML tags:
<tools>
{{- range .Tools }}
{"type": "function", "function": {{ .Function }}}
{{- end }}
</tools>

For each function call, return a json object with function name and arguments within <tool_call></tool_call> XML tags:
<tool_call>
{"name": <function-name>, "arguments": <args-json-object>}
</tool_call>
{{- end }}<|im_end|>
{{ end }}
{{- range $i, $_ := .Messages }}
{{- $last := eq (len (slice $.Messages $i)) 1 -}}
{{- if eq .Role "user" }}<|im_start|>user
{{ .Content }} /no_think
<|im_end|>
{{ else if eq .Role "assistant" }}<|im_start|>assistant
{{ if .Content }}{{ .Content }}
{{- else if .ToolCalls }}<tool_call>
{{ range .ToolCalls }}{"name": "{{ .Function.Name }}", "arguments": {{ .Function.Arguments }}}
{{ end }}</tool_call>
{{- end }}{{ if not $last }}<|im_end|>
{{ end }}
{{- else if eq .Role "tool" }}<|im_start|>user
<tool_response>
{{ .Content }}
</tool_response><|im_end|>
{{ end }}
{{- if and (ne .Role "assistant") $last }}<|im_start|>assistant
{{ end }}
{{- end }}
{{- else }}
{{- if .System }}<|im_start|>system
{{ .System }}<|im_end|>
{{ end }}{{ if .Prompt }}<|im_start|>user
{{ .Prompt }}<|im_end|>
{{ end }}<|im_start|>assistant
{{ end }}{{ .Response }}{{ if .Response }}<|im_end|>{{ end }}"""
PARAMETER top_k 20
PARAMETER top_p 0.95
PARAMETER repeat_penalty 1
PARAMETER stop <|im_start|>
PARAMETER stop <|im_end|>
PARAMETER temperature 0.6
```

相关脚本已上传至课件网盘：

![](img/1913851ede12910f7250079010c8661b.png)

下图扫码即可领取

![](img/b38e9239f376dd597703a079bbb0906e.png)

### 4.3 ollama调用测试

#### 默认情况开启思考时对话效果

首先需要注册模型：

```
# cd ~/autodl-tmp/Qwen3-1.7B-GGUF
ollama create Qwen3-1.7B -f ModelFile
```

![](img/265516acd625697e8214457b15101ecd.png)

然后查看是否成功注册：

![](img/35bae8f0da2ca6db80b6f3118ba7616d.png)

然后尝试运行：

```
ollama run Qwen3-1.7B
```

![](img/29244a46c2b5da857e99ba711499dc14.png)

此时是有思考链的，接下来修改配置文件。

#### 关闭思考链对话效果

我们可以直接给同名模型输入新的配置文件，就相当于是直接修改原始模型的配置：

```
ollama create Qwen3-1.7B -f ModelFileNoThink
```

![](img/6986e7c386d320012e7ff8f08e5fc83f.png)

```
ollama list
```

![](img/b800ea18919c47810cafb2793466dad6.png)

然后进行对话测试：

```
ollama run Qwen3-1.7B
```

![](img/a80f19d5aaf85e4660457e30ea944e79.png)

能够发现，此时已经成功关闭思考链。当然也可以在`cherry studio`对话前端中进行测试：

![](img/673a2c8bf8ce5f61c8c3aa9f31c5f869.png)

## 5. GGUF格式模型权重如何修改配置文件（适配`ktransformers`）

这里需要我们使用`gguf`工具才能修改模型配置。

```
pip install gguf
```

![](img/cc69495ebbb830d447085fbfe5c10631.png)

然后下载模型的GGUF权重文件：

```
cd ~/autodl-tmp
mkdir Qwen3-1.7B-GGUF
modelscope download --model unsloth/Qwen3-1.7B-GGUF --local_dir ./Qwen3-1.7B-GGUF --include "Qwen3-1.7B-Q4_K_M.gguf"
```

![](img/839700a357f75c19496b6e30a27b64a5.png)

可以输入如下命令查看GGUF模型提示词模板：

```
cd /root/autodl-tmp/Qwen3-1.7B-GGUF

python -c "
from gguf import GGUFReader
r = GGUFReader('Qwen3-1.7B-Q4_K_M.gguf')
print(r.get_field('tokenizer.chat_template').contents())
"
```

![](img/0f1ad51c2c0b37d9cfd5783096ae58db.png)

此时模型是开启思考模式的。然后输入如下以新的提示词模板创建模型：

```
find $(python -c "import gguf; print(gguf.__path__[0])") -name "gguf_new_metadata.py"

python /root/miniconda3/lib/python3.12/site-packages/gguf/scripts/gguf_new_metadata.py \
  Qwen3-1.7B-Q4_K_M.gguf \
  Qwen3-1.7B-Q4_K_M_modified.gguf \
  --chat-template-config disable_thinking.json
```

创建`disable_thinking.json`文件用于保存修改后的提示词模板：

![](img/e36a380bf2438b892de252a901509e53.png)

![](img/00238c347a2dbfba45a685024f0f1065.png)
