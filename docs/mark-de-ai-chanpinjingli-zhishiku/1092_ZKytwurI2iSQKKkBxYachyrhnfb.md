# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/LxzBdQ2LUoxKfXxXy9uc90lLnag](https://qqs7y1hozd1.feishu.cn/docx/LxzBdQ2LUoxKfXxXy9uc90lLnag)

```
import streamlit as st
from agno.agent import Agent, RunEvent
from agno.embedder.openai import OpenAIEmbedder
from agno.knowledge.url import UrlKnowledge
from agno.models.anthropic import Claude
from agno.tools.reasoning import ReasoningTools
from agno.vectordb.lancedb import LanceDb, SearchType
from dotenv import load_dotenv
import os

# Load environment variables
load_dotenv()

# Page configuration
st.set_page_config(
    page_title=&quot;Agentic RAG with Reasoning&quot;, 
    page_icon=&quot;🧐&quot;, 
    layout=&quot;wide&quot;
)

# Main title and description
st.title(&quot;🧐 Agentic RAG with Reasoning&quot;)
st.markdown(&quot;&quot;&quot;
This app demonstrates an AI agent that:
1. **Retrieves** relevant information from knowledge sources
2. **Reasons** through the information step-by-step
3. **Answers** your questions with citations

Enter your API keys below to get started!
&quot;&quot;&quot;)

# API Keys Section
st.subheader(&quot;🔑 API Keys&quot;)
col1, col2 = st.columns(2)
with col1:
    anthropic_key = st.text_input(
        &quot;Anthropic API Key&quot;, 
        type=&quot;password&quot;,
        value=os.getenv(&quot;ANTHROPIC_API_KEY&quot;, &quot;&quot;),
        help=&quot;Get your key from https://console.anthropic.com/&quot;
    )
with col2:
    openai_key = st.text_input(
        &quot;OpenAI API Key&quot;, 
        type=&quot;password&quot;,
        value=os.getenv(&quot;OPENAI_API_KEY&quot;, &quot;&quot;),
        help=&quot;Get your key from https://platform.openai.com/&quot;
    )

# Check if API keys are provided
if anthropic_key and openai_key:
    
    # Initialize knowledge base (cached to avoid reloading)
    @st.cache_resource(show_spinner=&quot;📚 Loading knowledge base...&quot;)
    def load_knowledge() -&gt; UrlKnowledge:
        &quot;&quot;&quot;Load and initialize the knowledge base with vector database&quot;&quot;&quot;
        kb = UrlKnowledge(
            urls=[&quot;https://docs.agno.com/introduction/agents.md&quot;],  # Default URL
            vector_db=LanceDb(
                uri=&quot;tmp/lancedb&quot;,
                table_name=&quot;agno_docs&quot;,
                search_type=SearchType.vector,  # Use vector search
                embedder=OpenAIEmbedder(
                    api_key=openai_key
                ),
            ),
        )
        kb.load(recreate=True)  # Load documents into vector DB
        return kb

    # Initialize agent (cached to avoid reloading)
    @st.cache_resource(show_spinner=&quot;🤖 Loading agent...&quot;)
    def load_agent(_kb: UrlKnowledge) -&gt; Agent:
        &quot;&quot;&quot;Create an agent with reasoning capabilities&quot;&quot;&quot;
        return Agent(
            model=Claude(
                id=&quot;claude-sonnet-4-20250514&quot;, 
                api_key=anthropic_key
            ),
            knowledge=_kb,
            search_knowledge=True,  # Enable knowledge search
            tools=[ReasoningTools(add_instructions=True)],  # Add reasoning tools
            instructions=[
                &quot;Include sources in your response.&quot;,
                &quot;Always search your knowledge before answering the question.&quot;,
            ],
            markdown=True,  # Enable markdown formatting
        )

    # Load knowledge and agent
    knowledge = load_knowledge()
    agent = load_agent(knowledge)

    # Sidebar for knowledge management
    with st.sidebar:
        st.header(&quot;📚 Knowledge Sources&quot;)
        st.markdown(&quot;Add URLs to expand the knowledge base:&quot;)
        
        # Show current URLs
        st.write(&quot;**Current sources:**&quot;)
        for i, url in enumerate(knowledge.urls):
            st.text(f&quot;{i+1}. {url}&quot;)
        
        # Add new URL
        st.divider()
        new_url = st.text_input(
            &quot;Add new URL&quot;, 
            placeholder=&quot;https://example.com/docs&quot;,
            help=&quot;Enter a URL to add to the knowledge base&quot;
        )
        
        if st.button(&quot;➕ Add URL&quot;, type=&quot;primary&quot;):
            if new_url:
                with st.spinner(&quot;📥 Loading new documents...&quot;):
                    knowledge.urls.append(new_url)
                    knowledge.load(
                        recreate=False,  # Don&#x27;t recreate DB
                        upsert=True,     # Update existing docs
                        skip_existing=True  # Skip already loaded docs
                    )
                st.success(f&quot;✅ Added: {new_url}&quot;)
                st.rerun()  # Refresh to show new URL
            else:
                st.error(&quot;Please enter a URL&quot;)

    # Main query section
    st.divider()
    st.subheader(&quot;🤔 Ask a Question&quot;)
    
    # Query input
    query = st.text_area(
        &quot;Your question:&quot;,
        value=&quot;What are Agents?&quot;,
        height=100,
        help=&quot;Ask anything about the loaded knowledge sources&quot;
    )
    
    # Run button
    if st.button(&quot;🚀 Get Answer with Reasoning&quot;, type=&quot;primary&quot;):
        if query:
            # Create containers for streaming updates
            col1, col2 = st.columns([1, 1])
            
            with col1:
                st.markdown(&quot;### 🧠 Reasoning Process&quot;)
                reasoning_container = st.container()
                reasoning_placeholder = reasoning_container.empty()
            
            with col2:
                st.markdown(&quot;### 💡 Answer&quot;)
                answer_container = st.container()
                answer_placeholder = answer_container.empty()
            
            # Variables to accumulate content
            citations = []
            answer_text = &quot;&quot;
            reasoning_text = &quot;&quot;
            
            # Stream the agent&#x27;s response
            with st.spinner(&quot;🔍 Searching and reasoning...&quot;):
                for chunk in agent.run(
                    query,
                    stream=True,  # Enable streaming
                    show_full_reasoning=True,  # Show reasoning steps
                    stream_intermediate_steps=True,  # Stream intermediate updates
                ):
                    # Update reasoning display
                    if chunk.reasoning_content:
                        reasoning_text = chunk.reasoning_content
                        reasoning_placeholder.markdown(
                            reasoning_text, 
                            unsafe_allow_html=True
                        )
                    
                    # Update answer display
                    if chunk.content and chunk.event in {RunEvent.run_response, RunEvent.run_completed}:
                        if isinstance(chunk.content, str):
                            answer_text += chunk.content
                            answer_placeholder.markdown(
                                answer_text, 
                                unsafe_allow_html=True
                            )
                    
                    # Collect citations
                    if chunk.citations and chunk.citations.urls:
                        citations = chunk.citations.urls
            
            # Show citations if available
            if citations:
                st.divider()
                st.subheader(&quot;📚 Sources&quot;)
                for cite in citations:
                    title = cite.title or cite.url
                    st.markdown(f&quot;- [{title}]({cite.url})&quot;)
        else:
            st.error(&quot;Please enter a question&quot;)

else:
    # Show instructions if API keys are missing
    st.info(&quot;&quot;&quot;
    👋 **Welcome! To use this app, you need:**
    
    1. **Anthropic API Key** - For Claude AI model
       - Sign up at [console.anthropic.com](https://console.anthropic.com/)
    
    2. **OpenAI API Key** - For embeddings
       - Sign up at [platform.openai.com](https://platform.openai.com/)
    
    Once you have both keys, enter them above to start!
    &quot;&quot;&quot;)

# Footer with explanation
st.divider()
with st.expander(&quot;📖 How This Works&quot;):
    st.markdown(&quot;&quot;&quot;
    **This app uses the Agno framework to create an intelligent Q&amp;A system:**
    
    1. **Knowledge Loading**: URLs are processed and stored in a vector database (LanceDB)
    2. **Vector Search**: Uses OpenAI&#x27;s embeddings for semantic search to find relevant information
    3. **Reasoning Tools**: The agent uses special tools to think through problems step-by-step
    4. **Claude AI**: Anthropic&#x27;s Claude model processes the information and generates answers
    
    **Key Components:**
    - `UrlKnowledge`: Manages document loading from URLs
    - `LanceDb`: Vector database for efficient similarity search
    - `OpenAIEmbedder`: Converts text to embeddings using OpenAI&#x27;s embedding model
    - `ReasoningTools`: Enables step-by-step reasoning
    - `Agent`: Orchestrates everything to answer questions
    &quot;&quot;&quot;)
```