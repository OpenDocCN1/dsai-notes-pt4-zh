
+   随着事件间距的增加，ChatGPT的F1得分降低。这表明ChatGPT不擅长识别长距离的因果关系。一个异常值是在[25,30)区间内gpt-4的F1得分。这是因为在gpt-4的1000个测试样例中，只有35个例子在[25,30)区间内，导致表现更加随机。然而，所有其他结果都表明，随着事件距离的增加，ChatGPT的表现会下降。

### 4.5 事件密度

如图4所示，我们分析了ChatGPT在ECI任务中处理具有不同数量事件的句子的表现。

![](img/5deacff1e3e6bccf16fb68c35ca7d3c1.png)

图4: ChatGPT在ESC数据集中处理具有不同事件数量的句子的表现。

我们发现：

+   随着事件密度的增加，大多数版本的ChatGPT更倾向于预测事件对为非因果关系。这主要是因为随着事件密度的增加，事件的上下文变得更加复杂，使得捕捉事件之间的关联变得更加困难。

+   随着事件密度的增加，ChatGPT的F1分数下降。这表明ChatGPT不擅长处理涉及多个事件的复杂情况。

### 4.6 因果关系类型

如图5所示，我们分析了ChatGPT在ECI任务中处理具有不同类型因果关系的事件对的准确性：1）显式因果，指的是句子中由因果提示词（例如，“lead to”）明确触发的因果关系；2）隐式因果，指的是未使用因果提示词表达的因果关系。

![](img/6952e1b2282339ae4fe1ba0d6b8a7fbc.png)

图5: 在ESC数据集中，ChatGPT在不同类型因果关系的事件对上的表现。

可以观察到：

与隐性因果性相比，ChatGPT在捕捉显性因果性方面表现更好。这主要是因为识别显性因果性只需识别因果提示词，而识别隐性因果性则需要利用上下文信息和常识知识进行推理。

### 4.7 开放式提示

最近，阿罗拉等人 [31]发现，开放式提示（例如“谁去了公园？”）对于ChatGPT来说，往往比限制性的提示（例如“约翰去了公园。对还是错？”）产生更好的结果。如表8所示，我们分析了ChatGPT使用开放式提示的因果推理性能：

1）“open-ended A.1/2/3”，要求ChatGPT生成输入句子中的所有因果事件对。我们设计了三种不同的提示，以全面评估ChatGPT的表现。

2）“open-ended B”，给出输入句子中的目标事件，并要求ChatGPT生成输入句子中与目标事件具有因果关系的事件。

这些提示的格式在图6中展示。

![](img/31c7a8c6a562ef6675d8568c255289a1.png)

图6: 开放式提示。标记为红色的内容需要ChatGPT回复。

我们对开放式提示采用了边界宽松的P、R和F1计算方法。具体来说，当预测的结果事件与标注的结果事件共享至少一个单词，同时预测的原因事件与标注的原因事件也共享至少一个单词，则认为预测的因果事件对是正确的。

![](img/d928f287c4622021df897b62782fbf6f.png)

表8: ChatGPT 使用开放式提示在 ECI 任务上的表现。“close-ended”表示图 1 中显示的原始 ECI 提示。值得注意的是，“close-ended”提示并不明确要求 ChatGPT 输出“yes”或“no”，但其句法形式引导 ChatGPT 几乎总是输出“yes”或“no”。

可以观察到：

开放式提示降低了ChatGPT的性能。这是因为开放式提示要求ChatGPT同时执行事件提取和ECI任务。然而，之前的研究 [32, 33]表明，ChatGPT不擅长提取事件。

## 5 结论

在本文中，我们对ChatGPT的因果推理能力进行了全面评估。实验表明：

+   ChatGPT不是一个好的因果推理器，但擅长因果解释生成；

+   ChatGPT存在严重的因果幻觉，这可能是由于因果的报告偏见；

+   随着ChatGPT版本的提升，以及ICL和CoT技术的应用，这种因果幻觉进一步加剧；

