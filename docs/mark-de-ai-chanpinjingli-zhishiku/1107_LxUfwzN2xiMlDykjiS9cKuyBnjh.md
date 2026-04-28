# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/M1qldw9hJoLeBxxv2MNcOYbbnYe](https://qqs7y1hozd1.feishu.cn/docx/M1qldw9hJoLeBxxv2MNcOYbbnYe)

```
import os
import tempfile
from datetime import datetime
from typing import List

import streamlit as st
import google.generativeai as genai
import bs4
from agno.agent import Agent
from agno.models.google import Gemini
from langchain_community.document_loaders import PyPDFLoader, WebBaseLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_qdrant import QdrantVectorStore
from qdrant_client import QdrantClient
from qdrant_client.models import Distance, VectorParams
from langchain_core.embeddings import Embeddings
from agno.tools.exa import ExaTools


class GeminiEmbedder(Embeddings):
    def __init__(self, model_name=&quot;models/text-embedding-004&quot;):
        genai.configure(api_key=st.session_state.google_api_key)
        self.model = model_name

    def embed_documents(self, texts: List[str]) -&gt; List[List[float]]:
        return [self.embed_query(text) for text in texts]

    def embed_query(self, text: str) -&gt; List[float]:
        response = genai.embed_content(
            model=self.model,
            content=text,
            task_type=&quot;retrieval_document&quot;
        )
        return response[&#x27;embedding&#x27;]


# Constants
COLLECTION_NAME = &quot;gemini-thinking-agent-agno&quot;


# Streamlit App Initialization
st.title(&quot;🤔 Agentic RAG with Gemini Thinking and Agno&quot;)

# Session State Initialization
if &#x27;google_api_key&#x27; not in st.session_state:
    st.session_state.google_api_key = &quot;&quot;
if &#x27;qdrant_api_key&#x27; not in st.session_state:
    st.session_state.qdrant_api_key = &quot;&quot;
if &#x27;qdrant_url&#x27; not in st.session_state:
    st.session_state.qdrant_url = &quot;&quot;
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


# Sidebar Configuration
st.sidebar.header(&quot;🔑 API Configuration&quot;)
google_api_key = st.sidebar.text_input(&quot;Google API Key&quot;, type=&quot;password&quot;, value=st.session_state.google_api_key)
qdrant_api_key = st.sidebar.text_input(&quot;Qdrant API Key&quot;, type=&quot;password&quot;, value=st.session_state.qdrant_api_key)
qdrant_url = st.sidebar.text_input(&quot;Qdrant URL&quot;, 
                                 placeholder=&quot;https://your-cluster.cloud.qdrant.io:6333&quot;,
                                 value=st.session_state.qdrant_url)

# Clear Chat Button
if st.sidebar.button(&quot;🗑️ Clear Chat History&quot;):
    st.session_state.history = []
    st.rerun()

# Update session state
st.session_state.google_api_key = google_api_key
st.session_state.qdrant_api_key = qdrant_api_key
st.session_state.qdrant_url = qdrant_url

# Add in the sidebar configuration section, after the existing API inputs
st.sidebar.header(&quot;🌐 Web Search Configuration&quot;)
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

# Add this to the sidebar configuration section
st.sidebar.header(&quot;🎯 Search Configuration&quot;)
st.session_state.similarity_threshold = st.sidebar.slider(
    &quot;Document Similarity Threshold&quot;,
    min_value=0.0,
    max_value=1.0,
    value=0.7,
    help=&quot;Lower values will return more documents but might be less relevant. Higher values are more strict.&quot;
)


# Utility Functions
def init_qdrant():
    &quot;&quot;&quot;Initialize Qdrant client with configured settings.&quot;&quot;&quot;
    if not all([st.session_state.qdrant_api_key, st.session_state.qdrant_url]):
        return None
    try:
        return QdrantClient(
            url=st.session_state.qdrant_url,
            api_key=st.session_state.qdrant_api_key,
            timeout=60
        )
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
                    size=768,  # Gemini embedding-004 dimension
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
            embedding=GeminiEmbedder()
        )
        
        # Add documents
        with st.spinner(&#x27;📤 Uploading documents to Qdrant...&#x27;):
            vector_store.add_documents(texts)
            st.success(&quot;✅ Documents stored successfully!&quot;)
            return vector_store
            
    except Exception as e:
        st.error(f&quot;🔴 Vector store error: {str(e)}&quot;)
        return None


# Add this after the GeminiEmbedder class
def get_query_rewriter_agent() -&gt; Agent:
    &quot;&quot;&quot;Initialize a query rewriting agent.&quot;&quot;&quot;
    return Agent(
        name=&quot;Query Rewriter&quot;,
        model=Gemini(id=&quot;gemini-exp-1206&quot;),
        instructions=&quot;&quot;&quot;You are an expert at reformulating questions to be more precise and detailed. 
        Your task is to:
        1. Analyze the user&#x27;s question
        2. Rewrite it to be more specific and search-friendly
        3. Expand any acronyms or technical terms
        4. Return ONLY the rewritten query without any additional text or explanations
        
        Example 1:
        User: &quot;What does it say about ML?&quot;
        Output: &quot;What are the key concepts, techniques, and applications of Machine Learning (ML) discussed in the context?&quot;
        
        Example 2:
        User: &quot;Tell me about transformers&quot;
        Output: &quot;Explain the architecture, mechanisms, and applications of Transformer neural networks in natural language processing and deep learning&quot;
        &quot;&quot;&quot;,
        show_tool_calls=False,
        markdown=True,
    )


def get_web_search_agent() -&gt; Agent:
    &quot;&quot;&quot;Initialize a web search agent.&quot;&quot;&quot;
    return Agent(
        name=&quot;Web Search Agent&quot;,
        model=Gemini(id=&quot;gemini-exp-1206&quot;),
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
        name=&quot;Gemini RAG Agent&quot;,
        model=Gemini(id=&quot;gemini-2.0-flash-thinking-exp-01-21&quot;),
        instructions=&quot;&quot;&quot;You are an Intelligent Agent specializing in providing accurate answers.
        
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
    &quot;&quot;&quot;
    Check if documents in vector store are relevant to the query.
    
    Args:
        query: The search query
        vector_store: The vector store to search in
        threshold: Similarity threshold
        
    Returns:
        tuple[bool, List]: (has_relevant_docs, relevant_docs)
    &quot;&quot;&quot;
    if not vector_store:
        return False, []
        
    retriever = vector_store.as_retriever(
        search_type=&quot;similarity_score_threshold&quot;,
        search_kwargs={&quot;k&quot;: 5, &quot;score_threshold&quot;: threshold}
    )
    docs = retriever.invoke(query)
    return bool(docs), docs


# Main Application Flow
if st.session_state.google_api_key:
    os.environ[&quot;GOOGLE_API_KEY&quot;] = st.session_state.google_api_key
    genai.configure(api_key=st.session_state.google_api_key)
    
    qdrant_client = init_qdrant()
    
    # File/URL Upload Section
    st.sidebar.header(&quot;📁 Data Upload&quot;)
    uploaded_file = st.sidebar.file_uploader(&quot;Upload PDF&quot;, type=[&quot;pdf&quot;])
    web_url = st.sidebar.text_input(&quot;Or enter URL&quot;)
    
    # Process documents
    if uploaded_file:
        file_name = uploaded_file.name
        if file_name not in st.session_state.processed_documents:
            with st.spinner(&#x27;Processing PDF...&#x27;):
                texts = process_pdf(uploaded_file)
                if texts and qdrant_client:
                    if st.session_state.vector_store:
                        st.session_state.vector_store.add_documents(texts)
                    else:
                        st.session_state.vector_store = create_vector_store(qdrant_client, texts)
                    st.session_state.processed_documents.append(file_name)
                    st.success(f&quot;✅ Added PDF: {file_name}&quot;)

    if web_url:
        if web_url not in st.session_state.processed_documents:
            with st.spinner(&#x27;Processing URL...&#x27;):
                texts = process_web(web_url)
                if texts and qdrant_client:
                    if st.session_state.vector_store:
                        st.session_state.vector_store.add_documents(texts)
                    else:
                        st.session_state.vector_store = create_vector_store(qdrant_client, texts)
                    st.session_state.processed_documents.append(web_url)
                    st.success(f&quot;✅ Added URL: {web_url}&quot;)

    # Display sources in sidebar
    if st.session_state.processed_documents:
        st.sidebar.header(&quot;📚 Processed Sources&quot;)
        for source in st.session_state.processed_documents:
            if source.endswith(&#x27;.pdf&#x27;):
                st.sidebar.text(f&quot;📄 {source}&quot;)
            else:
                st.sidebar.text(f&quot;🌐 {source}&quot;)

    # Chat Interface
    # Create two columns for chat input and search toggle
    chat_col, toggle_col = st.columns([0.9, 0.1])

    with chat_col:
        prompt = st.chat_input(&quot;Ask about your documents...&quot;)

    with toggle_col:
        st.session_state.force_web_search = st.toggle(&#x27;🌐&#x27;, help=&quot;Force web search&quot;)

    if prompt:
        # Add user message to history
        st.session_state.history.append({&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: prompt})
        with st.chat_message(&quot;user&quot;):
            st.write(prompt)

        # Step 1: Rewrite the query for better retrieval
        with st.spinner(&quot;🤔 Reformulating query...&quot;):
            try:
                query_rewriter = get_query_rewriter_agent()
                rewritten_query = query_rewriter.run(prompt).content
                
                with st.expander(&quot;🔄 See rewritten query&quot;):
                    st.write(f&quot;Original: {prompt}&quot;)
                    st.write(f&quot;Rewritten: {rewritten_query}&quot;)
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
Rewritten Question: {rewritten_query}

Please provide a comprehensive answer based on the available information.&quot;&quot;&quot;
                else:
                    full_prompt = f&quot;Original Question: {prompt}\nRewritten Question: {rewritten_query}&quot;
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
    st.warning(&quot;⚠️ Please enter your Google API Key to continue&quot;)
```