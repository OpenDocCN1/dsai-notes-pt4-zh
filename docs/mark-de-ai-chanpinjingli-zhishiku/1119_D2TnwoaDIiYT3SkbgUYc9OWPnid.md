# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/QWkUdy5RNoYb4dxa2eJc1O3Jnle](https://qqs7y1hozd1.feishu.cn/docx/QWkUdy5RNoYb4dxa2eJc1O3Jnle)

```
# Import necessary libraries
from agno.agent import Agent
from agno.models.ollama import Ollama
from agno.knowledge.pdf_url import PDFUrlKnowledgeBase
from agno.vectordb.qdrant import Qdrant
from agno.embedder.ollama import OllamaEmbedder
from agno.playground import Playground, serve_playground_app

# Define the collection name for the vector database
collection_name = &quot;thai-recipe-index&quot;

# Set up Qdrant as the vector database with the embedder
vector_db = Qdrant(
    collection=collection_name,
    url=&quot;http://localhost:6333/&quot;,
    embedder=OllamaEmbedder()
)

# Define the knowledge base with the specified PDF URL
knowledge_base = PDFUrlKnowledgeBase(
    urls=[&quot;https://phi-public.s3.amazonaws.com/recipes/ThaiRecipes.pdf&quot;],
    vector_db=vector_db,
)

# Load the knowledge base, comment out after the first run to avoid reloading
knowledge_base.load(recreate=True, upsert=True)

# Create the Agent using Ollama&#x27;s llama3.2 model and the knowledge base
agent = Agent(
    name=&quot;Local RAG Agent&quot;,
    model=Ollama(id=&quot;llama3.2&quot;),
    knowledge=knowledge_base,
)

# UI for RAG agent
app = Playground(agents=[agent]).get_app()

# Run the Playground app
if __name__ == &quot;__main__&quot;:
    serve_playground_app(&quot;local_rag_agent:app&quot;, reload=True)
```