+   ChatGPT对于提示中表达因果概念的方式敏感，且开放式提示不适合ChatGPT；

+   对于句子中的事件，ChatGPT擅长捕捉明确的因果关系，在事件密度较低和事件距离较小的句子中表现更好。

+   开放式生成提示无法提高ChatGPT的因果推理能力。

尽管可能存在更细致的提示，可以进一步超越我们报告的结果，但我们认为，仅依靠提示无法从根本上解决 ChatGPT 在因果推理中面临的问题。我们希望这项研究能激发未来的工作，例如解决ChatGPT的因果幻觉问题或在多因素和多模态因果推理的场景中进一步评估ChatGPT。

## 6 参考文献

[1] Jacob Devlin, Ming-Wei Chang, Kenton Lee, and Kristina Toutanova. 2019. BERT: Pre-training of deep bidirectional transformers for language understanding. In Proc. of NAACL.
[2] Yinhan Liu, Myle Ott, Naman Goyal, Jingfei Du, Mandar Joshi, Danqi Chen, Omer Levy, Mike Lewis, Luke Zettlemoyer, and Veselin Stoyanov. 2019. Roberta: A robustly optimized bert pretraining approach. arXiv preprint arXiv:1907.11692.
[3] Long Ouyang, Jeffrey Wu, Xu Jiang, Diogo Almeida, Carroll Wainwright, Pamela Mishkin, Chong Zhang, Sandhini Agarwal, Katarina Slama, Alex Ray, et al. 2022b. Training language models to follow instructions with human feedback. Advances in Neural Information Processing Systems, 35:27730–27744.
[4] Jason Wei, Xuezhi Wang, Dale Schuurmans, Maarten Bosma, brian ichter, Fei Xia, Ed H. Chi, Quoc V Le, and Denny Zhou. 2022. Chain of thought prompting elicits reasoning in large language models. In Advances in Neural Information Processing Systems.
[5] Tommaso Caselli and Piek Vossen. 2017. The event StoryLine corpus: A new benchmark for causal and temporal relation extraction. In Proceedings of the Events and Stories in the News Workshop.
[6] Paramita Mirza, Rachele Sprugnoli, Sara Tonelli, and Manuela Speranza. 2014. Annotating causality in the TempEval-3 corpus. In Proc. of EACL.
[7] Xiaozhi Wang, Yulin Chen, Ning Ding, Hao Peng, Zimu Wang, Yankai Lin, Xu Han, Lei Hou, Juanzi Li, Zhiyuan Liu, Peng Li, and Jie Zhou. 2022. Mavenere: A unified large-scale dataset for event coreference, temporal, causal, and subevent relation extraction. In Proceedings of EMNLP.
[8] Lei Gao, Prafulla Kumar Choubey, and Ruihong Huang. 2019. Modeling document-level causal structures for event causal relation identification. In Proc. of NAACL.
[9] Prafulla Kumar Choubey and Ruihong Huang. 2017. A sequential model for classifying temporal relations between intra-sentence events. In Proc. of EMNLP.
[10] Melissa Roemmele, Cosmin Adrian Bejan, and Andrew S Gordon. 2011. Choice of plausible alternatives: An evaluation of commonsense causal reasoning. In AAAI spring symposium: logical formalizations of commonsense reasoning, pages 90–95.
[11] Li Du, Xiao Ding, Kai Xiong, Ting Liu, and Bing Qin. 2022. e-care: a new dataset for exploring explainable causal reasoning. In Proceedings of the 60th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), pages 432–446.
[12] Kishore Papineni, Salim Roukos, Todd Ward, and WeiJing Zhu. 2002. Bleu: a method for automatic evaluation of machine translation. In Proceedings of the 40th annual meeting of the Association for Computational Linguistics, pages 311–318.
[13] Chin-Yew Lin. 2004. Rouge: A package for automatic evaluation of summaries. In Text summarization branches out, pages 74–81.
[14] Jacob Devlin, Ming-Wei Chang, Kenton Lee, and Kristina Toutanova. 2019. BERT: Pre-training of deep bidirectional transformers for language understanding. In Proc. of NAACL.
[15] Yinhan Liu, Myle Ott, Naman Goyal, Jingfei Du, Mandar Joshi, Danqi Chen, Omer Levy, Mike Lewis, Luke Zettlemoyer, and Veselin Stoyanov. 2019. Roberta: A robustly optimized bert pretraining approach. arXiv preprint arXiv:1907.11692.
[16] Jian Liu, Yubo Chen, and Jun Zhao. 2020. Knowledge enhanced event causality identification with mention masking generalizations. In Proc. of IJCAI.
[17] Zhongyang Li, Xiao Ding, Kuo Liao, Bing Qin, and Ting Liu. 2021. Causalbert: Injecting causal knowledge into pre-trained models with minimal supervision. arXiv preprint arXiv:2107.09852.
[18] Jintao Liu, Zequn Zhang, Zhi Guo, Li Jin, Xiaoyu Li, Kaiwen Wei, and Xian Sun. 2023. Kept: Knowledge enhanced prompt tuning for event causality identification. Knowledge-Based Systems, 259:110064.
[19] Shirong Shen, Heng Zhou, Tongtong Wu, and Guilin Qi. 2022. Event causality identification via derivative prompt joint learning. In Proceedings of the 29th International Conference on Computational Linguistics, pages 2288–2299, Gyeongju, Republic of Korea. International Committee on Computational Linguistics.
[20] Junyoung Chung, Caglar Gulcehre, KyungHyun Cho, and Yoshua Bengio. 2014. Empirical evaluation of gated recurrent neural networks on sequence modeling. arXiv preprint arXiv:1412.3555.
[21] Alec Radford, Jeff Wu, Rewon Child, David Luan, Dario Amodei, and Ilya Sutskever. 2019. Language models are unsupervised multitask learners. arXiv preprint arXiv:1412.3555.
[22] Li Du, Xiao Ding, Kai Xiong, Ting Liu, and Bing Qin. 2022. e-care: a new dataset for exploring explainable causal reasoning. In Proceedings of the 60th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), pages 432–446.
[23] Hugo Touvron, Thibaut Lavril, Gautier Izacard, Xavier Martinet, Marie-Anne Lachaux, Timothée Lacroix, Baptiste Rozière, Naman Goyal, Eric Hambro, Faisal Azhar, Aurelien Rodriguez, Armand Joulin, Edouard Grave, and Guillaume Lample. 2023. Llama: Open and efficient foundation language models.
[24] Hyung Won Chung, Le Hou, Shayne Longpre, Barret Zoph, Yi Tay, William Fedus, Yunxuan Li, Xuezhi Wang, Mostafa Dehghani, Siddhartha Brahma, Albert Webson, Shixiang Shane Gu, Zhuyun Dai, Mirac Suzgun, Xinyun Chen, Aakanksha Chowdhery, Alex Castro-Ros, Marie Pellat, Kevin Robinson, Dasha Valter, Sharan Narang, Gaurav Mishra, Adams Yu, Vincent Zhao, Yanping Huang, Andrew Dai, Hongkun Yu, Slav Petrov, Ed H. Chi, Jeff Dean, Jacob Devlin, Adam Roberts, Denny Zhou, Quoc V. Le, and Jason Wei. 2022. Scaling instruction-finetuned language models.
[25] Chengwei Qin, Aston Zhang, Zhuosheng Zhang, Jiaao Chen, Michihiro Yasunaga, and Diyi Yang. 2023. Is chatgpt a general-purpose natural language processing task solver?
[26] Chunkit Chan, Jiayang Cheng, Weiqi Wang, Yuxin Jiang, Tianqing Fang, Xin Liu, and Yangqiu Song. 2023. Chatgpt evaluation on sentence level relations: A focus on temporal, causal, and discourse relations.
