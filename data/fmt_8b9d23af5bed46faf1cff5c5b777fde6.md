
# Patterns for Building LLM-based Systems & Products

> 来源：[https://leonzhang.feishu.cn/docx/Hh1tdSFe3oYz9RxSlDTcVbMQnAK](https://leonzhang.feishu.cn/docx/Hh1tdSFe3oYz9RxSlDTcVbMQnAK)

> 🔗 原文链接：https://eugeneyan.com/writing/llm-patterns/

> Discussions on [HackerNews](https://news.ycombinator.com/item?id=36965993), [Twitter](https://twitter.com/eugeneyan/status/1686531758701899776), and [LinkedIn](https://www.linkedin.com/posts/eugeneyan_patterns-for-building-llm-based-systems-activity-7092300473981927424-_wVo)

“There is a large class of problems that are easy to imagine and build demos for, but extremely hard to make products out of. For example, self-driving: It’s easy to demo a car self-driving around a block, but making it into a product takes a decade.” - Karpathy

This write-up is about practical patterns for integrating large language models (LLMs) into systems & products. We’ll build on academic research, industry resources, and practitioner know-how, and distill them into key ideas and practices.

There are seven key patterns. They’re also organized along the spectrum of improving performance vs. reducing cost/risk, and closer to the data vs. closer to the user.

+ Evals : To measure performance
+ RAG : To add recent, external knowledge
+ Fine-tuning : To get better at specific tasks
+ Caching : To reduce latency & cost
+ Guardrails : To ensure output quality
+ Defensive UX : To anticipate & manage errors gracefully
+ Collect user feedback : To build our data flywheel

Also see this addendum on how to match these LLM patterns to potential problems.

![](img/a520b74c11d4d97c94b099d173e319e6.png)

LLM patterns: From data to user, from defensive to offensive ( see connections between patterns )

## Evals: To measure performance

Evaluations are a set of measurements used to assess a model’s performance on a task. They include benchmark data and metrics. From a HackerNews comment:

> How important evals are to the team is a major differentiator between folks rushing out hot garbage and those seriously building products in the space.

### Why evals?

Evals enable us to measure how well our system or product is doing and detect any regressions. (A system or product can be made up of multiple components such as LLMs, prompt templates, retrieved context, and parameters like temperature.) A representative set of evals takes us a step towards measuring system changes at scale. Without evals, we would be flying blind, or would have to visually inspect LLM outputs with each change.

### More about evals

There are many benchmarks in the field of language modeling . Some notable ones are:

+ [MMLU](https://arxiv.org/abs/2009.03300): A set of 57 tasks that span elementary math, US history, computer science, law, and more. To perform well, models must possess extensive world knowledge and problem-solving ability.

+ EleutherAI Eval : Unified framework to test models via zero/few-shot settings on 200 tasks. Incorporates a large number of evals including BigBench, MMLU, etc.

+ [HELM](https://arxiv.org/abs/2211.09110): Instead of specific tasks and metrics, HELM offers a comprehensive assessment of LLMs by evaluating them across domains. Metrics include accuracy, calibration, robustness, fairness, bias, toxicity, etc. Tasks include Q&A, information retrieval, summarization, text classification, etc.

+ [AlpacaEval](https://github.com/tatsu-lab/alpaca_eval): Automated evaluation framework which measures how often a strong LLM (e.g., GPT-4) prefers the output of one model over a reference model. Metrics include win rate, bias, latency, price, variance, etc. Validated to have high agreement with 20k human annotations.

We can group metrics into two categories: context-dependent or context-free.

+ Context-dependent : These take context into account. They’re often proposed for a specific task; repurposing them for other tasks will require some adjustment.

+ Context-free : These aren’t tied to the context when evaluating generated output; they only compare the output with the provided gold references. As they’re task agnostic, they’re easier to apply to a wide variety of tasks.

To get a better sense of these metrics (and their potential shortfalls), we’ll explore a few of the commonly used metrics such as BLEU, ROUGE, BERTScore, and MoverScore.

[BLEU](https://dl.acm.org/doi/10.3115/1073083.1073135)(Bilingual Evaluation Understudy) is a precision-based metric: It counts the number of n-grams in the generated output that also show up in the reference, and then divides it by the total number of words in the output. It’s predominantly used in machine translation and remains a popular metric due to its cost-effectiveness.

First, precision for various values of is computed:

is clipped by the maximum number of times an n-gram appears in any corresponding reference sentence.

Once we’ve computed precision at various , a final BLEU-N score is computed as the geometric mean of all the scores.

However, since precision relies solely on n-grams and doesn’t consider the length of the generated output, an output containing just one unigram of a common word (like a stop word) would achieve perfect precision. This can be misleading and encourage outputs that contain fewer words to increase BLEU scores. To counter this, a brevity penalty is added to penalize excessively short sentences.

Thus, the final formula is:

[ROUGE](https://aclanthology.org/W04-1013/)(Recall-Oriented Understudy for Gisting Evaluation) : In contrast to BLEU, ROUGE is recall-oriented. It counts the number of words in the reference that also occur in the output. It’s typically used to assess automatic summarization tasks.

There are several ROUGE variants. ROUGE-N is most similar to BLEU in that it also counts the number of matching n-grams between the output and the reference.

Other variants include:

+ ROUGE-L: This measures the longest common subsequence (LCS) between the output and the reference. It considers sentence-level structure similarity and zeros in on the longest series of co-occurring in-sequence n-grams.

+ ROUGE-S: This measures the skip-bigram between the output and reference. Skip-bigrams are pairs of words that maintain their sentence order regardless of the words that might be sandwiched between them.

[BERTScore](https://arxiv.org/abs/1904.09675)is an embedding-based metric that uses cosine similarity to compare each token or n-gram in the generated output with the reference sentence. There are three components to BERTScore:

+ Recall: Average cosine similarity between each token in the reference and its closest match in the generated output.

+ Precision: Average cosine similarity between each token in the generated output and its nearest match in the reference.

+ F1: Harmonic mean of recall and precision

BERTScore is useful because it can account for synonyms and paraphrasing. Simpler metrics like BLEU and ROUGE can’t do this due to their reliance on exact matches. BERTScore has been shown to have better correlation for tasks such as image captioning and machine translation.

[MoverScore](https://arxiv.org/abs/1909.02622)also uses contextualized embeddings to compute the distance between tokens in the generated output and reference. But unlike BERTScore, which is based on one-to-one matching (or “hard alignment”) of tokens, MoverScore allows for many-to-one matching (or “soft alignment”).

![](img/203e11a77b5a5265c1ceba9c3037a8bf.png)

BERTScore (left) vs. MoverScore (right; source )
