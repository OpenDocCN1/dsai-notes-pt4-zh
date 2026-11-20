
在总结文本的过程中，有意识地忽略掉某些细节，能够显著丰富你的提示工程技巧。本课不仅强调了给出精确与清晰指令的重要性，还为处理这一复杂任务提供了一个框架。通过不断练习，你将能够创建出完美契合你特定需求的总结，使得大语言模型（LLM）的应用更加个性化和高效。

## 课程18：在保持原有框架下的叙述拓展：精确文本编辑的技巧

### 引言

欢迎进入我们关于精确文本编辑的提示工程课程的新一课。本课重点讨论利用大语言模型（LLM）进行文本处理的一个核心技巧——如何在不改变原文特定预设元素的前提下，对文本进行扩展或补充。无论是保持原始文本的场景、主要角色、氛围或其他任何要素，同时为其增加内容，这种能力都是非常重要的。

### 挑战的理解

在不改变原文某些特定方面的同时对其进行扩展，主要挑战在于如何保持细腻的平衡。你需要在丰富故事内容、说明文或其他任何形式的文本的同时，保证新加入的内容自然衔接，不脱离原有的限制。要做到这一点，深入理解你所处理的内容及其相关的约束条件是必不可少的。

以扩展一个设定、情绪和角色都已固定的短故事为例，你的创意必须在这些既定的框架内发挥，使故事的扩展与原始设定既保持一致又逻辑连贯。

### 奇幻场景的扩展

作为示例，让我们看一下一个使用Markdown Prompt Framework（MPF）进行了美观格式化的示例提示：

```
__ASK__
Reprint and extend the provided text without altering certain specified elements.

__CONSTRAINTS__
- Do not change the following elements:
  1. The setting (the ancient bridge).
  2. The main characters (the hero and the shadowy figure).
  3. The mood (tense and ominous).
- Extend the story by adding 100-150 words.
- The extension should introduce a sudden twist in the ongoing confrontation.
- Maintain the original tone and style of the writing.

__CONTEXT__
- The text is a short snippet from a fantasy novel.
- It describes a scene where the hero, upon a mysterious, ancient bridge, faces off against a shadowy figure.
____
Given Text:  
In the heart of the mist-covered bridge, stood the hero, his sword drawn, reflecting the pale moonlight. Across from him, a shadowy figure emerged, its intentions as obscure as its form. The air was heavy with anticipation of the impending duel.
----
```

示例输出：

> In the heart of the mist-covered bridge, stood the hero, his sword drawn, reflecting the pale moonlight. Across from him, a shadowy figure emerged, its intentions as obscure as its form. The air was heavy with anticipation of the impending duel. The bridge, a silent witness to countless battles, seemed to hold its breath for yet another confrontation. The hero, with determination in his eyes, readied himself, knowing the shadowy adversary was unlike any he had faced before. Just as the hero braced for an attack, the ground beneath them trembled, and the ancient stones of the bridge began to glow with an ethereal light.

> Unexpectedly, the shadowy figure halted, its form becoming less opaque, revealing a visage tormented by sorrow rather than malice. "I am no enemy," the figure spoke, its voice echoing with a haunting melody that seemed to resonate with the very essence of the bridge. "The real foe lies beyond, seeking to destroy what I've sworn to protect." The hero, taken aback, lowered his weapon slightly, sensing the sincerity in the figure's words. The tension that once filled the air shifted, giving way to a fragile truce formed upon the realization that perhaps their fates were intertwined more closely than they had imagined. Together, they turned their attention to the darkness that lay beyond, where the true threat awaited, hidden in the shadows that stretched ominously across the ancient stones.

通过`CONSTRAINTS`部分，我们可以清楚地看到在故事扩展过程中需要维持哪些核心要素。如同从实际结果中所见，这种方法非常有效。

### 总结

