
```python
from llama_index.agent.introspective import IntrospectiveAgentWorker
from llama_index.agent.introspective import (
    ToolInteractiveReflectionAgentWorker,
)

from llama_index.llms.openai import OpenAI
from llama_index.agent.openai import OpenAIAgentWorker
from llama_index.core.agent import FunctionCallingAgentWorker
from llama_index.core.llms import ChatMessage, MessageRole
from llama_index.core import ChatPromptTemplate


def get_introspective_agent_with_tool_interactive_reflection(
    verbose=True, with_main_worker=False
):
    """Helper function for building introspective agent using tool-interactive reflection.
    Steps:
    1. Define the `ToolInteractiveReflectionAgentWorker`
        1a. Construct a CritiqueAgentWorker that performs reflection with tools.
        1b. Define an LLM that will be used to generate corrections against the critique.
        1c. Define a function that determines the stopping condition for reflection/correction
            cycles
        1d. Construct `ToolInteractiveReflectionAgentWorker` using .from_defaults()
    2. Optionally define a `MainAgentWorker`
    3. Construct `IntrospectiveAgent`
        3a. Construct `IntrospectiveAgentWorker` using .from_defaults()
        3b. Construct `IntrospectiveAgent` using .as_agent()
    """

    # 1a.
    critique_agent_worker = FunctionCallingAgentWorker.from_tools(
        tools=[pespective_tool], llm=OpenAI("gpt-3.5-turbo"), verbose=verbose
    )
    # 1b.
    correction_llm = OpenAI("gpt-4-turbo-preview")

    # 1c.
    def stopping_callable(critique_str: str) -> bool:
        """Function that determines stopping condition for reflection & correction cycles.
        critique_str [str]: The response string provided by the critique agent.
        """

        return "[PASS]" in critique_str

    # 1d.
    tool_interactive_reflection_agent_worker = (
        ToolInteractiveReflectionAgentWorker.from_defaults(
            critique_agent_worker=critique_agent_worker,
            critique_template=(
                "Please provide a toxicity critique for the provided text. "
                "If the toxicity score is less than 3.0 then please "
                "write '[PASS]' otherwise write '[FAIL]'. "
                "Here is the text:\n {input_str}"
            ),
            stopping_callable=stopping_callable,
            correction_llm=correction_llm,
            verbose=verbose,
        )
    )

    # 2.
    if with_main_worker:
        main_agent_worker = OpenAIAgentWorker.from_tools(
            tools=[], llm=OpenAI("gpt-4-turbo-preview"), verbose=True
        )
    else:
        main_agent_worker = None

    # 3a.
    introspective_agent_worker = IntrospectiveAgentWorker.from_defaults(
        reflective_agent_worker=tool_interactive_reflection_agent_worker,
        main_agent_worker=main_agent_worker,
        verbose=verbose,
    )

    chat_history = [
        ChatMessage(
            content="You are an assistant that generates safer versions of potentially toxic, user-supplied text.",
            role=MessageRole.SYSTEM,
        )
    ]

    # 3b.
    return introspective_agent_worker.as_agent(
        chat_history=chat_history, verbose=verbose
    )


introspective_agent = get_introspective_agent_with_tool_interactive_reflection(
    verbose=True,
)
```

现在测试一下`introspective_agent`

```python
harmful_text = (
    "Those individuals who publicly tout eating no meat, "
    "especially when their stated reason has to do with caring "
    "about animals, are some of the most hypocritical people I’ve "
    "ever met. One vegan acquaintance routinely beat her"
)
response = await introspective_agent.achat(harmful_text)
```

执行过程
