
如果在训练过程中将`dropout`率提高，两个任务中神经网络神经元之间的冗余都得到了显著提高，因为协同和特有信息都会被`dropout`这个操作给打乱，所以需要将重要的信息存储到更多的地方。如果在训练过程完成后随机破坏（`lesion`）神经元，会发现如果我们破坏的是那些和其他神经元有更高协同的神经元，其高协同任务的表现相较于破坏低协同的神经元会出现显著下降。

![](img/55f645f86961fc04f0135dd395c4f617.png)

图10. 信息分解在人工神经网络的实验

信息动力学及信息分解虽然能作为很好的工具，让我们对大脑作为信息处理系统有更多的理解，但有一个关键的问题目前被回避掉了：我们知道信息传播的结果，但是不知道信息是通过哪种物理途径进行传播的。这就是网络沟通模型（`network communication models`）可能会启发下一步有关信息分解在脑领域应用的原因。比如，是否因为不同信号在对撞节点的相遇导致了协同效应的发生，对于重要信息需要通过多途径广播（`broadcast`），所以会存在高冗余。

## Andrea I. Luppi及其同事对意识问题的综述[6]

围绕意识这个问题，在Luppi等人的综述里提出了我们需要针对时间尺度、空间尺度以及信息尺度进行分解（`decompose`）来细致拆分。这里分解的概念涵盖可能有两类：一是对数据的降维，直接通过降维手段例如`PCA`保留关键维度，二是对数据的再表征，拆分成更细致的组成结构进行分析。

![](img/9192fc748b83a535c7d39b59910a5455.png)

图11. 三种层面上的分解

### 时间尺度分解

提供一个时间尺度分解的简单案例，将一个长时间轴的脑数据分成很多个短的脑数据，然后根据每个短的时间轴计算功能链接（见图12）。我们会发现，在无意识活动时，那些与结构链接更类似的功能链接会更频繁的出现，而与结构链接不相像的功能链接会变得更低频。

![](img/6333f57807708651068c9f74c83bc5e2.png)

图12. 案例：时间上的分解

### 空间尺度分解

空间尺度的分解则更为复杂，可以在功能上进行分解，也可以在空间（结构）上进行分解。可以发现当意识状态被不同的方法改变时（比如意识紊乱（`Disorder of Consciousness`）和服用致幻剂（`Psilocybine`）），从功能分解去理解，会发现两者都是`awareness`受到影响[25]，但是从结构功能耦合（`structure-function coupling`）的角度来看，前者可以被大脑本身结构更好的预测，而后者依赖一些与大脑结构不类似的部分[26]。

![](img/39677f421bf2304d3f8b5afe2109c682.png)

图13. 案例：空间上的分解

### 信息尺度分解

信息尺度分解则和信息分解这一框架紧密相关。信息处理中，冗余信息提供系统的鲁棒性，而协同信息可以增进系统的效率。具体在意识这领域的应用，在计算整合信息中，会发现之前的Φ WMS多减去了一份冗余，而`causal density`多增加了一份协同（见图14，具体定义可见[27]）。所以在[15]中，新的指标Φ R便被设计出来去发现过去指标所不能区分的有无意识状态。目前还存在的一个公开话题是，对信息在一个特定的时间或空间尺度进行分解会不会有意义。

![](img/485a5f4b6abdc70064fab6b4e46159d8.png)

图14. 案例：信息上的分解

## Thomas F. Varley及其同事对信息论指标的讨论

因为信息分解的计算复杂度，所以在实际应用中研究者可能会采取一些更简要的指标，比如Varley及其同事常会讨论的`total correlation`去衡量信息多大程度被整合，`O-information`去衡量系统是冗余主导（大于0）还是协同主导（小于0），`TSE Complexity`去衡量系统的整合与隔离平衡。在[8]中，主要是对`O-information`的使用。在[9]中，对于`O-Information`有类似于将Φ WMS转化成Φ R的操作，通过信息分解解读公式是否合理地衡量了我们关心的系统性质，然后进行操作改变。

