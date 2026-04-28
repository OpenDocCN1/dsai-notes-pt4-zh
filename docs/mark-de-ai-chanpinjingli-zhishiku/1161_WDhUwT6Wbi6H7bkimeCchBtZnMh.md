# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/GGdpdLS1ioDElBxxbpocwj6jnaf](https://qqs7y1hozd1.feishu.cn/docx/GGdpdLS1ioDElBxxbpocwj6jnaf)

```
from agno.agent import Agent
from agno.models.ollama import Ollama
from agno.playground import Playground, serve_playground_app

reasoning_agent = Agent(name=&quot;Reasoning Agent&quot;, model=Ollama(id=&quot;qwq:32b&quot;), markdown=True)

# UI for Reasoning agent
app = Playground(agents=[reasoning_agent]).get_app()

# Run the Playground app
if __name__ == &quot;__main__&quot;:
    serve_playground_app(&quot;local_ai_reasoning_agent:app&quot;, reload=True)
```

```
from agno.agent import Agent
from agno.models.openai import OpenAIChat
from rich.console import Console

regular_agent = Agent(model=OpenAIChat(id=&quot;gpt-4o-mini&quot;), markdown=True)
console = Console()
reasoning_agent = Agent(
    model=OpenAIChat(id=&quot;gpt-4o&quot;),
    reasoning=True,
    markdown=True,
    structured_outputs=True,
)

task = &quot;How many &#x27;r&#x27; are in the word &#x27;supercalifragilisticexpialidocious&#x27;?&quot;

console.rule(&quot;[bold green]Regular Agent[/bold green]&quot;)
regular_agent.print_response(task, stream=True)
console.rule(&quot;[bold yellow]Reasoning Agent[/bold yellow]&quot;)
reasoning_agent.print_response(task, stream=True, show_full_reasoning=True)
```