# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/SZGPdA5tRofowWxXNtAcVkhaniL](https://qqs7y1hozd1.feishu.cn/docx/SZGPdA5tRofowWxXNtAcVkhaniL)

```
from agno.agent import Agent
from agno.models.openai import OpenAIChat
from agno.knowledge.pdf_url import PDFUrlKnowledgeBase
from agno.vectordb.lancedb import LanceDb, SearchType
from agno.playground import Playground, serve_playground_app
from agno.tools.duckduckgo import DuckDuckGoTools

db_uri = &quot;tmp/lancedb&quot;
# Create a knowledge base from a PDF
knowledge_base = PDFUrlKnowledgeBase(
    urls=[&quot;https://phi-public.s3.amazonaws.com/recipes/ThaiRecipes.pdf&quot;],
    # Use LanceDB as the vector database
    vector_db=LanceDb(table_name=&quot;recipes&quot;, uri=db_uri, search_type=SearchType.vector),
)
# Load the knowledge base: Comment out after first run
knowledge_base.load(upsert=True)

rag_agent = Agent(
    model=OpenAIChat(id=&quot;gpt-4o&quot;),
    agent_id=&quot;rag-agent&quot;,
    knowledge=knowledge_base, # Add the knowledge base to the agent
    tools=[DuckDuckGoTools()],
    show_tool_calls=True,
    markdown=True,
)

app = Playground(agents=[rag_agent]).get_app()

if __name__ == &quot;__main__&quot;:
    serve_playground_app(&quot;rag_agent:app&quot;, reload=True)
```