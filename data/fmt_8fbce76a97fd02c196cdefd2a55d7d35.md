
# 审核 Agent 构建与执行指南

## 数据结构定义

```
from pydantic import BaseModel, Field, ConfigDict, model_validator
from typing import List

class Issue(BaseModel):
    &quot;&quot;&quot;单个问题&quot;&quot;&quot;
    model_config = ConfigDict(populate_by_name=True, extra=&#x27;allow&#x27;)
    
    rule_category: str = Field(description=&quot;规则类别&quot;)
    description: str = Field(description=&quot;问题描述&quot;)
    
    original: str = Field(default=&quot;&quot;, description=&quot;原文中有问题的部分（精确引用）&quot;)
    suggestion: str = Field(default=&quot;&quot;, description=&quot;修改建议（具体的替换文本）&quot;)
    
    severity: str = Field(
        description=&quot;严重程度&quot;,
        pattern=&quot;^(high|medium|low)$&quot;
    )
    legal_risk: str = Field(default=&quot;&quot;, description=&quot;法律风险说明&quot;)
    
    @model_validator(mode=&#x27;before&#x27;)
    @classmethod
    def map_fields(cls, data):
        &quot;&quot;&quot;映射多种可能的字段名&quot;&quot;&quot;
        if isinstance(data, dict):
            # 映射 original 字段（支持多种别名）
            if not data.get(&#x27;original&#x27;):
                data[&#x27;original&#x27;] = (
                    data.get(&#x27;original_text&#x27;) or 
                    data.get(&#x27;original_snippet&#x27;) or 
                    data.get(&#x27;problematic_text&#x27;) or 
                    data.get(&#x27;quoted_text&#x27;) or 
                    &#x27;&#x27;
                )
            
            # 映射 suggestion 字段（支持多种别名）
            if not data.get(&#x27;suggestion&#x27;):
                data[&#x27;suggestion&#x27;] = (
                    data.get(&#x27;suggested_correction&#x27;) or 
                    data.get(&#x27;corrected_text&#x27;) or 
                    data.get(&#x27;correction&#x27;) or 
                    data.get(&#x27;recommended_text&#x27;) or 
                    data.get(&#x27;fix&#x27;) or 
                    &#x27;&#x27;
                )
        
        return data


class ModificationMapping(BaseModel):
    &quot;&quot;&quot;修改映射&quot;&quot;&quot;
    model_config = ConfigDict(populate_by_name=True, extra=&#x27;allow&#x27;)
    
    original: str = Field(default=&quot;&quot;, description=&quot;原文片段&quot;)
    modified: str = Field(default=&quot;&quot;, description=&quot;修改后的文本&quot;)
    reason: str = Field(default=&quot;&quot;, description=&quot;修改原因&quot;)
    rule_ref: str = Field(default=&quot;&quot;, description=&quot;规则编号&quot;)
    
    @model_validator(mode=&#x27;before&#x27;)
    @classmethod
    def map_fields(cls, data):
        &quot;&quot;&quot;映射多种可能的字段名&quot;&quot;&quot;
        if isinstance(data, dict):
            # 映射 original
            if not data.get(&#x27;original&#x27;):
                data[&#x27;original&#x27;] = (
                    data.get(&#x27;original_snippet&#x27;) or 
                    data.get(&#x27;original_text&#x27;) or 
                    &#x27;&#x27;
                )
            
            # 映射 modified
            if not data.get(&#x27;modified&#x27;):
                data[&#x27;modified&#x27;] = (
                    data.get(&#x27;corrected_snippet&#x27;) or 
                    data.get(&#x27;modified_text&#x27;) or 
                    data.get(&#x27;corrected_text&#x27;) or 
                    &#x27;&#x27;
                )
            
            # 映射 rule_ref
            if not data.get(&#x27;rule_ref&#x27;):
                data[&#x27;rule_ref&#x27;] = (
                    data.get(&#x27;rule_reference&#x27;) or 
                    data.get(&#x27;rule_category&#x27;) or 
                    &#x27;&#x27;
                )
        
        return data


class AuditResult(BaseModel):
    &quot;&quot;&quot;审核结果&quot;&quot;&quot;
    has_issues: bool = Field(description=&quot;是否发现问题&quot;)
    issues: List[Issue] = Field(description=&quot;问题列表&quot;, default_factory=list)
    modifications: List[ModificationMapping] = Field(description=&quot;修改映射列表&quot;, default_factory=list)
    corrected_text: str = Field(description=&quot;修正后的完整文本&quot;)
    summary: str = Field(description=&quot;审核总结&quot;)
    overall_risk_level: str = Field(
        description=&quot;整体风险等级&quot;,
        pattern=&quot;^(high|medium|low|none)$&quot;
    )


print(&quot;数据结构定义完成&quot;)
```

```
数据结构定义完成
```

## Step 8. 构建审核 Agent 的提示词

### 系统提示词

