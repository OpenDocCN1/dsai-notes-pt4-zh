# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/BbjAdI1nmozspbx1i7lc8Qohnlc](https://qqs7y1hozd1.feishu.cn/docx/BbjAdI1nmozspbx1i7lc8Qohnlc)

```
import os
import logging
import streamlit as st
from raglite import RAGLiteConfig, insert_document, hybrid_search, retrieve_chunks, rerank_chunks, rag
from rerankers import Reranker
from typing import List
from pathlib import Path
import anthropic
import time
import warnings

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)
warnings.filterwarnings(&quot;ignore&quot;, message=&quot;.*torch.classes.*&quot;)

RAG_SYSTEM_PROMPT = &quot;&quot;&quot;
You are a friendly and knowledgeable assistant that provides complete and insightful answers.
Answer the user&#x27;s question using only the context below.
When responding, you MUST NOT reference the existence of the context, directly or indirectly.
Instead, you MUST treat the context as if its contents are entirely part of your working memory.
&quot;&quot;&quot;.strip()

def initialize_config(openai_key: str, anthropic_key: str, cohere_key: str, db_url: str) -&gt; RAGLiteConfig:
    &quot;&quot;&quot;Initializes and returns a RAGLiteConfig object with the specified API keys and database URL.

    This function sets the provided API keys in the environment variables and returns a 
    RAGLiteConfig object configured with the given database URL and pre-defined settings for 
    language model, embedder, and reranker.

    Args:
        openai_key (str): The API key for OpenAI services.
        anthropic_key (str): The API key for Anthropic services.
        cohere_key (str): The API key for Cohere services.
        db_url (str): The database URL for connecting to the desired data source.

    Returns:
        RAGLiteConfig: A configuration object initialized with the specified parameters.

    Raises:
        ValueError: If there is an issue setting up the configuration, an error is raised with details.&quot;&quot;&quot;
    try:
        os.environ[&quot;OPENAI_API_KEY&quot;] = openai_key
        os.environ[&quot;ANTHROPIC_API_KEY&quot;] = anthropic_key
        os.environ[&quot;COHERE_API_KEY&quot;] = cohere_key
        
        return RAGLiteConfig(
            db_url=db_url,
            llm=&quot;claude-3-opus-20240229&quot;,
            embedder=&quot;text-embedding-3-large&quot;,
            embedder_normalize=True,
            chunk_max_size=2000,
            embedder_sentence_window_size=2,
            reranker=Reranker(&quot;cohere&quot;, api_key=cohere_key, lang=&quot;en&quot;)
        )
    except Exception as e:
        raise ValueError(f&quot;Configuration error: {e}&quot;)

def process_document(file_path: str) -&gt; bool:
    &quot;&quot;&quot;Processes a document by inserting it into a system with a given configuration.

    This function checks if a configuration is initialized in the session state.
    If the configuration is present, it attempts to insert the document located
    at the given file path using this configuration.

    Args:
        file_path (str): The path to the document to be processed.

    Returns:
        bool: True if the document was successfully processed; False otherwise.&quot;&quot;&quot;
    try:
        if not st.session_state.get(&#x27;my_config&#x27;):
            raise ValueError(&quot;Configuration not initialized&quot;)
        insert_document(Path(file_path), config=st.session_state.my_config)
        return True
    except Exception as e:
        logger.error(f&quot;Error processing document: {str(e)}&quot;)
        return False

def perform_search(query: str) -&gt; List[dict]:
    &quot;&quot;&quot;Conducts a hybrid search and returns a list of ranked chunks based on the query.

    This function performs a search using a hybrid search method, retrieves the relevant 
    chunks, and reranks them according to the query. It handles any exceptions that occur 
    during the process and logs the errors.

    Args:
        query (str): The search query string.

    Returns:
        List[dict]: A list of dictionaries representing the ranked chunks. Returns an 
        empty list if no results are found or if an error occurs.&quot;&quot;&quot;
    try:
        chunk_ids, scores = hybrid_search(query, num_results=10, config=st.session_state.my_config)
        if not chunk_ids:
            return []
        chunks = retrieve_chunks(chunk_ids, config=st.session_state.my_config)
        return rerank_chunks(query, chunks, config=st.session_state.my_config)
    except Exception as e:
        logger.error(f&quot;Search error: {str(e)}&quot;)
        return []

def handle_fallback(query: str) -&gt; str:
    try:
        client = anthropic.Anthropic(api_key=st.session_state.user_env[&quot;ANTHROPIC_API_KEY&quot;])
        system_prompt = &quot;&quot;&quot;You are a helpful AI assistant. When you don&#x27;t know something, 
        be honest about it. Provide clear, concise, and accurate responses. If the question 
        is not related to any specific document, use your general knowledge to answer.&quot;&quot;&quot;
        
        message = client.messages.create(
            model=&quot;claude-3-sonnet-20240229&quot;,
            max_tokens=1024,
            system=system_prompt,
            messages=[{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: query}],
            temperature=0.7
        )
        return message.content[0].text
    except Exception as e:
        logger.error(f&quot;Fallback error: {str(e)}&quot;)
        st.error(f&quot;Fallback error: {str(e)}&quot;)  # Show error in UI
        return &quot;I apologize, but I encountered an error while processing your request. Please try again.&quot;

def main():
    st.set_page_config(page_title=&quot;LLM-Powered Hybrid Search-RAG Assistant&quot;, layout=&quot;wide&quot;)
    
    for state_var in [&#x27;chat_history&#x27;, &#x27;documents_loaded&#x27;, &#x27;my_config&#x27;, &#x27;user_env&#x27;]:
        if state_var not in st.session_state:
            st.session_state[state_var] = [] if state_var == &#x27;chat_history&#x27; else False if state_var == &#x27;documents_loaded&#x27; else None if state_var == &#x27;my_config&#x27; else {}

    with st.sidebar:
        st.title(&quot;Configuration&quot;)
        openai_key = st.text_input(&quot;OpenAI API Key&quot;, value=st.session_state.get(&#x27;openai_key&#x27;, &#x27;&#x27;), type=&quot;password&quot;, placeholder=&quot;sk-...&quot;)
        anthropic_key = st.text_input(&quot;Anthropic API Key&quot;, value=st.session_state.get(&#x27;anthropic_key&#x27;, &#x27;&#x27;), type=&quot;password&quot;, placeholder=&quot;sk-ant-...&quot;)
        cohere_key = st.text_input(&quot;Cohere API Key&quot;, value=st.session_state.get(&#x27;cohere_key&#x27;, &#x27;&#x27;), type=&quot;password&quot;, placeholder=&quot;Enter Cohere key&quot;)
        db_url = st.text_input(&quot;Database URL&quot;, value=st.session_state.get(&#x27;db_url&#x27;, &#x27;sqlite:///raglite.sqlite&#x27;), placeholder=&quot;sqlite:///raglite.sqlite&quot;)
        
        if st.button(&quot;Save Configuration&quot;):
            try:
                if not all([openai_key, anthropic_key, cohere_key, db_url]):
                    st.error(&quot;All fields are required!&quot;)
                    return
                
                for key, value in {&#x27;openai_key&#x27;: openai_key, &#x27;anthropic_key&#x27;: anthropic_key, &#x27;cohere_key&#x27;: cohere_key, &#x27;db_url&#x27;: db_url}.items():
                    st.session_state[key] = value
                
                st.session_state.my_config = initialize_config(openai_key=openai_key, anthropic_key=anthropic_key, cohere_key=cohere_key, db_url=db_url)
                st.session_state.user_env = {&quot;ANTHROPIC_API_KEY&quot;: anthropic_key}
                st.success(&quot;Configuration saved successfully!&quot;)
            except Exception as e:
                st.error(f&quot;Configuration error: {str(e)}&quot;)

    st.title(&quot;👀 RAG App with Hybrid Search&quot;)

    if st.session_state.my_config:
        uploaded_files = st.file_uploader(&quot;Upload PDF documents&quot;, type=[&quot;pdf&quot;], accept_multiple_files=True, key=&quot;pdf_uploader&quot;)

        if uploaded_files:
            success = False
            for uploaded_file in uploaded_files:
                with st.spinner(f&quot;Processing {uploaded_file.name}...&quot;):
                    temp_path = f&quot;temp_{uploaded_file.name}&quot;
                    with open(temp_path, &quot;wb&quot;) as f:
                        f.write(uploaded_file.getvalue())
                    
                    if process_document(temp_path):
                        st.success(f&quot;Successfully processed: {uploaded_file.name}&quot;)
                        success = True
                    else:
                        st.error(f&quot;Failed to process: {uploaded_file.name}&quot;)
                    os.remove(temp_path)
            
            if success:
                st.session_state.documents_loaded = True
                st.success(&quot;Documents are ready! You can now ask questions about them.&quot;)

    if st.session_state.documents_loaded:
        for msg in st.session_state.chat_history:
            with st.chat_message(&quot;user&quot;): st.write(msg[0])
            with st.chat_message(&quot;assistant&quot;): st.write(msg[1])

        user_input = st.chat_input(&quot;Ask a question about the documents...&quot;)
        if user_input:
            with st.chat_message(&quot;user&quot;): st.write(user_input)
            with st.chat_message(&quot;assistant&quot;):
                message_placeholder = st.empty()
                try:
                    reranked_chunks = perform_search(query=user_input)
                    if not reranked_chunks or len(reranked_chunks) == 0:
                        logger.info(&quot;No relevant documents found. Falling back to Claude.&quot;)
                        st.info(&quot;No relevant documents found. Using general knowledge to answer.&quot;)
                        full_response = handle_fallback(user_input)
                    else:
                        formatted_messages = [{&quot;role&quot;: &quot;user&quot; if i % 2 == 0 else &quot;assistant&quot;, &quot;content&quot;: msg}
                                           for i, msg in enumerate([m for pair in st.session_state.chat_history for m in pair]) if msg]
                        
                        response_stream = rag(prompt=user_input, 
                                           system_prompt=RAG_SYSTEM_PROMPT,
                                           search=hybrid_search, 
                                           messages=formatted_messages,
                                           max_contexts=5, 
                                           config=st.session_state.my_config)
                        
                        full_response = &quot;&quot;
                        for chunk in response_stream:
                            full_response += chunk
                            message_placeholder.markdown(full_response + &quot;▌&quot;)
                    
                    message_placeholder.markdown(full_response)
                    st.session_state.chat_history.append((user_input, full_response))
                except Exception as e:
                    st.error(f&quot;Error: {str(e)}&quot;)
    else:
        st.info(&quot;Please configure your API keys and upload documents to get started.&quot; if not st.session_state.my_config else &quot;Please upload some documents to get started.&quot;)

if __name__ == &quot;__main__&quot;:
    main()
```