不改变特定元素的情况下扩展文本的能力，是一种融合了创造力和自制力的技巧。有效实现这一点，可以极大地提升一个故事或其他文本内容的深度和丰度。通过理解和运用本课程中介绍的策略，你将更加有能力应对使用大语言模型（LLM）进行精确文本编辑的挑战。记住，持续的实践是精通这项技巧的关键，因此，请继续探索不同文本和限制的组合，以完善你的技巧。

## 课程19：精确调整细节以扩展文本

### 引言

欢迎来到精确文本编辑的提示工程新课，本节我们将深入探讨一个非常吸引人的任务——在刻意修改某些特定元素或所谓的`X`时，如何扩展文本。这项任务展示了大语言模型（LLM）在处理和创造内容上的强大能力和灵活性。它不仅能够延续原有的叙述或信息，还能根据用户的具体需求加入精确的改动。本课程旨在提供策略，帮助你实现对文本进行微妙调整而不偏离其原本的方向或意图。

### 掌握在改变某些细节的同时扩展文本的技巧

扩展文本意味着为已有文本增加内容，使其更为丰富或详尽。本任务的一大特色就是有意识地修改某些特定的细节——即所谓的`X`。这些修改可能关乎语调、故事中的角色情感、描述的具体细节或是事实信息等多个方面。挑战在于如何让这些调整自然融入，既保持原文的完整性和连贯性，又能够融合新的方向或元素。

现在，让我们一起看看如何精确地引导一个LLM完成这一任务。

### 分步制定精确的提示设计

- **识别`X`**：首先，明确你想改变的`X`或是特定元素。在提示中具体说明这一点，能帮助LLM准确理解所需的修改范围和性质。
- **设定扩展参数**：然后确定你期望以何种程度和方式扩展文本。这包括可能需要指明的字数、叙事的走向或是添加新的主题或信息。
- **提供背景**：在提示中附上原文或其部分片段至关重要。这为LLM提供了一个出发点，并确保扩展内容与原文保持一致。
- **构建你的提示**：把以上所有因素综合起来，制作出既清晰又简洁的结构化提示。利用Markdown提示框架（MPF）增强提示的清晰度和结构性。

举例：

```
__ASK__
Extend the given text by adding new content, ensuring to change specific details as directed.

__CONSTRAINTS__
- Change the following elements when extending:
  1. The time of day (from night to dawn).
  2. The main character's emotion (from fear to determination).
- The extension should add a narrative shift that reflects these changes, with at least 200 words.
- Keep the genre and the primary story arc consistent with the original.
- Introduce at least one new character who plays a pivotal role in the story progression.

__CONTEXT__
- The original text is from a thriller novel where the protagonist, after evading capture all night in a dense urban environment, finds a momentary refuge.
____
Given Text:  
With the cover of darkness surrounding him, Alex hid in the shadow of an abandoned storefront, the sounds of his pursuers' footsteps fading into the distance. Fear gripped him tightly, knowing that discovery meant certain capture. The night, his only ally, offered scant comfort.
----
```

关键点在于`CONSTRAINTS`部分，这里我们要清晰地勾勒出对模型输出的预期。

### 实践环节

成功地在变更特定细节的同时扩展文本，需要在创造性和精确度之间找到平衡。这展现了LLMs在遵循详细参数生成与原文融为一体的新内容的能力。无论是进行创意写作、学术研究还是内容创造，掌握这种技巧能够极大地拓展文本操控和增强的可能性。通过尝试不同类型的扩展和调整，练习并完善你的技能，探索通过精确提示设计能达到的广阔天地。

## 课程20：无缝融合文本：精通上下文补全的技术

### 引言

欢迎进入我们关于精准文本编辑提示工程系列的新课。本节课将探讨一个对编程初学者及希望有效使用大语言模型（LLM）的用户极为关键的技巧：指导模型填补文本缺失的部分，同时保证新内容自然地适应原有的上下文、风格和叙述节奏。对任何希望以流畅且协调的方式利用LLM生成或调整文本的人来说，掌握这项技能是非常重要的。

### 文本整合的理解