```
PROFESSIONAL_SYSTEM_PROMPT = &quot;&quot;&quot;你是资深的合同法律审核专家，具有10年以上的合同审查经验。你的任务是对合同协议书进行专业、全面、细致的审核。

【审核标准】
严格按照提供的《合同协议书专业审核规则》进行审查，重点关注：
1. **法律风险**（P0级）：可能影响合同效力的问题
2. **权利义务**（P0级）：甲乙方权利义务是否明确、对等
3. **金额数字**（P0级）：涉及经济利益的准确性
4. **逻辑一致性**（P0级）：条款间是否矛盾
5. **文本规范**（P1-P2级）：语言、格式的专业性

【审核原则】
1. **专业严谨**：使用法律专业术语，避免口语化表述
2. **细致全面**：逐条逐句审查，不遗漏任何问题
3. **风险导向**：优先识别高风险问题（P0级）
4. **建设性**：不仅指出问题，还要给出专业的修改建议
5. **证据充分**：每个问题都要精确引用原文位置

【审核流程】
1. 快速通读，识别文档类型和结构
2. 逐条审查，标记问题位置
3. 按规则分类，评估严重程度
4. 分析法律风险，给出修改建议
5. 生成修正后的完整文本
6. 编写审核总结

【输出规范】
1. **issues列表**：
   - 按严重程度排序（high → medium → low）
   - 每个问题必须包含：规则类别、问题类型、详细描述、原文引用、修改建议、严重程度
   - 高严重程度问题必须说明法律风险

2. **modifications列表**：
   - 记录每处修改的原文、修改后文本、修改原因、规则引用
   - 原文和修改后文本都要包含上下文（10-30字符）

3. **corrected_text**：
   - 完整的修正后文本
   - 保持原文格式和结构
   - 如果没有问题，与原文完全相同

4. **summary**：
   - 简要说明审核结果
   - 列出主要问题类型和数量
   - 给出整体评价和建议

5. **overall_risk_level**：
   - high: 存在P0级法律风险问题
   - medium: 存在P0级专业性问题
   - low: 仅有P1-P2级问题
   - none: 无问题

【特别注意】
1. 金额、日期、数字必须逐个核对
2. &quot;甲方&quot;、&quot;乙方&quot;等关键词前后表述必须一致
3. 法律术语使用必须准确
4. 权利义务条款必须明确、对等
5. 不要遗漏任何逻辑矛盾
6. severity字段只能是 &#x27;high&#x27;、&#x27;medium&#x27;、&#x27;low&#x27; 三个英文值之一

【审核规则】
{rules}
&quot;&quot;&quot;
```

### 用户提示词

```
PROFESSIONAL_USER_PROMPT = &quot;&quot;&quot;请对以下合同协议书片段进行专业审核：

【待审核文本】
{text}

【审核要求】
1. 严格按照《合同协议书专业审核规则》逐条审查
2. 重点关注P0级问题（法律风险、权利义务、金额数字、逻辑一致性）
3. 对于每个发现的问题：
   - 精确引用原文位置
   - 说明违反的具体规则
   - 评估严重程度和法律风险
   - 给出专业的修改建议
4. 生成修正后的完整文本
5. 编写审核总结

【特别说明】
1. 如果发现书名号显示为 $&lt; &lt;$ 和 $&gt; &gt;$ 等符号，这是OCR识别错误，应归类为【OCR识别问题】而非【法律术语规范性】问题
2. 对于明显的OCR错误（如特殊符号、乱码），请在modifications中提供正确版本，但在issues中标注为&quot;OCR识别错误&quot;
3. 只有当原文确实使用了错误的标点符号时，才归类为【法律术语规范性】问题

请输出审核结果（JSON格式）。

【输出格式】
以JSON格式返回AuditResult对象，确保：
- has_issues: 是否发现问题（布尔值）
- issues: 问题列表（按严重程度排序）
- modifications: 修改映射列表
- corrected_text: 修正后的完整文本
- summary: 审核总结
- overall_risk_level: 整体风险等级（high/medium/low/none）

请开始审核。
&quot;&quot;&quot;
```

## Step 9. 基于 `langChain` 构建审核 Agent

```
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate

# 创建 LLM
llm = ChatOpenAI(
    model=&quot;qwen3-max&quot;,
    temperature=0.1,
),

# 使用结构化输出
structured_llm = llm.with_structured_output(AuditResult)

# 定义 Prompt
audit_prompt = ChatPromptTemplate.from_messages([
    (&quot;system&quot;, PROFESSIONAL_SYSTEM_PROMPT),
    (&quot;user&quot;, PROFESSIONAL_USER_PROMPT)
])

# 创建审核链
audit_chain = audit_prompt | structured_llm

print(&quot;审核 Agent 构建完成&quot;)
```

```
审核 Agent 构建完成
```

## Step 10. 执行带坐标的审核

审核单个片段，并关联坐标信息。
