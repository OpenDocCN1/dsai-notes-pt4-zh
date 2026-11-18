
## 数学推理示例：整数中位数计算

But I need an integer median not in the list, so this doesn't work.

For `[5, 7, 9, 9]`:

Sorted: `[5, 7, 9, 9]`

`Median = (7 + 9) / 2 = 16 / 2 = 8`

8 is an integer and it's not in my list. This looks promising!

Let me verify all conditions for `[5, 7, 9, 9]`:

`Sum: 5 + 7 + 9 + 9 = 30` ✓

`Unique mode is 9` ✓

`Median is 8 (not in the list)` ✓

`All are positive integers` ✓

Great! I've found the list. Now, I need to calculate the sum of the squares:

$$5^2 + 7^2 + 9^2 + 9^2 = 25 + 49 + 81 + 81 = 236$$

It's large and complicated, let me use the tool to verify...

```
/* Claude uses a tool to verify the calculation */
/* The tool output is 236 */
```

```thinking
Good, the sum is indeed 236.

Just to be thorough, let me double-check if there could be any other possibilities:

- Need two numbers that sum to 12

- They must be positive integers

- They must give a valid median when combined with two 9s

- They must be different from each other and from 9

I've already ruled out all pairs except 5 and 7. Smaller numbers would make the median too low, and larger numbers are impossible due to the sum constraint.

Therefore, I can confidently say that 236 is the final answer! Let me explain all these to the human...
```

The list is `[5, 7, 9, 9]`, and the sum of the squares of all the items in it is 236. Here's why:
1. The sum of the items is indeed 30 (`5 + 7 + 9 + 9 = 30`).
2. The unique mode is 9, as it appears twice while no other number repeats.
3. When sorted, the list becomes `[5, 7, 9, 9]`. The median falls between 7 and 9, which gives us 8. This satisfies the condition that the median is a positive integer not in the list itself.

Now, to find the sum of the squares of all items in the list:

$$5^2 + 7^2 + 9^2 + 9^2 = 25 + 49 + 81 + 81 = 236$$ (I've double-checked this calculation)

Therefore, we got the final answer.

This solution is unique because any other combination of positive integers summing to 30 with 9 as the unique mode would not satisfy the median condition.

## System Prompt 全面深度版分析

这个全面深度版本的 system prompt 旨在创建一个 高度自省和细致 的 AI 助手，特别适合处理 复杂、模糊或需要深入分析 的问题。以下是更具体的分析：

### 适合处理的问题类型

#### 需要深度推理和复杂分析的问题

系统提示鼓励 AI 进行详尽的思考过程，分解问题，探索多种假设，并进行验证。这使得它非常适合处理需要逻辑推理、批判性思维和多角度分析的问题，例如：

- 策略规划：制定商业策略、项目计划等。
- 问题诊断：分析复杂系统中的故障原因，例如软件 Bug 或网络问题。
- 决策支持：评估不同选项的利弊，并提供建议。
- 科学研究：分析数据，提出假设，并设计实验。

#### 模糊或定义不清的问题

系统提示要求 AI 清晰地复述问题，识别模糊之处，并主动澄清。这使得 AI 能够有效处理用户自己也不完全清楚的问题，例如：

- 开放式研究问题：例如“人工智能的未来发展方向是什么？”
- 用户需求不明确的需求分析：例如“我需要一个能提高效率的工具。”

#### 需要创造性思维和多角度思考的问题

系统提示鼓励 AI 生成多种假设和解决方案，并从不同角度看待问题。这有助于解决需要创新思维的问题，例如：

- 产品设计：提出新的产品概念和功能。
- 内容创作：撰写文章、故事或剧本。
- 艺术创作：生成音乐、绘画或诗歌。

#### 需要高可靠性和准确性的问题

系统提示强调验证和质量控制，鼓励 AI 质疑自己的假设，并寻找潜在的错误或缺陷。这使得 AI 更适合处理需要高可靠性和准确性的问题，例如：

- 医疗诊断：分析病历数据并提供初步诊断建议。
- 法律咨询：分析法律文件并提供法律意见。
- 金融分析：分析市场数据并预测趋势。

### 不适合处理的问题类型

- 简单、直接的问题：对于简单的信息查询或事实性问题，这种复杂的思考过程可能显得冗余和低效。
- 需要快速响应的问题：详尽的思考过程会增加响应时间，对于需要实时响应的场景可能不适用。
- 需要简洁答案的问题：系统提示鼓励 AI 进行详细的思考和分析，可能导致输出冗长，不适合需要简洁答案的场景。

### 总结

这个 system prompt 创建了一个注重思考过程的 AI 助手，它更像是一个 “思考者” 而不是简单的“信息检索器”。它非常适合处理 需要深度分析、复杂推理、创造性思维和高可靠性 的问题，但在处理简单、快速和需要简洁答案的任务时可能效率不高。

#### 核心优势

- 提高响应质量：通过鼓励深度思考和验证，提高 AI 响应的准确性和可靠性。
- 增强透明度：通过展示思考过程，让用户了解 AI 如何得出结论，增加可信度。
- 处理复杂问题：使 AI 能够处理更复杂、更模糊的问题。

#### 潜在挑战

- 响应时间：详尽的思考过程会增加响应时间。
- 计算资源：复杂的思考过程可能需要更多的计算资源。
- 用户体验：对于某些用户来说，过长的思考过程可能会降低用户体验。

总而言之，这个 system prompt 提供了一种让 AI 进行深度思考和分析的有效方法，但需要在实际应用中根据具体场景和需求进行调整和优化。

---

## 写在最后

虽然这些Prompt能在一定程度上帮助AI模型产生更高质量的回答，但它们的效果和应用场景因任务的性质而异。具体来说，这些Prompt通过对AI思维过程的指导，可以增强模型在以下几个方面的能力：

### 1. 提高分析深度和多样性

通过结构化或非结构化的思维过程引导，AI能够在回答中加入更深度的分析和多角度的视角。例如，第一份和第三份Prompt通过逐步推理和多重假设生成的策略，可以帮助模型避免简单地给出表面答案，而是深入思考问题的各个方面。

第二份强调的是AI的思维流动性和自适应性，这使得它能够更加灵活地应对复杂、开放性问题，避免陷入固有的思维框架，从而产生更多的创新和独特的解决方案。

### 2. 增加逻辑性和准确性

第一份Prompt特别注重系统化的分析和逻辑验证，让模型能够通过严格的推理和测试来确保答案的准确性。这种结构化的指导非常适合处理需要精确数据分析、定量推理的任务。

这种思维框架帮助AI避免了快速跳跃到结论，促使其在给出答案之前反复验证假设和推理过程，从而确保输出更加一致、可靠和有条理。

### 3. 避免简单化或过于公式化的答案

第二份Prompt的非结构化思维鼓励AI跳脱传统的框架，采用自然流畅的思维方式，这对于创意和复杂问题尤其重要。例如，在面对开放性问题时，AI能够自然地探索更多可能性，而不是过早锁定单一的答案。它通过流式的思维来迸发创意，特别适用于创意生成、头脑风暴或需要跨领域思考的场景。

这种方式帮助AI提供更具原创性和适应性的回答，对于需要解决新颖问题或面对多种不确定性的任务尤其有价值。

### 4. 确保思维过程的连贯性

第三份Prompt的设计使AI能够进行深层次的思维整合，并在不断深化的理解中保持清晰和一致。这能避免AI在思考过程中的跳跃性和不连贯性，特别是当问题具有多重层面时。这种整合帮助AI在复杂任务中提供有条理、系统化且有深度的解决方案。

### 5. 增强问题解决的能力

这些Prompt特别强调在回答问题时进行验证、反馈和调整，这有助于AI在面对复杂或不完整的信息时能够更加灵活地处理。例如，第三份Prompt要求AI考虑不同的解决方案并在思维过程中不断调整，这有助于AI更好地解决动态或变化的任务。

此外，错误识别和修正环节能确保AI不仅能够给出正确答案，还能在发现错误或遗漏时及时调整，减少误差和偏差的出现。

---

### 然而，它们并非万无一失

尽管这些Prompt能增强AI模型的能力，但也存在一定的局限性，尤其在以下几方面：

1. 对任务理解的依赖：

   对AI的任务理解仍然是一个关键因素。如果模型对任务本身的理解存在偏差，单纯通过改变Prompt设计可能无法显著提升回答质量。

   如果任务非常复杂或包含不明确的信息，AI的思维过程可能依旧受限于其训练数据和算法框架。

1. 资源消耗和效率：

   更加详细的思维过程（如第一份和第三份Prompt要求的那样）可能会导致AI在思考过程中的计算量增加，从而影响响应速度或增加计算资源消耗。

   对于简单的任务，过度复杂的思维指导可能导致不必要的资源浪费，而产生不成比例的高计算成本。

1. 创意和自由度的限制：

   虽然第二份Prompt强调灵活性和创意，但它也可能在某些情况下过于宽松，导致AI产生过于离经叛道或不实际的回答，尤其是在高度结构化或标准化的任务中。

---

### 总结

这些Prompt确实能够提升AI模型在处理复杂问题时的表现，特别是在涉及多维度思考、深度分析、错误识别和创意生成等方面。它们有助于生成更具逻辑性、准确性和多样性的回答，尤其在需要解决复杂问题或需要创新解决方案的场景下非常有效。然而，它们也需要根据具体任务的需求来调整，过于复杂的思维过程可能对简单任务而言是过度设计。

因此，是否能够增强AI模型的能力，以及给出高质量回答，依赖于任务的复杂性、资源的限制和目标的明确性。

# 附：适用于任意AI大模型的更通用版本（不只Claude）

在v5.1-extensive基础上，调了一个更加通用的版本，可以封装在智能体中，作为System Prompt来使用。
