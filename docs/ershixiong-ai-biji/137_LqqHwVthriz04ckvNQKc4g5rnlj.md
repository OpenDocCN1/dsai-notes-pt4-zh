
# json 格式还能用在哪里，请给我一些案例

> 来源：`https://qianniucity.feishu.cn/docx/Liq6dZBpnomNMWxtRHFcyVWnnjb`

这种复杂的 JSON 格式 Prompt 的核心价值在于能够以高度结构化和精确的方式定义生成内容的多个维度和约束条件。因此，它适用于任何需要精确控制和规范化 AI 输出的场景，尤其是当输出涉及多项独立或相关的要求时。

以下是一些这种 JSON 格式 Prompt 的应用案例：

## 案例 1：生成具有特定属性的游戏角色简介

+   **场景：** 为一个游戏设计项目生成一系列具有详细设定的角色。每个角色都需要符合特定的种族、职业、性格、技能体系等约束。

+   **为什么用 JSON：** 角色设定包含多个不同的属性类别（基本信息、背景、能力、外观等），使用 JSON 可以清晰地定义每个类别下的具体字段和约束范围（例如，种族必须是列表中的一个，力量值必须在某个范围内）。

+   **Prompt 结构示例：**

```
{
  &quot;task&quot;: &quot;Generate Game Character Profile&quot;,&quot;output_format&quot;: &quot;Structured Text Description&quot;,&quot;character_type&quot;: {&quot;race&quot;: [&quot;Human&quot;, &quot;Elf&quot;, &quot;Dwarf&quot;, &quot;Orc&quot;],&quot;class&quot;: [&quot;Warrior&quot;, &quot;Mage&quot;, &quot;Rogue&quot;, &quot;Cleric&quot;],&quot;archetype&quot;: &quot;Heroic&quot;
  },
  &quot;attributes&quot;: {&quot;strength&quot;: {&quot;min&quot;: 10, &quot;max&quot;: 20, &quot;priority&quot;: &quot;high&quot;},
    &quot;intelligence&quot;: {&quot;min&quot;: 8, &quot;max&quot;: 15},
    &quot;dexterity&quot;: {&quot;min&quot;: 12, &quot;max&quot;: 18}
  },
  &quot;background&quot;: {&quot;origin&quot;: &quot;Specify a village or city&quot;,&quot;key_event&quot;: &quot;A significant past event shaping the character&quot;
  },
  &quot;personality_traits&quot;: [&quot;Brave&quot;, &quot;Loyal&quot;, &quot;Independent&quot;],&quot;skills&quot;: [&quot;Sword Fighting&quot;, &quot;Basic Healing&quot;, &quot;Tracking&quot;],&quot;description_style&quot;: &quot;Concise and evocative&quot;,&quot;length_constraints&quot;: &quot;Approximately 200-300 words&quot;
}
```

## 案例 2：生成特定结构的营销邮件草稿

+   **场景：** 需要为即将发布的新产品撰写营销邮件，邮件必须包含固定的章节（比如，引人入胜的主题句、产品亮点、用户利益、行动号召、限时优惠信息）。

+   **为什么用 JSON：** 邮件的结构是固定的，每个章节的内容都需要 AI 根据提供的产品信息来填充。JSON 可以明确定义邮件的各个组成部分以及每个部分需要包含的信息类型。

+   **Prompt 结构示例：**

```
{&quot;task&quot;: &quot;Generate Marketing Email Draft&quot;,&quot;output_format&quot;: &quot;Plain Text Email&quot;,&quot;product_info&quot;: {&quot;name&quot;: &quot;AI Assistant Pro&quot;,&quot;key_features&quot;: [&quot;Automated Scheduling&quot;, &quot;Smart Reminders&quot;, &quot;Seamless Integration&quot;],&quot;benefits&quot;: [&quot;Save Time&quot;, &quot;Improve Productivity&quot;, &quot;Stay Organized&quot;],&quot;target_audience&quot;: &quot;Small Business Owners&quot;
  },&quot;email_structure&quot;: {&quot;subject_line&quot;: &quot;Catchy and Benefit-Oriented&quot;,&quot;introduction&quot;: &quot;Hook user with a common problem&quot;,&quot;product_highlight_section&quot;: &quot;Focus on Key Feature 1 &amp; 2, explain Benefits&quot;,&quot;social_proof_section&quot;: &quot;Mention user testimonials (if available, otherwise skip)&quot;,&quot;call_to_action&quot;: &quot;Clear instruction (e.g., &#x27;Learn More&#x27;, &#x27;Sign Up Now&#x27;)&quot;,&quot;limited_time_offer&quot;: {&quot;active&quot;: true, &quot;details&quot;: &quot;10% off for the first month&quot;}
  },&quot;tone&quot;: &quot;Professional yet enthusiastic&quot;,&quot;length_preference&quot;: &quot;Medium&quot;
}
```

## 案例 3：从非结构化文本中提取结构化数据

+   **场景：** 从大量的新闻报道中提取特定信息，如公司名称、新闻发生日期、主要事件、相关人物。

+   **为什么用 JSON：** 你需要 AI 识别文本中的特定实体类型，并将它们组织成一个标准的结构，而不是简单地复述新闻内容。JSON Schema 可以完美地定义输出数据的结构。

