
## 模型初始化与数据准备

```python
import numpy as np
import torch
from gritlm import GritLM

# Loads the model for both capabilities; If you only need embedding pass "mode="embedding"" to save memory (no lm head)
model = GritLM("GritLM/GritLM-7B", torch_dtype="auto")
# To load the 8x7B you will likely need multiple GPUs.
# All the kwargs are passed to HF from_pretrained so you can just do the below to load on multiple GPUs:
# model = GritLM("GritLM/GritLM-8x7B", torch_dtype="auto", device_map="auto")
# You can also load other models e.g.
# model = GritLM("Muennighoff/SGPT-125M-weightedmean-nli-bitfit", pooling_method="weighted_mean", attn=None)
# model = GritLM("hkunlp/instructor-base", pooling_method="mean", attn=None)

queries = ['Please explain to me how Bitcoin works.', 'What is "Generative Representational Instruction Tuning"?']
documents = [
    "A purely peer-to-peer version of electronic cash would allow online payments to be sent directly from one party to another without going through a financial institution. Digital signatures provide part of the solution, but the main benefits are lost if a trusted third party is still required to prevent double-spending. We propose a solution to the double-spending problem using a peer-to-peer network. The network timestamps transactions by hashing them into an ongoing chain of hash-based proof-of-work, forming a record that cannot be changed without redoing the proof-of-work. The longest chain not only serves as proof of the sequence of events witnessed, but proof that it came from the largest pool of CPU power. As long as a majority of CPU power is controlled by nodes that are not cooperating to attack the network, they'll generate the longest chain and outpace attackers. The network itself requires minimal structure. Messages are broadcast on a best effort basis, and nodes can leave and rejoin the network at will, accepting the longest proof-of-work chain as proof of what happened while they were gone.",
    "All text-based language problems can be reduced to either generation or embedding. Current models only perform well at one or the other. We introduce generative representational instruction tuning (GRIT) whereby a large language model is trained to handle both generative and embedding tasks by distinguishing between them through instructions. Compared to other open models, our resulting GritLM 7B sets a new state of the art on the Massive Text Embedding Benchmark (MTEB) and outperforms all models up to its size on a range of generative tasks. By scaling up further, GritLM 8X7B outperforms all open generative language models that we tried while still being among the best embedding models. Notably, we find that GRIT matches training on only generative or embedding data, thus we can unify both at no performance loss. Among other benefits, the unification via GRIT speeds up Retrieval-Augmented Generation (RAG) by > 60% for long documents, by no longer requiring separate retrieval and generation models. Models, code, etc. are freely available at https://github.com/ContextualAI/gritlm."
]

CACHE_FORMAT_DOC = "\n<|user|>\n{query}\n\nAnswer the prior query while optionally using the context prior to it\n<|assistant|>\n"
CACHE_FORMAT_QUERY = "\n<|user|>\n{doc}\n\nOptionally using the prior context answer the query prior to it\n<|assistant|>\n"
CACHE_FORMAT_QUERY_DOC = "\n<|user|>\nOptionally using the prior context answer the query prior to it\n<|assistant|>\n"
CACHE_FORMAT_DOC_QUERY = "\n<|user|>\nAnswer the prior query while optionally using the context prior to it\n<|assistant|>\n"

def gritlm_instruction(instruction):
    return "<|user|>\n" + instruction + "\n<|embed|>\n" if instruction else "<|embed|>\n"
```

### GRIT DOC CACHING

```python
# cache: Tuple of `tuple(torch.FloatTensor)` of length `config.n_layers`, with each tuple having 2 tensors of shape `(batch_size, num_heads, sequence_length, embed_size_per_head)`
d_rep, d_cache = model.encode(documents, instruction=gritlm_instruction(""), get_cache=True)
q_rep = model.encode(queries, instruction=gritlm_instruction(""))

from scipy.spatial.distance import cosine
sims = {q: [1 - cosine(q_rep[i], d_rep[j]) for j in range(len(d_rep))] for i, q in enumerate(queries)}

for q, q_sims in sims.items():
    sim_idx = np.argmax(q_sims)
    cache = tuple([
        (d_cache[i][0][sim_idx:sim_idx+1], d_cache[i][1][sim_idx:sim_idx+1]) for i, c in enumerate(d_cache)
    ])
    # BOS is already in the cache
    inputs = model.tokenizer(CACHE_FORMAT_DOC.format(query=q), return_tensors="pt", add_special_tokens=False).to(model.device)
    inputs["use_cache"] = True
    # Attend to the cache too
    inputs["attention_mask"] = torch.cat((
        torch.ones((cache[0][0].shape[0], cache[0][0].shape[2]), dtype=torch.long, device=inputs["attention_mask"].device),
        inputs["attention_mask"],
    ), dim=1)
    generation = model.generate(**inputs, max_new_tokens=256, past_key_values=cache, do_sample=False)
    decoded = model.tokenizer.batch_decode(generation)
    print(decoded[0])

"""
<|user|>
What is "Generative Representational Instruction Tuning"?

Answer the prior query while optionally using the context prior to it
<|assistant|>
Generative Representational Instruction Tuning (GRIT) is a method for training language models that can perform both generative and embedding tasks. It involves training a large language model to handle both types of tasks by distinguishing between them through instructions. GRIT is designed to improve the performance of language models on both generative and embedding tasks, and it can be used to unify both types of tasks at no performance loss.</s>
"""
```

