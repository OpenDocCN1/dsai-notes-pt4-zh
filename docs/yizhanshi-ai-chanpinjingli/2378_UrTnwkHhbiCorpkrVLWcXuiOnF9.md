
# 01🈷️10日

> 来源：[`https://v11enp9ok1h.feishu.cn/docx/PIMYdEi3ooyVcsxmT45cRHtsnZd`](https://v11enp9ok1h.feishu.cn/docx/PIMYdEi3ooyVcsxmT45cRHtsnZd)

> 🔔AI情报助手午报「01月10日」

> ✨✨✨✨✨✨✨✨

## 1️⃣ 🧬🌏 陈丹琦团队降本大法又来了：数据砍掉三分之一，性能却完全不减

+ 陈丹琦团队通过引入元数据，实现了大模型预训练效率的提升，同时不增加额外的计算成本。
+ 新预训练方法`MeCo`（Metadata Conditioning then Cooldown）在不同模型规模下均能提升性能，减少数据使用量。
+ `MeCo`方法通过使用元数据加速预训练，并在冷却阶段提升下游性能，实验显示其性能与使用更多数据的标准预训练相当。
+ 该研究开启了引导语言模型的新方法，例如使用特定元数据提高特定任务性能。
+ 研究证明了`MeCo`与不同类型的元数据兼容，能够整合多种元数据以提升模型性能。

关键词：元数据，预训练，大模型，性能提升，兼容性

🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/R8zLXfp6Z3We3_JFDpo7DQ)

## 2️⃣ 🧬 从`ROPE`到`Yarn`，一条通用公式速通长文本大模型中的位置编码

+ 本文介绍了从`ROPE`到`Yarn`的一系列长文本位置编码技术，旨在提高大型语言模型处理长文本的能力。
+ `ROPE`作为一种相对位置编码方法，通过旋转变换将位置索引转化为模型内部的旋转信息。
+ `Position Interpolation (PI)`通过均匀拉伸位置索引到预训练窗口内，简单有效但可能导致高频信息丢失。
+ `NTK-Aware Interpolation`引入动态调整，保持频率参数不变，介于直接外推和线性内插之间的平滑方法。
+ `Yarn`结合温度缩放和`NTK-by-parts`插值技术，全面提升长文本外推能力，尤其在128K长度性能上表现出色。

关键词：位置编码，长文本处理，ROPE，NTK-Aware Interpolation，Yarn

🔗 文中引用的链接

+ [知乎专栏](https://zhuanlan.zhihu.com/p/15311461897)

## 3️⃣ 🧠 千问LLM：在家也能养“大脑”：本地运行大型语言模型的奇妙之旅

+ 本地部署大型语言模型（LLM）已成为现实，不再受限于硬件和复杂的部署流程。
+ `Ollama`客户端简化了模型的获取和运行，使得个人电脑也能拥有强大的AI助手。
+ 通过简单的命令行操作，用户可以激活本地大脑并开始对话，`Ollama`客户端支持多种模型。
+ `Llama Stack`框架提供了一个集成环境，支持`Agent`、`Memory`、`Inference`等功能，使得本地LLM部署更加专业。
+ 实际案例展示了如何使用`Llama Stack`和`Ollama`进行本地LLM部署，包括配置参数、运行模型和编写推理脚本。

关键词：本地部署，大型语言模型，Ollama客户端，Llama Stack，人工智能

🔗 文中引用的链接

+ [`Ollama GitHub`](https://github.com/ollama/ollama)
+ [`Ollama模型库`](https://ollama.com/library)
+ [`Llama Stack文档`](https://llama-stack.readthedocs.io/en/latest/introduction/index.html)

## 4️⃣ 🌏 AI眼镜在CES井喷！中国厂商霸场，2个方向4大技术最火

+ CES 2025上AI眼镜成为中国厂商展示的重点，雷鸟创新、`Rokid`等品牌备受瞩目。
+ AI眼镜分为AI拍摄眼镜和AI+AR眼镜两大类，轻量化和隐私显示成为趋势。
+ `Halliday Glasses`采用微投影技术，实现隐形显示，而`Rokid`等品牌采用光波导方案。
+ AI眼镜技术迭代聚焦于拍摄能力、轻量化、小型化和定制化，提升AI体验。
+ 2025年AI眼镜市场竞争加剧，中国厂商在产品迭代和技术创新上展现出竞争力。

关键词：AI眼镜，AR技术，轻量化，隐私显示，技术创新

🔗 文中引用的链接

+ [CES 2025 AI眼镜报道](https://mp.weixin.qq.com/s/kC7kRaHzzK_bU3rX7xGXdQ)

## 5️⃣ 🌏 CES对话联想副总裁：4年后AI PC含量100%！“交卷”之年已来

+ 联想在CES 2025展示了AI PC新品，包括全球首款卷轴屏AI PC `ThinkBook Plus Gen 6`。
+ 联想探索AI在硬件形态上的新应用，如智能可穿戴设备套装`AI Travel Set`等。
+ 联想认为AI PC将推动几乎所有配件的更替，AI功能最终会无处不在。
+ 联想AI PC内置个人智能体`AI Now`和`小天`，强调用户个人知识库保存在设备本地，保护隐私。
+ 联想预计AI PC普及率在未来几年内将达到40%-50%，甚至高达80%。

关键词：AI PC，联想，CES 2025，个人智能体，AI Now

🔗 文中引用的链接

+ [智东西报道](https://mp.weixin.qq.com/s/g8DYIZcEa1uEHzO7pKI9zA)

## 6️⃣ 🌏 Nature Physics 信息热力学经典综述：通往信息的物理本质

+ 本文是2015年发表于`Nature Physics`的经典综述，介绍了基于随机热力学和涨落定理的信息热力学新理论框架。
+ 文章探讨了信息的物理本质，以及如何评估信息操作的热力学代价，如测量、擦除、复制和反馈。
+ 提及麦克斯韦妖和西拉德引擎，讨论了熵与信息之间的关系，以及信息对第二定律的影响。
+ 介绍了非平衡统计物理读书会，旨在探讨统计物理学的前沿进展。
+ 文章还涉及了信息处理的热力学方面，包括压缩、算法复杂性、逻辑可逆性和校对过程。

关键词：信息热力学，涨落定理，随机热力学，麦克斯韦妖，西拉德引擎

🔗 文中引用的链接

+ [论文地址](https://www.nature.com/articles/nphys3230)
