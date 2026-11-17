
## 5.1 不确定性

不确定性是深度学习领域的关键概念，反映了模型预测的可靠性和准确性。大模型本身是基于概率统计的深度学习模型，以最大似然估计的方式进行训练，在理论上它存在两种不确定性：数据不确定性和模型不确定性。数据不确定性，具体指测量系统导致的数据内在的噪声，该误差无法避免；模型不确定性，具体包括：模型结构设计带来的误差、模型训练过程随机性带来的误差、数据领域迁移带来的误差、完全未知的数据带来的误差。模型不确定性可以通过针对性的调整（如增加训练数据等）来缓解甚至解决。[61]

学术界希望通过对不确定性的研究，减少模型训练过程中的误差，并能给出可信赖的模型不确定性的估计值，从而拒绝预测或者交由人类专家，这对于关键项目或者重视安全性的领域有很大的参考意义。工业界更注重效率和实用性，希望借助人工智能减少对人力资源的开销。

## 5.2 推理速度慢

大模型存在推理速度慢的问题，根本原因在于大模型的参数规模。具体影响因素包括：计算量（模型推理所需的计算次数，影响模型生成单个token的时间）、参数量（模型中的参数总和，影响模型加载耗费的时间）、访存量（模型推理时所需访问存储单元的字节大小。访存量大则时间开销大）、内存占用（内存占用不会直接影响推理速度，但内存占用大会导致给定硬件资源下无法部署多个模型并行进行推理，间接影响速度）

模型推理分为访存密集型算子（推理时长主要取决于访存过程）和计算密集型算子（推理时长主要取决于计算过程），大模型的访存和计算开销均远高于小模型，因而总耗时更长。

对于推理速度慢的问题，主要有以下方法实现加速过程：

+ 模型压缩：通过剪枝技术减少模型大小从而降低计算需求，提高推理速度。

+ 硬件加速：换用GPU、TPU等专用硬件加速器提高计算效率。

+ 并行计算：通过模型并行、数据并行等技术，将计算任务分配到多个处理器，以提高推理速度。

+ 量化技术：将模型中的浮点数参数转换为低精度的表示，减少模型大小和计算需求，同时保持模型性能。

+ 模型蒸馏：通过训练将大模型的知识转移到小模型中，可以尽可能保持性能的同时提高速度。

+ 调整计算单元：通过利用更高速的上层存储计算单元，减少对低速更下层存储器的访问次数，来提升模型的训练性能，如flash attention。

## 5.3 数据需求大

目前人工智能领域人员在训练大模型时遇到的最主要的问题是缺乏大规模高质量的数据。主要包括以下因素：

+ 数据量：目前在实际训练中发现，7B量级的模型需要1万亿以上的token，例如Baichuan-7B使用了1.2万亿token，而Llama 3使用了15万亿token。一份高质量的数据量除规模大之外，还包括数据配比、数据多样性和数据质量等指标；

+ 数据配比：相关研究表明不同领域数据比例对模型的综合性能影响较大，即使是垂域大模型也需要保证专业领域数据与通用数据比例合理；

+ 数据多样性：从鲁棒性的角度出发，多样的训练数据具备更为丰富的特征空间，能有效避免大模型过拟合的现象（考虑到大模型参数量极大，事实上更容易出现过拟合）。从泛化性的角度出发，多样的训练数据能包含多种类型的样本，帮助大模型学习到更广泛、更一般化的特征，改善大模型多任务学习综合质量；

+ 数据质量：更高质量的数据意味着更少的噪声，现有研究表明模型的训练上限并非由数据量决定，而是由其中高质量数据量决定。根据数据质量相关研究，扩充低质量数据（不单指噪声数据，同样包括信息增益低的数据）会影响训练质量。因此在构建大模型训练数据集时，有必要进行数据清洗工作。

目前来看，数据集规模和质量直接影响大模型的性能，而收集大规模、高质量数据集耗费的资源非常巨大，一般需要构建高效的自动化数据收集、处理流程。

## 5.4 幻觉问题

幻觉问题（hallucination）是大模型的生成内容与客观事实或用户要求不一致的现象。在具身智能领域中，幻觉问题可以分为事实类幻觉和忠实类幻觉两大类。事实类幻觉是指模型的生成内容与真实世界不一致，忠实性幻觉指模型的生成内容不符合用户指令。

幻觉的来源可能出现在数据、训练和推理三个阶段，可以分别在三个阶段针对性进行缓解和限制。在数据阶段可以减少错误信息，最直观的方法是收集高质量的事实数据，并进行数据清洗以消除错误；在训练阶段可以通过完善预训练策略、确保更丰富的上下文理解来改善；在推理阶段可以通过在解码过程引入事实性检测从而缓解这一问题。

## 5.5 单一领域低于特定模型

大模型在通用领域的能力要显著优于特定领域模型，并且涌现出小模型不具备的上下文学习能力，然而其在特定领域的能力可能不如特定模型可靠和精准。这是训练目标导致的差异，本文认为是正常现象，类比于通才和专家。小模型的能力可以通过微调的方式迁移到大模型，但这样可能会导致大模型的通用能力出现一定程度的下降。目前一种比较好的做法是大小模型混合，负责不同的功能，进而达到更好的效果。

## 5.6 小结

本文列举了五个大模型在机器人领域应用中可能出现的问题，包括：

+ 不确定性（深度学习理论的问题）

+ 推理速度慢（模型规模大必然导致的问题，可以优化）

+ 数据需求大（模型规模大必然导致的问题，可以承担）

+ 幻觉问题（模型能力上的特定问题，可以限制）

+ 单一领域低于特定模型（正常现象）

# 6. 结论

