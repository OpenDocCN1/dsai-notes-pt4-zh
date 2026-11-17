
# LLM Post-Training Pipeline

## Supervised Fine-Tuning

Training [techniques](https://huggingface.co/docs/trl/en/index): Full [fine-tuning](https://axolotl.ai/) updates all model parameters but requires significant compute. Parameter-efficient fine-tuning techniques like `LoRA` and `QLoRA` reduce memory requirements by training a small number of adapter parameters while keeping base weights frozen. `QLoRA` combines `4-bit` quantization with `LoRA` to reduce VRAM usage. These techniques are all implemented in the most popular fine-tuning frameworks: `TRL`, `Unsloth`, and `Axolotl`.

Training parameters: Key parameters include learning rate with schedulers, batch size, gradient accumulation, number of epochs, optimizer (like `8-bit AdamW`), weight decay for regularization, and warmup steps for training stability. `LoRA` also adds three parameters: `rank` (typically 16-128), `alpha` (1-2x `rank`), and `target modules`.

Distributed training: Scale training across multiple GPUs using `DeepSpeed` or `FSDP`. `DeepSpeed` provides three `ZeRO` optimization stages with increasing levels of memory efficiency through state partitioning. Both methods support gradient checkpointing for memory efficiency.

Monitoring: Track training metrics including loss curves, learning rate schedules, and gradient norms. Monitor for common issues like loss spikes, gradient explosions, or performance degradation.

📚 References:

+ [Fine-tune Llama 3.1 Ultra-Efficiently with Unsloth](https://huggingface.co/blog/mlabonne/sft-llama3) by Maxime Labonne: Hands-on tutorial on how to fine-tune a `Llama 3.1` model using `Unsloth`.

+ `Axolotl` - Documentation by Wing Lian: Lots of interesting information related to distributed training and dataset formats.

+ [Mastering LLMs](https://parlance-labs.com/education/) by Hamel Husain: Collection of educational resources about fine-tuning (but also RAG, evaluation, applications, and prompt engineering).

+ [LoRA insights](https://lightning.ai/pages/community/lora-insights/) by Sebastian Raschka: Practical insights about `LoRA` and how to select the best parameters.

## Preference Alignment

Preference alignment is a second stage in the post-training pipeline, focused on aligning generated answers with human preferences. This stage was designed to tune the tone of LLMs and reduce toxicity and hallucinations. However, it has become increasingly important to also boost their performance and improve usefulness. Unlike `SFT`, there are many preference alignment algorithms. Here, we'll focus on the three most important ones: `DPO`, `GRPO`, and `PPO`.

+ Rejection sampling: For each prompt, use the trained model to generate multiple responses, and score them to infer chosen/rejected answers. This creates on-policy data, where both responses come from the model being trained, improving alignment stability.

+ Direct Preference Optimization. Directly optimizes the policy to maximize the likelihood of chosen responses over rejected ones. It doesn't require reward modeling, which makes it more computationally efficient than RL techniques but slightly worse in terms of quality. Great for creating chat models.

+ [Reward model](https://huggingface.co/docs/trl/en/index): Train a [reward model](https://github.com/OpenRLHF/OpenRLHF) with human feedback to predict metrics like human preferences. It can leverage frameworks like `TRL`, [verl](https://github.com/volcengine/verl), and `OpenRLHF` for scalable training.

+ Reinforcement Learning: [RL](https://arxiv.org/abs/2402.03300) techniques like GRPO and [PPO](https://arxiv.org/abs/1707.06347) iteratively update a policy to maximize rewards while staying close to the initial behavior. They can use a reward model or reward functions to score responses. They tend to be computationally expensive and require careful tuning of hyperparameters, including learning rate, batch size, and clip range. Ideal for creating reasoning models.

📚 References:

+ Illustrating RLHF by Hugging Face: Introduction to `RLHF` with reward model training and fine-tuning with reinforcement learning.

+ [LLM Training: RLHF and Its Alternatives](https://magazine.sebastianraschka.com/p/llm-training-rlhf-and-its-alternatives) by Sebastian Raschka: Overview of the `RLHF` process and alternatives like `RLAIF`.

+ Preference Tuning LLMs by Hugging Face: Comparison of the `DPO`, `IPO`, and `KTO` algorithms to perform preference alignment.

+ Fine-tune with DPO by Maxime Labonne: Tutorial to fine-tune a `Mistral-7b` model with `DPO` and reproduce `NeuralHermes-2.5`.

+ Fine-tune with GRPO by Maxime Labonne: Practical exercise to fine-tune a small model with `GRPO`.

+ [DPO Wandb logs](https://wandb.ai/alexander-vishnevskiy/dpo/reports/TRL-Original-DPO--Vmlldzo1NjI4MTc4) by Alexander Vishnevskiy: It shows you the main `DPO` metrics to track and the trends you should expect.

## Evaluation

Reliably evaluating LLMs is a complex but essential task guiding data generation and training. It provides invaluable feedback about areas of improvement, which can be leveraged to modify the data mixture, quality, and training parameters. However, it's always good to remember Goodhart's law: "When a measure becomes a target, it ceases to be a good measure."

+ Automated benchmarks: Evaluate models on specific tasks using curated datasets and metrics, like `MMLU`. It works well for concrete tasks but struggles with abstract and creative capabilities. It is also prone to data contamination.

+ Human evaluation: It involves humans prompting models and grading responses. Methods range from vibe checks to systematic annotations with specific guidelines and large-scale community voting (arena). It is more suited for subjective tasks and less reliable for factual accuracy.

+ Model-based evaluation: Use judge and reward models to evaluate model outputs. It highly correlates with human preferences but suffers from bias toward their own outputs and inconsistent scoring.

+ Feedback signal: Analyze error patterns to identify specific weaknesses, such as limitations in following complex instructions, lack of specific knowledge, or susceptibility to adversarial prompts. This can be improved with better data generation and training parameters.

📚 References:

+ Evaluation guidebook by Clémentine Fourrier: Practical insights and theoretical knowledge about LLM evaluation.

+ Open LLM Leaderboard by Hugging Face: Main leaderboard to compare LLMs in an open and reproducible way (automated benchmarks).

+ Language Model Evaluation Harness by EleutherAI: A popular framework for evaluating LLMs using automated benchmarks.

+ [Lighteval](https://github.com/huggingface/lighteval) by Hugging Face: Alternative evaluation framework that also includes model-based evaluations.

+ [Chatbot Arena](https://lmarena.ai/) by LMSYS: Elo rating of general-purpose LLMs, based on comparisons made by humans (human evaluation).

## Quantization

Quantization is the process of converting the parameters and activations of a model using a lower precision. For example, weights stored using 16 bits can be converted into a `4-bit` representation. This technique has become increasingly important to reduce the computational and memory costs associated with LLMs.

+ Base techniques: Learn the different levels of precision (`FP32`, `FP16`, `INT8`, etc.) and how to perform naïve quantization with `absmax` and `zero-point` techniques.

+ [`GGUF` & `llama.cpp`](https://github.com/ggerganov/llama.cpp): Originally designed to run on CPUs, `llama.cpp` and the `GGUF` format have become the most popular tools to run LLMs on consumer-grade hardware. It supports storing special tokens, vocabulary, and metadata in a single file.