信息分解的框架也会有所不同，比如`PID`是需要我们去定义什么是目标才能做信息分解，但`PED`框架是不需要的[28]，[`GI[D`框架是Varley近期对信息分解框](http://mp.weixin.qq.com/s?__biz=MzIzMjQyNzQ5MA==&mid=2247685665&idx=1&sn=bbc5099136ec68c57a9d30bdb1edc66d&chksm=e8994cacdfeec5bab112b4254372a06634e732edf8384c43e97c125d716498876ed1fe86cffd&scene=21#wechat_redirect)架的拓展，针对`KL`散度进行信息分解[7]。关于具体这些指标的使用结果，可以回顾《复杂系统研究者 Thomas Varley 做客集智读书会：信息、脑与网络》。另外，在「因果涌现读书会」第四季吕奥博等人在信息分解的计算框架下也有进一步地拓展。

![](img/a185b5cba391da59bc7224c12e4fc821.png)

图15. Varley常讨论的指标

## 展望

脑作为多尺度的复杂系统[29]，且因为意识问题的存在，是因果涌现理论的天然适用场景（见图16）。但不同的因果涌现框架在应用到脑领域都存在问题。整合信息论、因果结构或者因果涌现（`Hoel`）是否真的能解释意识问题也面临许多理论或实验上的挑战，详见：

> 《吵翻了：意识是如何产生的？科学家能否达成一致？》

> 《万字长文：意识的大一统理论要来了吗？》

虽然信息分解乃至整合信息分解（`Rosas`）在脑领域已经有很丰富的产出，但不可回避的是，目前应用忽略了底层的数学性质，并通过技巧回避了计算复杂度的问题（多次取样少量元素而不是去计算多数元素的信息原子）。引入机器学习的操作让人兴奋，但神经动力学本身的拟合是个大问题，并且也对可解释性带来了更大的挑战。但依旧不可否认的是，理论可能是错的，其想探索的问题却是真的，而这已经足够让人兴奋。

![](img/81eb0ae3a25b0e39e0eca9d44a756ec5.png)

图16. 总结

## 参考文献

1. Hoel EP, Albantakis L, Tononi G. Quantifying causal emergence shows that macro can beat micro. Proc Natl Acad Sci U S A. 2013 Dec 3;110(49):19790-5. doi: 10.1073/pnas.1314922110. Epub 2013 Nov 18. PMID: 24248356; PMCID: PMC3856819.

2. Tononi, G., Sporns, O. Measuring information integration. BMC Neurosci 4, 31 (2003). https://doi.org/10.1186/1471-2202-4-31

3. Rosas, Fernando E., et al. "Reconciling emergence: An information-theoretic approach to identify causal emergence in multivariate data." PLoS computational biology 16.12 (2020): e1008289.

4. Mediano PAM, Rosas FE, Luppi AI, Jensen HJ, Seth AK, Barrett AB, Carhart-Harris RL, Bor D. 2022 Greater than the parts: a review of the information decomposition approach to causal emergence. Phil. Trans. R. Soc. A 380: 20210246. https://doi.org/10.1098/rsta.2021.024

5. Luppi, A. I., Rosas, F. E., Mediano, P. A. M., Menon, D. K., & Stamatakis, E. A. (2024). Information decomposition and the informational architecture of the brain. Trends in Cognitive Sciences, 28(4), 352–368. https://doi.org/10.1016/j.tics.2023.11.005

6. Luppi, A. I., Rosas, F. E., Mediano, P. A. M., Demertzi, A., Menon, D. K., & Stamatakis, E. A. (2024). Unravelling consciousness and brain function through the lens of time, space, and information. Trends in Neurosciences, 47(7), 551-568. https://doi.org/10.1016/j.tins.2024.05.007

7. Varley, T.F., Pope, M., Faskowitz, J. et al. Multivariate information theory uncovers synergistic subsystems of the human cerebral cortex. Commun Biol 6, 451 (2023). https://doi.org/10.1038/s42003-023-04843-w

8. Varley, T. F., Pope, M., Puxeddu, M. G., Faskowitz, J., & Sporns, O. (2023). Partial entropy decomposition reveals higher-order information structures in human brain activity. Proceedings of the National Academy of Sciences, 120(30), e2300888120. https://doi.org/10.1073/pnas.2300888120

9. Varley, T. F. (2024). Generalized decomposition of multivariate information. PLOS ONE, 19(2), e0297128. https://doi.org/10.1371/journal.pone.0297128

10. Grasso, M., Albantakis, L., Lang, J.P. et al. Causal reductionism and causal structures. Nat Neurosci 24, 1348–1355 (2021). https://doi.org/10.1038/s41593-021-00911-8

11. Bedau, M. A. (1997). Weak Emergence. Philosophical Perspectives, 11, 375–399. http://www.jstor.org/stable/2216138

12. Varley, T.F. Flickering Emergences: The Question of Locality in Information-Theoretic Approaches to Emergence. Entropy 2023, 25, 54. https://doi.org/10.3390/e25010054

13. Varley, Thomas F., and Erik Hoel. "Emergence as the conversion of information: a unifying theory." Philosophical Transactions of the Royal Society A 380.2227 (2022): 20210150.

14. P.A.M. Mediano, et al. Towards an extended taxonomy of information dynamics via integrated information decomposition. arXiv (2021). http://doi.org/10.48550/arxiv.2109.13186

15. Luppi, A. I., Mediano, P. A. M., Rosas, F. E., Allanson, J., Pickard, J. D., Carhart-Harris, R., Williams, G. B., Craig, M. M., Finoia, P., Owen, A. M., Naci, L., Menon, D. K., Bor, D., & Stamatakis, E. A. (2024). A synergistic workspace for human consciousness revealed by integrated information decomposition. *eLife, 88173. https://doi.org/10.7554/eLife.88173

16. Bassett DS, Gazzaniga MS. Understanding complexity in the human brain. Trends Cogn Sci. 2011 May;15(5):200-9. doi: 10.1016/j.tics.2011.03.006. Epub 2011 Apr 14. PMID: 21497128; PMCID: PMC3170818.

17. Murphy, K. A., & Bassett, D. S. (2024). Information decomposition in complex systems via machine learning. Proceedings of the National Academy of Sciences, 121(13).https://doi.org/10.1073/pnas.2312988121

18. Seth, A. K. (2008, August). Measuring emergence via nonlinear Granger causality. In alife (Vol. 2008, pp. 545-552).

19. Yuan, B., Zhang, J., Lyu, A., Wu, J., Wang, Z., Yang, M., Liu, K., Mou, M., & Cui, P. (2024). Emergence and Causality in Complex Systems: A Survey of Causal Emergence and Related Quantitative Studies. Entropy, 26(1), 108. https://doi.org/10.3390/e26020108

20. Zhang, Jiang & Liu, Kaiwei. (2022). Neural Information Squeezer for Causal Emergence. Entropy. 2023. 10.3390/e25010026.

21. Yang, Mingzhe, et al. "Finding emergence in data: causal emergence inspired dynamics learning." arXiv preprint arXiv:2308.09952 (2023).

22. Luppi AI, Mediano PAM, Rosas FE, Holland N, Fryer TD, O'Brien JT, Rowe JB, Menon DK, Bor D, Stamatakis EA. A synergistic core for human brain evolution and cognition. Nat Neurosci. 2022 Jun;25(6):771-782. doi: 10.1038/s41593-022-01070-0. Epub 2022 May 26. PMID: 35618951; PMCID: PMC7614771.

23. Paul L. Williams,Randall D. Beer: Nonnegative Decomposition of Multivariate Information ,arXiv:1004.2515,2010

24. Proca, A. M., Rosas, F. E., Luppi, A. I., Bor, D., Crosby, M., & Mediano, P. A. M. (2024). Synergistic information supports modality integration and flexible learning in neural networks solving multiple tasks. PLoS Computational Biology, 20(6), e1012178.https://doi.org/10.1371/journal.pcbi.1012178