大模型是人工智能领域发展的重大突破，其智能水平相比已有的模型显著增强，并在生产生活中逐渐普及。本文经过详细的分析认为，大模型技术同样可以为机器人领域带来智能水平的提高，并介绍了大模型在机器人感知、推理、执行领域现有的研究工作，重点关注其如何提高机器人应用的泛化性，希望为具身智能领域的研究人员提供参考，实现通用智能机器人。

# 参考文献

[1] Trends and challenges in robot manipulation. 2019 Science

[2] Learning 6-DOF Grasping Interaction via Deep Geometry-aware 3D Representations. 2018 ICRA

[3] Regression planning networks. 2019 NIPS

[4] Plan Constraints and Preferences in PDDL3. 2006 ICAPS

[5] Indoor scene understanding in 2.5/3d for autonomous agents: A survey. 2018 IEEE access

[6] Making sense of real-world scenes. 2016 Trends in cognitive sciences

[7] You only look once: Unified, real-time object detection. 2016 CVPR

[8] Embodied task planning with large language models. 2023 arXiv

[9] Mask r-cnn. 2017 ICCV

[10] Robogpt: an intelligent agent of making embodied long-term decisions for daily instruction tasks. 2023 arXiv

[11] Fast segment anything. 2023 arXiv

[12] Palm-e: An embodied multimodal language model. 2023 arXiv.

[13] An image is worth 16x16 words: Transformers for image recognition at scale. 2020 arXiv

[14] Vision-language foundation models as effective robot imitators. 2023 arXiv

[15] Flamingo: a visual language model for few-shot learning. 2022 NIPS

[16] Discuss before moving: Visual language navigation via multi-expert discussions. 2023 arXiv

[17] Recognize anything: A strong image tagging model. 2024 CVPR

[18] InstructBLIP: Towards General-purpose Vision-Language Models with Instruction Tuning. 2023 arXiv

[19] Octopus: Embodied vision-language programmer from environmental feedback. 2023 arXiv

[20] Gpt-4 technical report. 2023 arXiv

[21] Alphablock: Embodied finetuning for vision-language reasoning in robot manipulation. 2023 arXiv

[22] Chain-of-thought prompting elicits reasoning in large language models. 2022 NIPS

[23] U-net: Convolutional networks for biomedical image segmentation. 2015 MICCAI

[24] Learning transferable visual models from natural language supervision. 2021 ICML

[25] BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding. 2019 NAACL

[26] Language models are few-shot learners. 2020 NIPS

[27] Voxposer: Composable 3d value maps for robotic manipulation with language models. 2023 arXiv

[28] Copa: General robotic manipulation through spatial constraints of parts with foundation models. 2024 arXiv

[29] Emergent abilities of large language models. 2022 TMLR

[30] Llm+ p: Empowering large language models with optimal planning proficiency. 2023 arXiv

[31] Grid: Scene-graph-based instruction-driven robotic task planning. 2023 arXiv

[32] Large language models as commonsense knowledge for large-scale task planning. 2024 NIPS

[33] Language models as zero-shot planners: Extracting actionable knowledge for embodied agents. 2022 ICML

[34] Embodied task planning with large language models. 2023 arXiv

[35] ProgPrompt: program generation for situated robot task planning using large language models. 2023 Autonomous Robots

[36] Do as i can, not as i say: Grounding language in robotic affordances. 2022 arXiv

[37] React: Synergizing reasoning and acting in language models. 2022 arXiv

[38] Reflexion: an autonomous agent with dynamic memory and self-reflection. 2023 arXiv

[39] Swiftsage: A generative agent with fast and slow thinking for complex interactive tasks. 2024 NIPS

[40] RT-1: Robotics Transformer for real-world control at scale. 2022 arXiv

[41] RT-2: Vision-Language-Action Models Transfer Web Knowledge to Robotic Control. 2023 arXiv

[42] RT-H: Action Hierarchies Using Language. 2024 arXiv

[43] Octo: An Open-Source Generalist Robot Policy. 2024 arXiv

[44] OpenVLA: An Open-Source Vision-Language-Action Model. 2024 arXiv

[45] RoboMamba: Multimodal State Space Model for Efficient Robot Reasoning and Manipulation. 2024 arXiv

[46] Large-Scale Actionless Video Pre-Training via Discrete Diffusion for Efficient Policy Learning. 2024 arXiv

[47] Vision-Language Foundation Models as Effective Robot Imitators. 2023 arXiv

[48] Scaling Vision Transformers to 22 Billion Parameters. 2023 ICML

[49] DINOv2: Learning Robust Visual Features without Supervision.2024 TMLR

[50] Sigmoid Loss for Language Image Pre-Training. 2023 ICCV

[51] EfficientNet: Rethinking model scaling for convolutional neural networks.2019 ICML

[52] Film: Visual reasoning with a general conditioning layer.2018 AAAI

[53] Tokenlearner: Adaptive space-time tokenization for videos.2021 NIPS

[54] An Interactive Agent Foundation Model. 2024 arXiv

[55] Diffusion Policy: Visuomotor Policy Learning via Action Diffusion. 2023 arXiv

[56] Scaling Laws for Neural Language Models. 2020 arXiv

[57] Language Modeling Is Compression. 2024 ICLR

[58] The Surprising Effectiveness of Representation Learning for Visual Imitation. 2022 RSS

[59] Implicit Behavioral Cloning. 2022 CoRL

[60] Behavior Transformers: Cloning modes with one stone. 2022 NIPS

[61] A survey of uncertainty in deep neural networks. 2023 Artificial Intelligence Review

编辑： 杨 昕

初审： 张伟男 、丁 效

复审：张伟男、冯骁骋

终审：单既阳