+   **Prompt 结构示例：**

```
{&quot;task&quot;: &quot;Extract Structured Data from Text&quot;,&quot;input_text&quot;: &quot;Paste news article text here...&quot;,&quot;output_format&quot;: &quot;JSON Object&quot;,&quot;extraction_schema&quot;: {&quot;company_name&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;The primary company mentioned&quot;},&quot;event_date&quot;: {&quot;type&quot;: &quot;date&quot;, &quot;description&quot;: &quot;Date the main event occurred, format YYYY-MM-DD&quot;},&quot;main_event_summary&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;A brief summary of the main event (max 50 words)&quot;},&quot;key_individuals&quot;: {&quot;type&quot;: &quot;array&quot;, &quot;items&quot;: {&quot;type&quot;: &quot;string&quot;}, &quot;description&quot;: &quot;List of names of key people involved&quot;},&quot;sentiment&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;enum&quot;: [&quot;Positive&quot;, &quot;Negative&quot;, &quot;Neutral&quot;], &quot;description&quot;: &quot;Overall sentiment towards the company/event&quot;}
  },&quot;instructions&quot;: &quot;Strictly adhere to the provided schema and extract only relevant information.&quot;
}
```

## 案例 4：生成带有特定代码风格和功能的程序代码

+   **场景：** 需要一个 Python 函数，它必须接受特定的参数、执行特定的逻辑、返回特定类型的数据，并且遵循公司内部的代码风格指南（例如，PEP 8 规范，特定的注释风格）。

+   **为什么用 JSON：** 可以详细定义函数的输入/输出、核心逻辑描述、依赖项以及代码风格要求。

+   **Prompt 结构示例：**

```
      {&quot;task&quot;: &quot;Generate Code Snippet&quot;,&quot;language&quot;: &quot;Python&quot;,&quot;output_format&quot;: &quot;Code Block&quot;,&quot;function_details&quot;: {&quot;name&quot;: &quot;calculate_discount&quot;,&quot;parameters&quot;: [
      {&quot;name&quot;: &quot;price&quot;, &quot;type&quot;: &quot;float&quot;, &quot;description&quot;: &quot;Original price&quot;},
      {&quot;name&quot;: &quot;discount_rate&quot;, &quot;type&quot;: &quot;float&quot;, &quot;description&quot;: &quot;Discount rate (0 to 1)&quot;}
    ],&quot;return_type&quot;: &quot;float&quot;,&quot;description&quot;: &quot;Calculates the discounted price based on original price and rate.&quot;,&quot;logic_steps&quot;: [&quot;Validate discount_rate is between 0 and 1&quot;,&quot;Calculate discount amount (price * discount_rate)&quot;,&quot;Subtract discount amount from price&quot;,&quot;Return final price&quot;
    ]
  },&quot;code_style&quot;: {&quot;adhere_to&quot;: &quot;PEP 8&quot;,&quot;add_docstrings&quot;: true,&quot;use_type_hints&quot;: true
  },&quot;error_handling&quot;: &quot;Include basic validation for discount_rate&quot;
}
```

## 案例 5：定义一个复杂的多模态输出（例如，包含文本和图片描述）

+   **场景：** 创建一个包含产品描述和配图要求的电商商品详情。

+   **为什么用 JSON：** 可以将文本描述的要求（长度、重点、风格）与图片生成或选择的要求（主题、风格、构图）结合在一个结构化的指令中。

+   **Prompt 结构示例：**

```
{&quot;task&quot;: &quot;Generate Product Description and Image Spec&quot;,&quot;product_name&quot;: &quot;Handmade Ceramic Mug&quot;,&quot;key_selling_points&quot;: [&quot;Unique Glaze Finish&quot;, &quot;Ergonomic Handle&quot;, &quot;Microwave Safe&quot;],&quot;target_customer&quot;: &quot;Artisan craft lovers&quot;,&quot;text_description_spec&quot;: {&quot;length&quot;: &quot;Approx. 150 words&quot;,&quot;tone&quot;: &quot;Warm and Craftsmanship-focused&quot;,&quot;structure&quot;: [&quot;Highlight uniqueness&quot;, &quot;Mention practicality&quot;, &quot;Suggest usage scenarios&quot;]
  },&quot;image_spec&quot;: {&quot;number_of_images&quot;: 2,&quot;image_1&quot;: {&quot;subject&quot;: &quot;The mug from the side, showing the glaze texture&quot;,&quot;style&quot;: &quot;Natural lighting, focus on detail, soft background&quot;
    },&quot;image_2&quot;: {&quot;subject&quot;: &quot;The mug being held, showing scale and handle feel&quot;,&quot;style&quot;: &quot;Lifestyle shot, warm atmosphere&quot;
    }
  }
}
```

这些案例都展示了如何利用 JSON 格式的层级结构和精确定义，将复杂的、多方面约束的任务清晰地传达给 AI 模型，从而获得更可控、更符合预期的输出。本质上，JSON Prompt 将“告诉 AI 怎么做”的需求，转化为了“给 AI 一个遵循的详细配置或蓝图”。