### GRIT QUERY CACHING

```python
# cache: Tuple of `tuple(torch.FloatTensor)` of length `config.n_layers`, with each tuple having 2 tensors of shape `(batch_size, num_heads, sequence_length, embed_size_per_head)`
d_rep = model.encode(documents, instruction=gritlm_instruction(""))
q_rep, q_cache = model.encode(queries, instruction=gritlm_instruction(""), get_cache=True)

from scipy.spatial.distance import cosine
sims = {d: [1 - cosine(q_rep[i], d_rep[j]) for j in range(len(d_rep))] for i, d in enumerate(documents)}

for d, d_sims in sims.items():
    sim_idx = np.argmax(d_sims)
    cache = tuple([
        (q_cache[i][0][sim_idx:sim_idx+1], q_cache[i][1][sim_idx:sim_idx+1]) for i, c in enumerate(q_cache)
    ])
    # BOS is already in the cache
    inputs = model.tokenizer(CACHE_FORMAT_QUERY.format(doc=d), return_tensors="pt", add_special_tokens=False).to(model.device)
    inputs["use_cache"] = True
    # Attend to the cache too
    inputs["attention_mask"] = torch.cat((
        torch.ones((cache[0][0].shape[0], cache[0][0].shape[2]), dtype=torch.long, device=inputs["attention_mask"].device),
        inputs["attention_mask"],
    ), dim=1)
    generation = model.generate(**inputs, max_new_tokens=256, past_key_values=cache, do_sample=False)
    decoded = model.tokenizer.batch_decode(generation)
    print(decoded[0])

"""
<|user|>
All text-based language problems can be reduced to either generation or embedding. Current models only perform well at one or the other. We introduce generative representational instruction tuning (GRIT) whereby a large language model is trained to handle both generative and embedding tasks by distinguishing between them through instructions. Compared to other open models, our resulting GritLM 7B sets a new state of the art on the Massive Text Embedding Benchmark (MTEB) and outperforms all models up to its size on a range of generative tasks. By scaling up further, GritLM 8X7B outperforms all open generative language models that we tried while still being among the best embedding models. Notably, we find that GRIT matches training on only generative or embedding data, thus we can unify both at no performance loss. Among other benefits, the unification via GRIT speeds up Retrieval-Augmented Generation (RAG) by > 60% for long documents, by no longer requiring separate retrieval and generation models. Models, code, etc. are freely available at <https://github.com/ContextualAI/gritlm>.</s>
"""
```

### GRIT QUERY-DOC CACHING

```python
# cache: Tuple of `tuple(torch.FloatTensor)` of length `config.n_layers`, with each tuple having 2 tensors of shape `(batch_size, num_heads, sequence_length, embed_size_per_head)`
d_rep, d_cache = model.encode(documents, instruction=gritlm_instruction(""), get_cache=True, add_special_tokens=False)
q_rep, q_cache = model.encode(queries, instruction=gritlm_instruction(""), get_cache=True)

from scipy.spatial.distance import cosine
sims = {q: [1 - cosine(q_rep[i], d_rep[j]) for j in range(len(d_rep))] for i, q in enumerate(queries)}

for i, (q, q_sims) in enumerate(sims.items()):
    sim_idx = np.argmax(q_sims)
    cache_query = tuple([
        (q_cache[j][0][i:i+1], q_cache[j][1][i:i+1]) for j, c in enumerate(q_cache)
    ])
    cache_doc = tuple([
        (d_cache[j][0][sim_idx:sim_idx+1], d_cache[j][1][sim_idx:sim_idx+1]) for j, c in enumerate(d_cache)
    ])
    # For DOC-QUERY simply swap the order of the cache, change the format to CACHE_FORMAT_DOC_QUERY & set add_special_tokens=True in the `model.encode(..` above
    cache = [(
        torch.cat((layer[0], cache_doc[i][0]), dim=2),
        torch.cat((layer[1], cache_doc[i][1]), dim=2),
    ) for i, layer in enumerate(cache_query)]
    # BOS is already in the cache
    inputs = model.tokenizer(CACHE_FORMAT_QUERY_DOC, return_tensors="pt", add_special_tokens=False).to(model.device)
    inputs["use_cache"] = True
    # Attend to the cache too
    inputs["attention_mask"] = torch.cat((
        torch.ones((cache[0][0].shape[0], cache[0][0].shape[2]), dtype=torch.long, device=inputs["attention_mask"].device),
        inputs["attention_mask"],
    ), dim=1)
    generation = model.generate(**inputs, max_new_tokens=256, past_key_values=cache, do_sample=False)
    decoded = model.tokenizer.batch_decode(generation)
    print(decoded[0])

"""
<|user|>
Optionally using the prior context answer the query prior to it
<|assistant|>
Sure, here's an example of how the prior context could be used to answer a query:

Query: "What is GRIT?"

Prior context: "We introduce generative representation instruction tuning (GRIT) whereby a large language model is trained to handle both generative and embedding tasks by distinguishing between them through instructions."

Answer: GRIT is a method for training language models to handle both generative and embedding tasks by distinguishing between them through instructions.</s>
"""
```
