
# A Visual Guide to LLM Agents

> 来源：[https://gotoaiworld.feishu.cn/docx/DzJYdATyfoRC2ZxU68oc0cfSnOd](https://gotoaiworld.feishu.cn/docx/DzJYdATyfoRC2ZxU68oc0cfSnOd)

LLM Agents are becoming widespread, seemingly taking over the “regular” conversational LLM we are familiar with. These incredible capabilities are not easily created and require many components working in tandem.

![](img/0c04459206ba6043ccb01410658d939a.png)

With over 60 custom visuals in this post, you will explore the field of LLM Agents, their main components, and explore Multi-Agent frameworks.

Thank you for reading Exploring Language Models! Subscribe to receive new posts on Gen AI and the book: Hands-On Large Language Models

Subscribe

👈 click on the stack of lines on the left to see a Table of Contents (ToC)

# What Are LLM Agents?

To understand what LLM Agents are, let us first explore the basic capabilities of an LLM. Traditionally, an LLM does nothing more than next-token prediction.

![](img/5995180889eb4602db2ee3d745117f54.png)

By sampling many tokens in a row, we can mimic conversations and use the LLM to give more extensive answers to our queries.

![](img/d02f42e1581ee6a13a2f2e052c209c7d.png)

However, when we continue the “conversation”, any given LLM will showcase one of its main disadvantages. It does not remember conversations!

![](img/7fe213eaf3ba0be38dfd3015c3799a76.png)

There are many other tasks that LLMs often fail at, including basic math like multiplication and division:

![](img/c9bf85b7cd9b2c3ebeabe14951a4b83c.png)

Does this mean LLMs are horrible? Definitely not! There is no need for LLMs to be capable of everything as we can compensate for their disadvantage through external tools, memory, and retrieval systems.

Through external systems, the capabilities of the LLM can be enhanced. Anthropic calls this “The Augmented LLM”.

![](img/7ba1bd8ec8c19ff9c87a6a6112a2a044.png)

For instance, when faced with a math question, the LLM may decide to use the appropriate tool (a calculator).

![](img/66909cbb5a5d413bec7c5988c262e495.png)

So is this “Augmented LLM” then an Agent? No, and maybe a bit yes…

Let’s start with a d[efi](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-llm-agents#footnote-1-156659273)nition of Agents:(1)

An agent is anything that can be viewed as perceiving its environment through sensors and acting upon that environment through actuators.

— Russell & Norvig, AI: A Modern Approach (2016)

Agents interact with their environment and typically consist of several important components:

+ Environments — The world the agent interacts with

+ Sensors — Used to observe the environment

+ Actuators — Tools used to interact with the environment

+ Effectors — The “brain” or rules deciding how to go from observations to actions

![](img/d185d746f6a3cbf6851d68306e6478af.png)

This framework is used for all kinds of agents that interact with all kinds of environments, like robots interacting with their physical environment or AI agents interacting with software.

We can generalize this framework a bit to make it suitable for the “Augmented LLM” instead.

![](img/bd5869a0c607f1d0b46b1be70ecf278d.png)

Using the “Augmented” LLM, the Agent can observe the environment through textual input (as LLMs are generally textual models) and perform certain actions through its use of tools (like searching the web).

To select which actions to take, the LLM Agent has a vital component: its ability to plan. For this, LLMs need to be able to “reason” and “think” through methods like chain-of-thought.

![](img/231392449aeae45e3ac0bbbadd5afd25.png)

For more information about reasoning, check out The Visual Guide to Reasoning LLMs

Using this reasoning behavior, LLM Agents will plan out the necessary actions to take.

![](img/506a79d7617696ea19c1d3bbfd90d406.png)

This planning behavior allows the Agent to understand the situation (LLM), plan next steps (planning), take actions (tools), and keep track of the taken actions (memory).

![](img/c4a899d315aa06995c6008905932e42a.png)

Depending on the system, you can LLM Agents with varying degrees of autonomy.

![](img/ebe4a6ce0cc9598f3a7444cf99e00dc5.png)

Depending on who you ask, a system is more “agentic” the more the LLM decides how the system can behave.

In the next sections, we will go through various methods of autonomous behavior through the LLM Agent’s three main components: Memory, Tools, and Planning.

# Memory

LLMs are forgetful systems, or more accurately, do not perform any memorization at all when interacting with them.

For instance, when you ask an LLM a question and then follow it up with another question, it will not remember the former.

![](img/03d9bd2af8326a14322528a9cd6063e0.png)

We typically refer to this as short-term memory, also called working memory, which functions as a buffer for the (near-) immediate context. This includes recent actions the LLM Agent has taken.

However, the LLM Agent also needs to keep track of potentially dozens of steps, not only the most recent actions.

![](img/ca9619d528fb208d296ebdceec277c78.png)

This is referred to as long-term memory as the LLM Agent could theoretically take dozens or even hundreds of steps that need to be memorized.

![](img/7ed9441ffcf032783e904e8bf668bd44.png)

Let’s explore several tricks for giving these models memory.

## Short-Term Memory

The most straightforward method for enabling short-term memory is to use the model's context window, which is essentially the number of tokens an LLM can process.

![](img/c47328a24fe2a3b37e0edee3e0f85407.png)

The context window tends to be at least 8192 tokens and sometimes can scale up to hundreds of thousands of tokens!

A large context window can be used to track the full conversation history as part of the input prompt.

[https://substackcdn.com/image/fetch/$s_!12fR!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/](https://substackcdn.com/image/fetch/$s_!12fR!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https://substack-post-media.s3.amazonaws.com/public/images/66e6afca-afc6-4a3f-a4b0-4d11e050c558_1204x616.png)https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F66e6afca-afc6-4a3f-a4b0-4d11e050c558_1204x616.png

This works as long as the conversation history fits within the LLM’s context window and is a nice way of mimicking memory. However, instead of actually memorizing a conversation, we essentially “tell” the LLM what that conversation was.

For models with a smaller context window, or when the conversation history is large, we can instead use another LLM to summarize the conversations that happened thus far.

![](img/c8bc30a6cf650bce2fe5130fb08e7683.png)

By continuously summarizing conversations, we can keep the size of this conversation small. It will reduce the number of tokens while keeping track of only the most vital information.

## Long-term Memory

Long-term memory in LLM Agents includes the agent’s past action space that needs to be retained over an extended period.

A common technique to enable long-term memory is to store all previous interactions, actions, and conversations in an external vector database.

To build such a database, conversations are first embedded into numerical representations that capture their meaning.

![](img/4a308a7a9bae21692be3cc58be133a3a.png)

After building the database, we can embed any given prompt and find the most relevant information in the vector database by comparing the prompt embedding with the database embeddings.

![](img/8430f317ba89dbbbe786ddfc23c993c9.png)

This method is often referred to as Retrieval-Augmented Generation (RAG).

Long-term memory can also involve retaining information from different sessions. For instance, you might want an LLM Agent to remember any research it has done in previous sessions.
