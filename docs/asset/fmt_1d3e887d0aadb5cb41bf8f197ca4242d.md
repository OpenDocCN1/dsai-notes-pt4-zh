
Their evaluation involved the believability of the Agent’s behaviors as the main metric, with human evaluators scoring them.

![](img/7d9d8db6f16577ab812e4ee358ca3eb2.png)

[A](https://arxiv.org/pdf/2304.03442)nnotated figure of the Generative Agents: Interactive Simulacra of Human Behavior paper.

It showcases how important observation, planning, and reflection are together in the performance of these Generative Agents. As explored before, planning is not complete without reflective behavior.

## Modular Frameworks

Whate[ver ](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-15-156659273)[fram](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-16-156659273)ework you choose for creating Multi-Agent systems, they are generally composed of several ingredients, including its profile, perception of the environment, memory, planning, and available actions.(15)(16)

![](img/8040a73d880e1db0663011bca37f4c67.png)

P[opul](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-17-156659273)ar framew[orks](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-18-156659273) for implem[enti](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-19-156659273)ng these components are AutoGen(17), MetaGPT(18), and CAMEL(19). However, each framework approaches communication between each Agent a bit differently.

With CAMEL, for instance, the user first creates its question and defines AI User and AI Assistant roles. The AI user role represents the human user and will guide the process.

![](img/a8a138522e59221ad4bf72d5e5ea5470.png)

After that, the AI User and AI Assistant will collaborate on resolving the query by interacting with each other.

![](img/2c73c3377b7da98190cbf9b44a9ee526.png)

This role-playing methodology enables collaborative communication between agents.

AutoGen and MetaGPT have different methods of communication, but it all boils down to this collaborative nature of communication. Agents have opportunities to engage and talk with one another to update their current status, goals, and next steps.

In the last year, and especially the last few weeks, the growth of these frameworks has been explosive.

![](img/c91b6b5b275e3831e6218553e2f5ad06.png)

2025 is going to be a truly exciting year as these frameworks keep maturing and developing!

# Conclusion

This concludes our journey of LLM Agents! Hopefully, this post gives a better understanding of how LLM Agents are built.

To see more visualizations related to LLMs and to support this newsletter, check out the book I wrote on Large Language Models!

![](img/162f7c873f01d10b342ba4a521e60269.png)

Official website of [the bo](https://www.amazon.com/Hands-Large-Language-Models-Understanding/dp/1098150961)[ok. Yo](https://github.com/handsOnLLM/Hands-On-Large-Language-Models)u can order the book on Amazon. All code is uploaded to GitHub.

[1](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-1-156659273)

Russell, S. J., & Norvig, P. (2016). Artificial intelligence: a modern approach. pearson.

[2](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-2-156659273)

Sumers, Theodore, et al. "Cognitive architectures for language agents." Transactions on Machine Learning Research (2023).

[3](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-3-156659273)

Schick, Timo, et al. "Toolformer: Language models can teach themselves to use tools." Advances in Neural Information Processing Systems 36 (2023): 68539-68551.

[4](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-4-156659273)

Qin, Yujia, et al. "Toolllm: Facilitating large language models to master 16000+ real-world apis." arXiv preprint arXiv:2307.16789 (2023).

[5](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-5-156659273)

Patil, Shishir G., et al. "Gorilla: Large language model connected with massive apis." Advances in Neural Information Processing Systems 37 (2024): 126544-126565.

[6](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-6-156659273)

"Introducing the Model Context Protocol." Anthropic, www.anthropic.com/news/model-context-protocol. Accessed 13 Mar. 2025.

[7](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-7-156659273)

Mann, Ben, et al. "Language models are few-shot learners." arXiv preprint arXiv:2005.14165 1 (2020): 3.

[8](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-8-156659273)

Wei, Jason, et al. "Chain-of-thought prompting elicits reasoning in large language models." Advances in neural information processing systems 35 (2022): 24824-24837.

[9](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-9-156659273)

Kojima, Takeshi, et al. "Large language models are zero-shot reasoners." Advances in neural information processing systems 35 (2022): 22199-22213.

[10](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-10-156659273)

Yao, Shunyu, Zhao, Jeffrey, Yu, Dian, Du, Nan, Shafran, Izhak, Narasimhan, Karthik, and Cao, Yuan. ReAct: Synergizing Reasoning and Acting in Language Models. Retrieved from https://par.nsf.gov/biblio/10451467. International Conference on Learning Representations (ICLR).

[11](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-11-156659273)

Shinn, Noah, et al. "Reflexion: Language agents with verbal reinforcement learning." Advances in Neural Information Processing Systems 36 (2023): 8634-8652.

[12](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-12-156659273)

Madaan, Aman, et al. "Self-refine: Iterative refinement with self-feedback." Advances in Neural Information Processing Systems 36 (2023): 46534-46594.

[13](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-13-156659273)

Park, Joon Sung, et al. "Generative agents: Interactive simulacra of human behavior." Proceedings of the 36th annual acm symposium on user interface software and technology. 2023.

[14](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-14-156659273)

To see a cool interactive playground of t[he Generative Agents,](https://reverie.herokuapp.com/arXiv_Demo/) follow this link: https://reverie.herokuapp.com/arXiv_Demo/

[15](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-15-156659273)

Wang, Lei, et al. "A survey on large language model based autonomous agents." Frontiers of Computer Science 18.6 (2024): 186345.

[16](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-16-156659273)

Xi, Zhiheng, et al. "The rise and potential of large language model based agents: A survey." Science China Information Sciences 68.2 (2025): 121101.

[17](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-17-156659273)

Wu, Qingyun, et al. "Autogen: Enabling next-gen llm applications via multi-agent conversation." arXiv preprint arXiv:2308.08155 (2023).

[18](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-18-156659273)

Hong, Sirui, et al. "Metagpt: Meta programming for multi-agent collaborative framework." arXiv preprint arXiv:2308.00352 3.4 (2023): 6.

[19](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-anchor-19-156659273)

Li, Guohao, et al. "Camel: Communicative agents for" mind" exploration of large language model society." Advances in Neural Information Processing Systems 36 (2023): 51991-52008.
