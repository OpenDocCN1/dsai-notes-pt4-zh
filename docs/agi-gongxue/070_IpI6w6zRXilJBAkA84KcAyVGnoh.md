# GPU科普（todo）

> 来源：[https://vcn0zlxse9ua.feishu.cn/docx/AeOodaoDdon18DxSBwZcuXqbnHf](https://vcn0zlxse9ua.feishu.cn/docx/AeOodaoDdon18DxSBwZcuXqbnHf)

## 为什么使用GPU?

**大模型（如GPT、**BERT等**）依赖GPU的核心原**因在于其海量参数与计算需求与GPU的并行架构、高吞吐能力高度契合。以下是具体解析：

---

### **1.** **大模型的核心计算特征**

+   **参数量巨大**：如GPT-3有1750亿参数，训练需处理PB级数据。

+   **矩阵运算密集**：神经网络每一层的权重更新涉及大规模矩阵乘法（如[batch_size × hidden_dim]矩阵运算）。

+   **高度并行性**：同一批次（batch）内的样本计算互不依赖，可并行处理。

---

1.  **GPU如何满足大模型的需求？**

#### **(1) 并行计算加速**

+   **海量核心应对矩阵运算**：
GPU的数千个CUDA核心（如NVIDIA A100有6912核心）可同时处理矩阵的多个元素，相比CPU（几十核）提速数十至数百倍。
示例：单个矩阵乘法A×B在GPU上可分解为数千个子任务并行执行。

#### **(2) 高带宽显存（VRAM）**

+   **大模型参数存储**：
训练时模型参数需全部加载到显存中（如GPT-3需数百GB显存）。GPU显存（如HBM2e）带宽可达1.5TB/s以上，远超CPU内存（约50GB/s），避免数据搬运瓶颈。

#### **(3) 专用加速硬件**

+   Tensor Core加速混合精度：
NVIDIA的Tensor Core支持FP16/FP8混合精度计算，提升训练速度（如A100的TFLOPS达312 TFLOPS，远超CPU的1-2 TFLOPS）。
案例：使用Tensor Core训练ResNet-50，速度比纯FP32快3倍。

#### **(4) 软件生态支持**

+   **CUDA与深度学习框架深度集成**：
PyTorch、TensorFlow等框架底层调用CUDA和cuDNN库，自动将计算任务映射到GPU，开发者无需手动优化并行代码。

---

1.  **为什么不用CPU？**

+   **核心数量少**：CPU通常仅几十核，无法并行处理数十亿参数的计算。

+   **内存带宽低**：CPU内存带宽难以支撑大模型参数的高速存取。

+   **计算效率低**：CPU针对通用任务优化，缺乏针对矩阵运算的硬件加速单元。

对比实验：训练BERT模型，8卡GPU集群需1天，同等CPU集群需数月。

---

1.  **GPU在大模型中的具体应用场景**

#### **(1) 训练阶段**

+   **分布式训练**：多GPU通过NVLINK或InfiniBand互联，并行处理数据（数据并行）或模型（模型并行）。
如：Meta训练LLaMA-65B使用2048块A100 GPU。

+   **混合精度训练**：Tensor Core支持FP16/FP32混合计算，节省显存并加速收敛。

#### **(2) 推理阶段**

+   **实时响应需求**：GPU的并行能力支持高并发推理（如ChatGPT同时服务百万用户）。

+   **量化与优化**：通过TensorRT等工具将模型转换为GPU专用格式，进一步提升推理速度。

---

1.  **未来挑战与替代方案**

+   **显存容量限制****：
当前**最**大单卡显存（**如H100 94GB）仍无法容纳万亿参数模型，需依赖模型并行与显存优化技术（如ZeRO、梯度检查点）。

+   **替代硬件探索**：

+   **TPU**：Google专为矩阵运算设计的张量处理器，但生态封闭。

+   **Chiplet与存算一体**：未来可能突破显存带宽瓶颈。

## GPU是什么

GPU（Graphics Processing Unit，图形处理器）是一种专门设计用于高效处理并行计算任务的处理器，尤其在图形渲染、科学计算和人工智能领域表现突出。以下是GPU的核心要点解析：

---

1.  **GPU与CPU的区别**

+   **CPU**（中央处理器）：
具备少量高性能核心（通常4-16核），擅长处理复杂逻辑和顺序任务，如操作系统管理、程序控制等。

+   **GPU**：
拥有成百上千的小型核心（如NVID**IA的C**UDA核心、AMD的流处理器），专为并行计算设计，适合同时处理大量简单任务（如图形像素渲染、矩阵运算）。

---

1.  **GPU的核心优势：并行架构**

+   **SIMD架构**：单指令多数据（SIMD）模式，同一指令同时作用于多个数据点，适合图形渲染（处理数百万像素/顶点）和科学计算（如大规模矩阵乘法）。

+   **高吞吐量**：通过海量核心同时工作，GPU在数据并行任务中速度远超CPU，例如训练神经网络时性能可提升数十倍。

---

1.  **GPU的关键组件**

+   **流处理器/CUDA核心**：执行计算任务的基本单元，数量从数百到上万不等。

+   **显存（VRAM）**：高带宽内存（如GDDR6/HBM），专为快速存取大规模数据设计，显存容量直接影响可处理的数据量。

+   **专用加速单元**：如NVIDIA的Tensor Core（加速AI训练）和RT Core（光线追踪），AMD的Infinity Cache（提升数据访问效率）。

---

1.  **GPU的应用场景**

+   **图形渲染**：游戏、影视特效、3D建模（如DirectX、OpenGL/Vulkan API）。

+   **科学计算**：气候模拟、分子动力学（CUDA/OpenCL编程）。

+   **人工智能**：深度学习训练与推理（TensorFlow/PyTorch框架依赖GPU加速）。

+   **加密货币挖矿**：并行计算哈希函数（现多被ASIC矿机取代）。

+   **数据中心与云计算**：提供大规模算力支持（如NVIDIA A100、AMD Instinct系列）。

---

1.  **主流GPU厂商与技术**

+   **NVIDIA**：

+   **CUDA生态**：行业标准并行计算平台，支持深度学习库（如cuDNN）。

+   架构演进：从Fermi到Ampere，集成Tensor Core和光线追踪技术。

+   **AMD**：

+   **RDNA架构**：专注游戏显卡（如Radeon RX系列）。

+   **CDNA架构**：面向计算（如MI系列加速器），支持ROCm开源生态。

+   **其他**：Intel推出Xe架构（如Arc显卡），进军独立GPU市场。

---

1.  **GPU编程与挑战**

+   **编程模型**：需将任务分解为并行线程（如CUDA的线程块/网格），使用特定语言（CUDA C++、OpenCL）或框架（PyTorch自动优化）。

+   **瓶颈与优化**：

+   **数据传输**：CPU与GPU间通过PCIe总线交换数据，需减少传输延迟。

+   **功耗与散热**：高性能GPU功耗可达数百瓦，需强散热方案。

+   **适用性**：非并行任务（如串行逻辑）难以受益于GPU加速。

---

1.  **未来趋势**

+   **AI与异构计算**：GPU与CPU、FPGA等协同工作，优化AI推理与边缘计算。

+   **光追与实时渲染**：游戏/影视迈向电影级画质（如虚幻引擎5）。

+   **量子计算辅助**：GPU加速量子模拟，推动材料科学、药物研发。

## GPU厂家

#### **一、主流 GPU 厂商及产品**

1.  **NVIDIA（英伟达）**

+   **消费级**：

+   **RTX 4090D****：24GB 显存**，旗舰级游戏性能，售价[约 ](https://product.pconline.com.cn/vga/)[14](http://zh.gluon.ai/chapter_appendix/buy-gpu.html),999 元（华硕 TUF 型号）1317。

+   **RTX 4070 SUPER****：主打 2K/**[4K](https://cloud.tencent.cn/developer/information/gpu%E4%BB%B7%E6%A0%BC) 游戏，售价约 4,799 元15。

+   **RTX 4060**：中端性价比型号，售价约 **2,359 元**（微星 VENTUS 型号）[13](https://product.pconline.com.cn/vga/)。

+   **数据中心级**：

+   **A100**：AI 训练旗舰，官方售价约 **1 万美元****（约 7 万元**人民币），但因禁售及[供](https://www.stcn.com/article/detail/867752.html?u_atoken=ee5e83bcc7ad9a72bc1fa626caa00010&u_asig=bbbf3)[需](https://baijiahao.baidu.com/s?id=1824463713946313042&wfr=spider&for=pc)[紧](https://aliyunyh.com/20049.html)张，中国市场价飙升至 15 万元/颗（2023 年数据）125。

+   **H100****：大模型训**[练](https://baijiahao.baidu.com/s?id=1824463713946313042&wfr=spider&for=pc)[首](https://aliyunyh.com/20049.html)[选，](https://pdf.dfcfw.com/pdf/H3_AP202303201584401192_1.pdf)国际版售价约 3 万美元（约 21 万元人民币），但因供应短缺，黑市价格更高2514。

+   **中国特供版**：

+   **A800**：替代 A100，价格约 **9.5 万元/颗**（2023 年数据）[1](https://www.stcn.com/article/detail/867752.html?u_atoken=ee5e83bcc7ad9a72bc1fa626caa00010&u_asig=bbbf3)。

+   **H800**[：](https://www.stcn.com/article/detail/867752.html?u_atoken=ee5e83bcc7ad9a72bc1fa626caa00010&u_asig=bbbf3)[替](https://baijiahao.baidu.com/s?id=1823078274570367230&wfr=spider&for=pc)代 H100，价格未公开，预计略低于 H100 国际版14。

1.  **AMD**

+   **消费级**：

+   **RX 7900 XTX****：对标 RTX 4080，**售价约 6,000[-](https://www.stcn.com/article/detail/867752.html?u_atoken=ee5e83bcc7ad9a72bc1fa626caa00010&u_asig=bbbf3)[8](http://yikaihuangjinyikaihohosdl.ok.1688.com/shop/www/zhuti/-677075CFD4BFA8.html)[,0](https://cloud.tencent.cn/developer/information/gpu%E4%BB%B7%E6%A0%BC)00 元（性价比优势显著）1615。

+   **数据中心级**：

+   **Instinct MI250X**：性能对标 A100，价格约 **5 万美元/颗**（约 35 万元人民币）[14](https://pdf.dfcfw.com/pdf/H3_AP202303201584401192_1.pdf)。

1.  **国产 GPU**

+   **摩尔线程 S80**：支持国产化设计软件，售价约 **2,000-3,000 元**[8](https://zhuanlan.zhihu.com/p/641475066)[17](http://zh.gluon.ai/chapter_appendix/buy-gpu.html)。

+   **壁仞 BR100**[：](https://zhuanlan.zhihu.com/p/641475066)[1P](http://zh.gluon.ai/chapter_appendix/buy-gpu.html)FLOPS 算力，定价未公开（生成式 AI 与超算领域）817。

+   **景嘉微 JM9 系列**：国产化替代市场，价格未公开[17](http://zh.gluon.ai/chapter_appendix/buy-gpu.html)。

---

#### **二、专业级 GPU**

+   **NVIDIA** RTX A600[0：](https://zhuanlan.zhihu.com/p/623764445)[48](https://pdf.dfcfw.com/pdf/H3_AP202303201584401192_1.pdf)GB 显存，支持多屏 8K 输出，售价约 3-5 万元（影视/工业设计）1114。

---

#### **三、云服务 GPU 租赁价格**

+   **Google Cloud**：

+   **NVIDIA P4**：推理场景，每小时 **0.6 美元**[8](https://zhuanlan.zhihu.com/p/641475066)。

+   **NVIDIA V100**：中等规模训练，每小时 **2.48 美元**[8](https://zhuanlan.zhihu.com/p/641475066)。

+   **AWS/Azure**：

+   **A100/H100 实例**：价格随配置波动，单卡月租约 **3,000-5,000 元**（企业级客户）[12](https://www.jaeaiot.com/news/detail/248.html)。

---

#### **四、价格波动与市场因素**

1.  **供需失衡**：AI 大模型热潮导致 GPU 需求激增，英伟达 A100/H**100 等高端型**[号](https://www.stcn.com/article/detail/867752.html?u_atoken=ee5e83bcc7ad9a72bc1fa626caa00010&u_asig=bbbf3)[供](https://aliyunyh.com/20049.html)应短缺，价格涨幅达 50%-200%15。

1.  **出口管制**：美国对华禁售 A100/H100，导致中国[特](https://www.stcn.com/article/detail/867752.html?u_atoken=ee5e83bcc7ad9a72bc1fa626caa00010&u_asig=bbbf3)[供](https://baijiahao.baidu.com/s?id=1823078274570367230&wfr=spider&for=pc)版 A800/H800 价格水涨船高14。

1.  **国产替代**：国产 GPU 如摩尔线程 S80 价格[稳](https://zhuanlan.zhihu.com/p/641475066)[定，](http://zh.gluon.ai/chapter_appendix/buy-gpu.html)但性能尚无法完全替代英伟达高端型号817。