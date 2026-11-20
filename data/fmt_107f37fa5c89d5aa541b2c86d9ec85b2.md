
# The LLM architecture and Visual

> 来源：[https://gotoaiworld.feishu.cn/docx/CrkwdFXiNoZRFxxLnhrcLLWFnSg](https://gotoaiworld.feishu.cn/docx/CrkwdFXiNoZRFxxLnhrcLLWFnSg)

## The LLM architecture

An in-depth knowledge of the Transformer architecture is not required, but it's important to understand the main steps of modern LLMs: converting text into numbers through tokenization, processing these tokens through layers including attention mechanisms, and finally generating new text through various sampling strategies.

+ Architectural Overview: Understand the evolution from encoder-decoder Transformers to decoder-only architectures like GPT, which form the basis of modern LLMs. Focus on how these models process and generate text at a high level.
+ Tokenization: Learn the principles of tokenization - how text is converted into numerical representations that LLMs can process. Explore different tokenization strategies and their impact on model performance and output quality.
+ Attention mechanisms: Master the core concepts of attention mechanisms, particularly self-attention and its variants. Understand how these mechanisms enable LLMs to process long-range dependencies and maintain context throughout sequences.
+ Sampling techniques: Explore various text generation approaches and their tradeoffs. Compare deterministic methods like greedy search and beam search with probabilistic approaches like temperature sampling and nucleus sampling.

📚 References:

+ Visual intro to Transformers by 3Blue1Brown: Visual introduction to Transformers for complete beginners.
+ LLM Visualization by Brendan Bycroft: Interactive 3D visualization of LLM internals.
+ [nanoGPT](https://www.youtube.com/watch?v=kCc8FmEb1nY) by Andrej Karpathy: A 2h-long YouTube video to reimplement [GPT from scratch](https://www.youtube.com/watch?v=zduSFxRajkE) (for programmers). He also made a video about tokenization.
+ Attention? Attention! by Lilian Weng: Historical overview to introduce the need for attention mechanisms.
+ Decoding Strategies in LLMs by Maxime Labonne: Provide code and a visual introduction to the different decoding strategies to generate text.