在讨论如何创建达到此目标的提示之前，首先需要明确“文本整合”的含义。文本整合指的是将新内容加入现有文本，使得这部分新增内容仿佛从一开始就属于原文。这一过程需要细致考量文本的语气、风格及其叙述的连贯性。

### 上下文与约束的重要性

一次成功的内容整合，起点是为LLM明确提供上下文和限制。上下文使模型能够理解现有文本的情境、角色和场景。而约束则明确指导模型在补全缺失部分时的可行范围。将两者结合，可以确保模型输出与原文的既定风格和内容紧密匹配，实现真正的无缝融合。

接下来，让我们来看一个基础但缺乏聚焦的尝试：

```
__ASK__
Generate the missing dialogue in the story.

__CONSTRAINTS__
- Fill in the dialogue.

__CONTEXT__
- The setting is a detective story.

[CONTENTS OF THE STORY WITH MISSING PART]
```

这次尝试因为缺少细节而可能导致输出结果过于泛泛或与期望不符。

### 制定精确提示以实现文本的无缝融合

要想无缝地融合文本，你的提示必须明确指出缺失的具体内容及其如何与文本的其他部分保持一致。我们来详细探讨构建高效提示的关键要素：

```
__ASK__
Fill in the missing part of the text.

__CONSTRAINTS__
- Only fill in the specified missing part of the text.
- Do not alter any other part of the original text.
- The filled-in section must seamlessly integrate with the rest of the text, maintaining the existing tone, style, and narrative flow.
- Introduce a key piece of dialogue that foreshadows future events in the story.

__CONTEXT__
- The text is an excerpt from a detective novel.
- The missing part occurs during a crucial conversation between the detective and a mysterious informant.
____
Given Text (with missing part indicated by "___"):  
In the dimly lit alley, Detective Marlowe met with the informant, an elusive figure known only as Raven. "You're playing a dangerous game," Marlowe warned, his voice low and steady. ___ "And that's exactly why you need me," Raven replied, a sly grin spreading across his shadowed face.
----
```

示例输出：

> In the dimly lit alley, Detective Marlowe met with the informant, an elusive figure known only as Raven. "You're playing a dangerous game," Marlowe warned, his voice low and steady. "The pieces on the board are moving faster than you think, and not all of them are what they seem." "And that's exactly why you need me," Raven replied, a sly grin spreading across his shadowed face.

详细设定上下文并明确约束有助于指导模型产生既能填补空缺又显得刻意且流畅的内容。

### 实践应用

经过对精确文本整合的理论和技术的探索，现在是时候把这些知识付诸实践了。掌握此技能的关键在于不断尝试和优化。你可以从简单的文本开始实验，随着对制定能够实现文本无缝融合的提示越来越熟练，逐步提高挑战的复杂度。

记得，我们的目标不只是简单地补充文本空白；更重要的是要维护并丰富原有的叙事。这个技巧不仅在创意写作中非常有用，对于需要修改或扩展已有文本的各种任务也同样适用，比如更新文件、创作一致的续篇内容，或者在进行中的对话系统中形成回应。

课程即将结束！让我们通过一些有趣的练习题来完成最后的冲刺吧。

![](img/db732e6e9dc4b6e7cf08bf9b3a1d755e.png)

# 第五阶段：提示工程的高级技巧深掘

本课程深入探讨了引导语工程的高级技术，旨在装备学习者以创新的策略来设计出更有效的AI引导语。课程内容覆盖从头脑风暴到系统引导语的优化，从思维链技巧到引导语的迭代创建，旨在教授学习者如何构思和制定复杂引导语，进而提升与大型语言模型（LLMs）的互动效果。通过这些高级技术，学习者将能够更好地理解和运用AI语言模型，以解决更复杂的问题和挑战。

## 课程21：系统提示的技巧和秘诀

### 引言

欢迎加入高级提示工程技术课程。这是一门进阶课程，致力于深化你在这一新兴领域中的基础知识，并教授提升技能的高级技巧。我们的起点是学习如何制作优秀的系统消息，但在深入探索之前，我们先回顾一下系统消息的定义。

### 了解系统提示
