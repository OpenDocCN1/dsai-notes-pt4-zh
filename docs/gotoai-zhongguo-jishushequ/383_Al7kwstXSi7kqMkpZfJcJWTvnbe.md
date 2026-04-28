# Building Effective AI Agents: A Guide for Startups

> 来源：[https://gotoaiworld.feishu.cn/docx/TmNXdulW5onAf1xo12AcEQaLnWh](https://gotoaiworld.feishu.cn/docx/TmNXdulW5onAf1xo12AcEQaLnWh)

![](img/fc3191d8bfe319f245d3141a5dfa5180.png)

https://www.lomitpatel.com/articles/building-effective-ai-agents/

If you’re [a startup](https://www.lomitpatel.com/articles/a-startup/) fo[un](https://www.lomitpatel.com/articles/how-ai-can-make-marketing-more-effective-without-touching-creative/)der, investor, or marketing leader, you might be wondering how to leverage artificial intelligence. The key is building effective AI agents, but there can be a learning curve.

About 10% of companies are currently using AI agents, but a significant 82% plan to implement them soon. This data comes from a Capgemini survey of 1,100 executives, highlighting the rapidly growing interest in building effective AI agents.

## Table Of Contents:

+   Defining Agentic Systems

+   When Not To Use Agents

+   Smart Ways to Use AI Frameworks

+   [S](https://www.lomitpatel.com/articles/building-effective-ai-agents/#startingwiththebasicsforbuildingeffectiveaiagents)tarting with the Basics for Building Effective AI Agents

+   Ways to Structure AI Systems

+   Prompt Chaining Workflow

+   Routing Workflow

+   Parallelization Workflow

+   Orchestrator-Workers Workflow

+   Evaluator-Optimizer Workflow

+   Working with Agents

+   [Prompt Design](https://www.lomitpatel.com/articles/building-effective-ai-agents/#promptdesign)

+   [Tips for Improving ](https://www.lomitpatel.com/articles/building-effective-ai-agents/#tipsforimprovingtoolswithaiagents)Tools with AI Agents

+   Combining and Improving Patterns

+   [Conclusion](https://www.lomitpatel.com/articles/building-effective-ai-agents/#conclusion)

## Defining Agentic Systems

The term “agent” is widely used, often with varying interpretations. Even top-performing companies haven’t always used highly complex methods when building effective AI agents.

Some envision agents as autonomous robots. Others think of predefined workflows.

We classify these as “agentic systems,” categorized into two types: workflows and agents. Workflows follow pre-coded paths with specific tools, while agents use large language models (LLMs) to dynamically determine their path and tool usage to accomplish tasks.

Workflows provide control for well-defined tasks, while AI agents offer adaptability. However, a single, optimized call might suffice in some cases.

### When Not To Use Agents

When developing applications with LLMs, prioritize simplicity. Over-engineering early on is often counterproductive.

Using AI agents can increase latency and costs. Workflows are better suited for predictable tasks.

Carefully consider what approach aligns best with your objectives.

## Smart Ways to Use AI Frameworks

M[any frame](https://langchain-ai.github.io/langgraph)works exist to help create these systems, like LangGraph from LangChain. You also have Amazon’s AI Agent framework.

[Other](https://rivet.ironcladapp.com/) opti[ons in](https://www.vellum.ai/)clude Rivet and Vellum, which allows for the design and testing of intricate systems. These tools accelerate development, but the abstraction layers can obscure the underlying prompts.

Directly using APIs from LLMs can be a more efficient approach, enabling rapid setup. If using a framework, thoroughly understand the underlying code, as misinterpretations are a common source of errors.

### Starting with the Basics for Building Effective AI Agents

Begin with a robust LLM. Enhance it with features for searching, data retrieval, and data storage.

Modern AI models can also manage searches and identify information to retain. Adapt the system to meet your specific task and user requirements.

You can explore Anthropic’s Model Context Protocol, which allows developers to connect with other tools via a custom integration. Assume that each call to an LLM utilizes these capabilities moving forward.

## Ways to Structure AI Systems

Below are several methods for model development, learned from practical experience. We start simple and progressively increase in adding complexity.

### Prompt Chaining Workflow

Prompt chaining involves breaking down complex tasks into sequential steps. The system verifies each step to maintain accuracy.

This approach works well when tasks can be easily divided. It forgoes some immediate speed, allowing the model to handle a more manageable request.

For example, this strategy is effective in advertising creation or book writing, once the initial concepts are approved. It allows for iterative refinement and feedback incorporation.

### Routing Workflow

A routing workflow determines the nature of an input. It helps to categorize tasks and enhance messaging.

This system excels in complex scenarios where individual responses might be superior. This can involve content classification, perhaps using different models or mathematical operations.

It’s commonly used for handling customer inquiries, where different categories, such as refunds or basic support, receive customized responses. LLMs dynamically direct the user queries to be handled accurately.

### Parallelization Workflow

LLMs can collaborate on a task, with their outputs aggregated programmatically.

It has two main forms: sectioning and voting. Sectioning divides tasks, while voting provides multiple attempts from various angles.

This is beneficial when sections r[equire rapid](https://www.techjuice.pk/the-most-effective-ai-tools-for-academic-research) processing and you need an effective AI capable of generating diverse perspectives. For instance, consider a safety system where one AI agent handles tasks, while another monitors for potential risks.

### Orchestrator-Workers Workflow

Here, a central LLM, the orchestrator, assigns tasks. It delegates work to worker LLMs and then reviews the results to create a final output.

This method is highly effective for complex tasks, with assignments made on an as-needed basis based on the input received by the orchestrator. One example is in coding, affecting various parts of a codebase.

It’s also valuable for extensive research projects requiring analysis of diverse data sources for related information. This helps with performing tasks over multiple rounds.

### Evaluator-Optimizer Workflow

One component generates work, while another provides feedback.

This setup is ideal when clear evaluation criteria exist for improvement and benefits. The workflow fits scenarios where feedback clearly enhances outputs, similar to a human editor refining written content.

For example, an evaluator LLM could suggest improvements to an initial response, helping to better articulate the original concepts. AI technology can learn with this.

## Working with Agents

[AI Agents](https://www.lomitpatel.com/articles/ai-agents/) are becoming increasingly valuable for businesses as the underlying models continue to advance.

Once initiated, the model may require ongoing input. Predicting every step in advance is often impossible.

While this flexibility comes at a cost, it offers robust contro[l for lar](https://www.ibm.com/think/topics/ai-agents)ge-scale operations. Below are several tasks where AI agents excel.

One example is code debugging. Another, the “computer tasks” use case, enables an LLM to execute steps to achieve a specified goal.

## Prompt Design

Prompts for tool configuration warrant the same level of attention as the rest of the coding process. You might, for example, choose a file modification within a specific program.

Opting for markdown or a format readily accessible online is recommended. Providing the LLM with ample space to “think” will boost accuracy.

Adopt the perspective of a novice user and consider how to clarify prompts, providing guidance for newer team members, even in organizations specializing in building effective AI agents. Refine input details to minimize erroneous steps, reducing the likelihood of mistakes.

Experiment with different prompt variations and employ input models for testing and adjusting future behavior. Reference the Anthropic guide on Github.

Consider using frameworks that might help get the job done, these include: LangGraph and Amazon’s agent framework.

## Tips for Improving Tools with AI Agents

Regardless of the architecture, tools play a critical role in these systems. They enable interaction with external applications, allowing AI to make effective use of data for responses.

Communicate user cases and any required input in a format easily understandable by any model. Define tool boundaries by modifying descriptions.

Conduct thorough testing within real-world systems. Monitor for issues, such as naming conflicts, and address them to create reliable tools that avoid pathfinding errors.

Robust AI systems necessitate a strategic approach, prioritizing clear setups and concrete details. Validate implementations, utilizing a step-by-step methodology for integrating components in a practical and dependable manner. See the below table:

These AI agents are well-suited for handling one-on-one customer interactions.

They empower systems to interface with company data and knowledge bases. Tasks such as updating tickets are efficiently managed by AI.

### Combining and Improving Patterns

These concepts offer ways to work more efficiently. Make modifications according to user needs.

Measuring outcomes provides tangible improvements. Improvement should only be pursued if testing demonstrably improves results.

Consider the simplest method initially, and then gradually expand. If errors occur, revise the prompt.

Maintain transparency in plans to make the reasoning clear. Fine-tune tools to maintain a reliable, tested, and trustworthy process.

Teams have observed that AI agent usage can positively impact business results, indicating confidence in their performance. You can maintain the quality of the work delegated to an AI system by implementing adequate review processes, such as unit testing.

## Conclusion

Achieving success with LLMs doesn’t hinge on developing overly complex systems. It involves focusing on solutions aligned with your specific needs and building trust in your AI system’s ability to perform tasks for your target audience.

As models continue to improve, new opportunities emerge for businesses. Start building effective AI agents by beginning with simple implementations and iteratively enhancing them based on measured performance. Address concerns proactively.