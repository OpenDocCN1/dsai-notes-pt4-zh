# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Ia6odzyaAoJkUbxEnNncbgRnntc](https://qqs7y1hozd1.feishu.cn/docx/Ia6odzyaAoJkUbxEnNncbgRnntc)

```
import os
import tempfile
from datetime import datetime
from typing import List
import streamlit as st
import bs4
from agno.agent import Agent
from agno.models.ollama import Ollama
from langchain_community.document_loaders import PyPDFLoader, WebBaseLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_qdrant import QdrantVectorStore
from qdrant_client import QdrantClient
from qdrant_client.models import Distance, VectorParams
from langchain_core.embeddings import Embeddings
from agno.tools.exa import ExaTools
from agno.embedder.ollama import OllamaEmbedder


class OllamaEmbedderr(Embeddings):
    def __init__(self, model_name=&quot;snowflake-arctic-embed&quot;):
        &quot;&quot;&quot;
        Initialize the OllamaEmbedderr with a specific model.

        Args:
            model_name (str): The name of the model to use for embedding.
        &quot;&quot;&quot;
        self.embedder = OllamaEmbedder(id=model_name, dimensions=1024)

    def embed_documents(self, texts: List[str]) -&gt; List[List[float]]:
        return [self.embed_query(text) for text in texts]

    def embed_query(self, text: str) -&gt; List[float]:
        return self.embedder.get_embedding(text)


# Constants
COLLECTION_NAME = &quot;test-qwen-r1&quot;


# Streamlit App Initialization
st.title(&quot;🐋 Qwen 3 Local RAG Reasoning Agent&quot;)

# --- Add Model Info Boxes --- 
st.info(&quot;**Qwen3:** The latest generation of large language models in Qwen series, offering a comprehensive suite of dense and mixture-of-experts (MoE) models.&quot;)
st.info(&quot;**Gemma 3:** These models are multimodal—processing text and images—and feature a 128K context window with support for over 140 languages.&quot;)
# -------------------------

# Session State Initialization
if &#x27;model_version&#x27; not in st.session_state:
    st.session_state.model_version = &quot;qwen3:1.7b&quot;  # Default to lighter model
if &#x27;vector_store&#x27; not in st.session_state:
    st.session_state.vector_store = None
if &#x27;processed_documents&#x27; not in st.session_state:
    st.session_state.processed_documents = []
if &#x27;history&#x27; not in st.session_state:
    st.session_state.history = []
if &#x27;exa_api_key&#x27; not in st.session_state:
    st.session_state.exa_api_key = &quot;&quot;
if &#x27;use_web_search&#x27; not in st.session_state:
    st.session_state.use_web_search = False
if &#x27;force_web_search&#x27; not in st.session_state:
    st.session_state.force_web_search = False
if &#x27;similarity_threshold&#x27; not in st.session_state:
    st.session_state.similarity_threshold = 0.7
if &#x27;rag_enabled&#x27; not in st.session_state:
    st.session_state.rag_enabled = True  # RAG is enabled by default


# Sidebar Configuration
st.sidebar.header(&quot;⚙️ Settings&quot;)

# Model Selection
st.sidebar.header(&quot;🧠 Model Choice&quot;)
model_help = &quot;&quot;&quot;
- qwen3:1.7b: Lighter model (MoE)
- gemma3:1b: More capable but requires better GPU/RAM(32k context window)
- gemma3:4b: More capable and MultiModal (Vision)(128k context window)
- deepseek-r1:1.5b
- qwen3:8b: More capable but requires better GPU/RAM

Choose based on your hardware capabilities.
&quot;&quot;&quot;
st.session_state.model_version = st.sidebar.radio(
    &quot;Select Model Version&quot;,
    options=[&quot;qwen3:1.7b&quot;, &quot;gemma3:1b&quot;, &quot;gemma3:4b&quot;, &quot;deepseek-r1:1.5b&quot;, &quot;qwen3:8b&quot;],
    help=model_help
)

st.sidebar.info(&quot;Run ollama pull qwen3:1.7b&quot;)

# RAG Mode Toggle
st.sidebar.header(&quot;📚 RAG Mode&quot;)
st.session_state.rag_enabled = st.sidebar.toggle(&quot;Enable RAG&quot;, value=st.session_state.rag_enabled)

# Clear Chat Button
if st.sidebar.button(&quot;✨ Clear Chat&quot;):
    st.session_state.history = []
    st.rerun()

# Show API Configuration only if RAG is enabled
if st.session_state.rag_enabled:
    st.sidebar.header(&quot;🔬 Search Tuning&quot;)
    st.session_state.similarity_threshold = st.sidebar.slider(
        &quot;Similarity Threshold&quot;,
        min_value=0.0,
        max_value=1.0,
        value=0.7,
        help=&quot;Lower values will return more documents but might be less relevant. Higher values are more strict.&quot;
    )

# Add in the sidebar configuration section, after the existing API inputs

st.sidebar.header(&quot;🌍 Web Search&quot;)
st.session_state.use_web_search = st.sidebar.checkbox(&quot;Enable Web Search Fallback&quot;, value=st.session_state.use_web_search)

if st.session_state.use_web_search:
    exa_api_key = st.sidebar.text_input(
        &quot;Exa AI API Key&quot;, 
        type=&quot;password&quot;,
        value=st.session_state.exa_api_key,
        help=&quot;Required for web search fallback when no relevant documents are found&quot;
    )
    st.session_state.exa_api_key = exa_api_key
    
    # Optional domain filtering
    default_domains = [&quot;arxiv.org&quot;, &quot;wikipedia.org&quot;, &quot;github.com&quot;, &quot;medium.com&quot;]
    custom_domains = st.sidebar.text_input(
        &quot;Custom domains (comma-separated)&quot;, 
        value=&quot;,&quot;.join(default_domains),
        help=&quot;Enter domains to search from, e.g.: arxiv.org,wikipedia.org&quot;
    )
    search_domains = [d.strip() for d in custom_domains.split(&quot;,&quot;) if d.strip()]

# Utility Functions
def init_qdrant() -&gt; QdrantClient | None:
    &quot;&quot;&quot;Initialize Qdrant client with local Docker setup.

    Returns:
        QdrantClient: The initialized Qdrant client if successful.
        None: If the initialization fails.
    &quot;&quot;&quot;
    try:
        return QdrantClient(url=&quot;http://localhost:6333&quot;)
    except Exception as e:
        st.error(f&quot;🔴 Qdrant connection failed: {str(e)}&quot;)
        return None


# Document Processing Functions
def process_pdf(file) -&gt; List:
    &quot;&quot;&quot;Process PDF file and add source metadata.&quot;&quot;&quot;
    try:
        with tempfile.NamedTemporaryFile(delete=False, suffix=&#x27;.pdf&#x27;) as tmp_file:
            tmp_file.write(file.getvalue())
            loader = PyPDFLoader(tmp_file.name)
            documents = loader.load()
            
            # Add source metadata
            for doc in documents:
                doc.metadata.update({
                    &quot;source_type&quot;: &quot;pdf&quot;,
                    &quot;file_name&quot;: file.name,
                    &quot;timestamp&quot;: datetime.now().isoformat()
                })
                
            text_splitter = RecursiveCharacterTextSplitter(
                chunk_size=1000,
                chunk_overlap=200
            )
            return text_splitter.split_documents(documents)
    except Exception as e:
        st.error(f&quot;📄 PDF processing error: {str(e)}&quot;)
        return []


def process_web(url: str) -&gt; List:
    &quot;&quot;&quot;Process web URL and add source metadata.&quot;&quot;&quot;
    try:
        loader = WebBaseLoader(
            web_paths=(url,),
            bs_kwargs=dict(
                parse_only=bs4.SoupStrainer(
                    class_=(&quot;post-content&quot;, &quot;post-title&quot;, &quot;post-header&quot;, &quot;content&quot;, &quot;main&quot;)
                )
            )
        )
        documents = loader.load()
        
        # Add source metadata
        for doc in documents:
            doc.metadata.update({
                &quot;source_type&quot;: &quot;url&quot;,
                &quot;url&quot;: url,
                &quot;timestamp&quot;: datetime.now().isoformat()
            })
            
        text_splitter = RecursiveCharacterTextSplitter(
            chunk_size=1000,
            chunk_overlap=200
        )
        return text_splitter.split_documents(documents)
    except Exception as e:
        st.error(f&quot;🌐 Web processing error: {str(e)}&quot;)
        return []


# Vector Store Management
def create_vector_store(client, texts):
    &quot;&quot;&quot;Create and initialize vector store with documents.&quot;&quot;&quot;
    try:
        # Create collection if needed
        try:
            client.create_collection(
                collection_name=COLLECTION_NAME,
                vectors_config=VectorParams(
                    size=1024,  
                    distance=Distance.COSINE
                )
            )
            st.success(f&quot;📚 Created new collection: {COLLECTION_NAME}&quot;)
        except Exception as e:
            if &quot;already exists&quot; not in str(e).lower():
                raise e
        
        # Initialize vector store
        vector_store = QdrantVectorStore(
            client=client,
            collection_name=COLLECTION_NAME,
            embedding=OllamaEmbedderr()
        )
        
        # Add documents
        with st.spinner(&#x27;📤 Uploading documents to Qdrant...&#x27;):
            vector_store.add_documents(texts)
            st.success(&quot;✅ Documents stored successfully!&quot;)
            return vector_store
            
    except Exception as e:
        st.error(f&quot;🔴 Vector store error: {str(e)}&quot;)
        return None

def get_web_search_agent() -&gt; Agent:
    &quot;&quot;&quot;Initialize a web search agent.&quot;&quot;&quot;
    return Agent(
        name=&quot;Web Search Agent&quot;,
        model=Ollama(id=&quot;llama3.2&quot;),
        tools=[ExaTools(
            api_key=st.session_state.exa_api_key,
            include_domains=search_domains,
            num_results=5
        )],
        instructions=&quot;&quot;&quot;You are a web search expert. Your task is to:
        1. Search the web for relevant information about the query
        2. Compile and summarize the most relevant information
        3. Include sources in your response
        &quot;&quot;&quot;,
        show_tool_calls=True,
        markdown=True,
    )


def get_rag_agent() -&gt; Agent:
    &quot;&quot;&quot;Initialize the main RAG agent.&quot;&quot;&quot;
    return Agent(
        name=&quot;Qwen 3 RAG Agent&quot;,
        model=Ollama(id=st.session_state.model_version),
        instructions=&quot;&quot;&quot;You are an Intelligent Agent specializing in providing accurate answers.

        When asked a question:
        - Analyze the question and answer the question with what you know.
        
        When given context from documents:
        - Focus on information from the provided documents
        - Be precise and cite specific details
        
        When given web search results:
        - Clearly indicate that the information comes from web search
        - Synthesize the information clearly
        
        Always maintain high accuracy and clarity in your responses.
        &quot;&quot;&quot;,
        show_tool_calls=True,
        markdown=True,
    )




def check_document_relevance(query: str, vector_store, threshold: float = 0.7) -&gt; tuple[bool, List]:

    if not vector_store:
        return False, []
        
    retriever = vector_store.as_retriever(
        search_type=&quot;similarity_score_threshold&quot;,
        search_kwargs={&quot;k&quot;: 5, &quot;score_threshold&quot;: threshold}
    )
    docs = retriever.invoke(query)
    return bool(docs), docs


chat_col, toggle_col = st.columns([0.9, 0.1])

with chat_col:
    prompt = st.chat_input(&quot;Ask about your documents...&quot; if st.session_state.rag_enabled else &quot;Ask me anything...&quot;)

with toggle_col:
    st.session_state.force_web_search = st.toggle(&#x27;🌐&#x27;, help=&quot;Force web search&quot;)

# Check if RAG is enabled 
if st.session_state.rag_enabled:
    qdrant_client = init_qdrant()
    
    # --- Document Upload Section (Moved to Main Area) ---
    with st.expander(&quot;📁 Upload Documents or URLs for RAG&quot;, expanded=False):
        if not qdrant_client:
            st.warning(&quot;⚠️ Please configure Qdrant API Key and URL in the sidebar to enable document processing.&quot;)
        else:
            uploaded_files = st.file_uploader(
                &quot;Upload PDF files&quot;, 
                accept_multiple_files=True, 
                type=&#x27;pdf&#x27;
            )
            url_input = st.text_input(&quot;Enter URL to scrape&quot;)

            if uploaded_files:
                st.write(f&quot;Processing {len(uploaded_files)} PDF file(s)...&quot;)
                all_texts = []
                for file in uploaded_files:
                    if file.name not in st.session_state.processed_documents:
                        with st.spinner(f&quot;Processing {file.name}... &quot;): 
                            texts = process_pdf(file)
                            if texts: 
                                all_texts.extend(texts)
                                st.session_state.processed_documents.append(file.name)
                    else:
                        st.write(f&quot;📄 {file.name} already processed.&quot;)
                
                if all_texts:
                    with st.spinner(&quot;Creating vector store...&quot;):
                        st.session_state.vector_store = create_vector_store(qdrant_client, all_texts)

            if url_input:
                if url_input not in st.session_state.processed_documents:
                    with st.spinner(f&quot;Scraping and processing {url_input}...&quot;):
                        texts = process_web(url_input)
                        if texts:
                            st.session_state.vector_store = create_vector_store(qdrant_client, texts)
                            st.session_state.processed_documents.append(url_input)
                else:
                    st.write(f&quot;🔗 {url_input} already processed.&quot;)
                    
            if st.session_state.vector_store:
                st.success(&quot;Vector store is ready.&quot;)
            elif not uploaded_files and not url_input:
                 st.info(&quot;Upload PDFs or enter a URL to populate the vector store.&quot;)

    # Display sources in sidebar
    if st.session_state.processed_documents:
        st.sidebar.header(&quot;📚 Processed Sources&quot;)
        for source in st.session_state.processed_documents:
            if source.endswith(&#x27;.pdf&#x27;):
                st.sidebar.text(f&quot;📄 {source}&quot;)
            else:
                st.sidebar.text(f&quot;🌐 {source}&quot;)

if prompt:
    # Add user message to history
    st.session_state.history.append({&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: prompt})
    with st.chat_message(&quot;user&quot;):
        st.write(prompt)

    if st.session_state.rag_enabled:

            # Existing RAG flow remains unchanged
            with st.spinner(&quot;🤔Evaluating the Query...&quot;):
                try:
                    rewritten_query = prompt
                    
                    with st.expander(&quot;Evaluating the query&quot;):
                        st.write(f&quot;User&#x27;s Prompt: {prompt}&quot;)
                except Exception as e:
                    st.error(f&quot;❌ Error rewriting query: {str(e)}&quot;)
                    rewritten_query = prompt

            # Step 2: Choose search strategy based on force_web_search toggle
            context = &quot;&quot;
            docs = []
            if not st.session_state.force_web_search and st.session_state.vector_store:
                # Try document search first
                retriever = st.session_state.vector_store.as_retriever(
                    search_type=&quot;similarity_score_threshold&quot;,
                    search_kwargs={
                        &quot;k&quot;: 5, 
                        &quot;score_threshold&quot;: st.session_state.similarity_threshold
                    }
                )
                docs = retriever.invoke(rewritten_query)
                if docs:
                    context = &quot;\n\n&quot;.join([d.page_content for d in docs])
                    st.info(f&quot;📊 Found {len(docs)} relevant documents (similarity &gt; {st.session_state.similarity_threshold})&quot;)
                elif st.session_state.use_web_search:
                    st.info(&quot;🔄 No relevant documents found in database, falling back to web search...&quot;)

            # Step 3: Use web search if:
            # 1. Web search is forced ON via toggle, or
            # 2. No relevant documents found AND web search is enabled in settings
            if (st.session_state.force_web_search or not context) and st.session_state.use_web_search and st.session_state.exa_api_key:
                with st.spinner(&quot;🔍 Searching the web...&quot;):
                    try:
                        web_search_agent = get_web_search_agent()
                        web_results = web_search_agent.run(rewritten_query).content
                        if web_results:
                            context = f&quot;Web Search Results:\n{web_results}&quot;
                            if st.session_state.force_web_search:
                                st.info(&quot;ℹ️ Using web search as requested via toggle.&quot;)
                            else:
                                st.info(&quot;ℹ️ Using web search as fallback since no relevant documents were found.&quot;)
                    except Exception as e:
                        st.error(f&quot;❌ Web search error: {str(e)}&quot;)

            # Step 4: Generate response using the RAG agent
            with st.spinner(&quot;🤖 Thinking...&quot;):
                try:
                    rag_agent = get_rag_agent()
                    
                    if context:
                        full_prompt = f&quot;&quot;&quot;Context: {context}

Original Question: {prompt}
Please provide a comprehensive answer based on the available information.&quot;&quot;&quot;
                    else:
                        full_prompt = f&quot;Original Question: {prompt}\n&quot;
                        st.info(&quot;ℹ️ No relevant information found in documents or web search.&quot;)

                    response = rag_agent.run(full_prompt)
                    
                    # Add assistant response to history
                    st.session_state.history.append({
                        &quot;role&quot;: &quot;assistant&quot;,
                        &quot;content&quot;: response.content
                    })
                    
                    # Display assistant response
                    with st.chat_message(&quot;assistant&quot;):
                        st.write(response.content)
                        
                        # Show sources if available
                        if not st.session_state.force_web_search and &#x27;docs&#x27; in locals() and docs:
                            with st.expander(&quot;🔍 See document sources&quot;):
                                for i, doc in enumerate(docs, 1):
                                    source_type = doc.metadata.get(&quot;source_type&quot;, &quot;unknown&quot;)
                                    source_icon = &quot;📄&quot; if source_type == &quot;pdf&quot; else &quot;🌐&quot;
                                    source_name = doc.metadata.get(&quot;file_name&quot; if source_type == &quot;pdf&quot; else &quot;url&quot;, &quot;unknown&quot;)
                                    st.write(f&quot;{source_icon} Source {i} from {source_name}:&quot;)
                                    st.write(f&quot;{doc.page_content[:200]}...&quot;)

                except Exception as e:
                    st.error(f&quot;❌ Error generating response: {str(e)}&quot;)

    else:
        # Simple mode without RAG
        with st.spinner(&quot;🤖 Thinking...&quot;):
            try:
                rag_agent = get_rag_agent()
                web_search_agent = get_web_search_agent() if st.session_state.use_web_search else None
                
                # Handle web search if forced or enabled
                context = &quot;&quot;
                if st.session_state.force_web_search and web_search_agent:
                    with st.spinner(&quot;🔍 Searching the web...&quot;):
                        try:
                            web_results = web_search_agent.run(prompt).content
                            if web_results:
                                context = f&quot;Web Search Results:\n{web_results}&quot;
                                st.info(&quot;ℹ️ Using web search as requested.&quot;)
                        except Exception as e:
                            st.error(f&quot;❌ Web search error: {str(e)}&quot;)
                
                # Generate response
                if context:
                    full_prompt = f&quot;&quot;&quot;Context: {context}

Question: {prompt}

Please provide a comprehensive answer based on the available information.&quot;&quot;&quot;
                else:
                    full_prompt = prompt

                response = rag_agent.run(full_prompt)
                response_content = response.content
                
                # Extract thinking process and final response
                import re
                think_pattern = r&#x27;&lt;think&gt;(.*?)&lt;/think&gt;&#x27;
                think_match = re.search(think_pattern, response_content, re.DOTALL)
                
                if think_match:
                    thinking_process = think_match.group(1).strip()
                    final_response = re.sub(think_pattern, &#x27;&#x27;, response_content, flags=re.DOTALL).strip()
                else:
                    thinking_process = None
                    final_response = response_content
                
                # Add assistant response to history (only the final response)
                st.session_state.history.append({
                    &quot;role&quot;: &quot;assistant&quot;,
                    &quot;content&quot;: final_response
                })
                
                # Display assistant response
                with st.chat_message(&quot;assistant&quot;):
                    if thinking_process:
                        with st.expander(&quot;🤔 See thinking process&quot;):
                            st.markdown(thinking_process)
                    st.markdown(final_response)

            except Exception as e:
                st.error(f&quot;❌ Error generating response: {str(e)}&quot;)

else:
    st.warning(&quot;You can directly talk to qwen and gemma models locally! Toggle the RAG mode to upload documents!&quot;)
```