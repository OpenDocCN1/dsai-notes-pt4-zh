# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Yz3gdlxBXobBS3xDaWHceTlHnNg](https://qqs7y1hozd1.feishu.cn/docx/Yz3gdlxBXobBS3xDaWHceTlHnNg)

```
import os
import logging
import streamlit as st
from raglite import RAGLiteConfig, insert_document, hybrid_search, retrieve_chunks, rerank_chunks, rag
from rerankers import Reranker
from typing import List, Dict, Any
from pathlib import Path
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

def initialize_config(settings: Dict[str, Any]) -&gt; RAGLiteConfig:
    &quot;&quot;&quot;Initializes and returns a RAGLiteConfig object based on provided settings.

    This function constructs a RAGLiteConfig object using the database URL, 
    language model path, and embedder path specified in the `settings` dictionary.
    The configuration includes default options for embedder normalization and 
    chunk size. A reranker is also initialized with a predefined model.

    Args:
        settings (Dict[str, Any]): A dictionary containing configuration 
        parameters. Expected keys are &#x27;DBUrl&#x27;, &#x27;LLMPath&#x27;, and &#x27;EmbedderPath&#x27;.

    Returns:
        RAGLiteConfig: An initialized configuration object for RAGLite.

    Raises:
        ValueError: If there is an error in the configuration process, such as 
        missing keys or invalid values in the settings dictionary.&quot;&quot;&quot;
    try:
        return RAGLiteConfig(
            db_url=settings[&quot;DBUrl&quot;],
            llm=f&quot;llama-cpp-python/{settings[&#x27;LLMPath&#x27;]}&quot;,
            embedder=f&quot;llama-cpp-python/{settings[&#x27;EmbedderPath&#x27;]}&quot;,
            embedder_normalize=True,
            chunk_max_size=512,
            reranker=Reranker(&quot;ms-marco-MiniLM-L-12-v2&quot;, model_type=&quot;flashrank&quot;)
        )
    except Exception as e:
        raise ValueError(f&quot;Configuration error: {e}&quot;)

def process_document(file_path: str) -&gt; bool:
    &quot;&quot;&quot;Processes a document by inserting it into a system with a given configuration.

    This function attempts to insert a document specified by the file path into
    a system using a predefined configuration stored in the session state. It
    logs an error if the operation fails.

    Args:
        file_path (str): The path to the document file that needs to be processed.

    Returns:
        bool: True if the document is successfully processed; False if an error occurs.&quot;&quot;&quot;
    try:
        if not st.session_state.get(&#x27;my_config&#x27;):
            raise ValueError(&quot;Configuration not initialized&quot;)
        insert_document(Path(file_path), config=st.session_state.my_config)
        return True
    except Exception as e:
        logger.error(f&quot;Error processing document: {str(e)}&quot;)
        return False

def perform_search(query: str) -&gt; List[dict]:
    &quot;&quot;&quot;Conducts a hybrid search and returns reranked results.

    This function performs a hybrid search using the provided query and
    attempts to retrieve and rerank relevant chunks. It returns a list of
    reranked search results.

    Args:
        query (str): The search query string.

    Returns:
        List[dict]: A list of dictionaries containing reranked search results.
        Returns an empty list if no results are found or if an error occurs.&quot;&quot;&quot;
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
        system_prompt = &quot;&quot;&quot;You are a helpful AI assistant. When you don&#x27;t know something, 
        be honest about it. Provide clear, concise, and accurate responses.&quot;&quot;&quot;
        
        response_stream = rag(
            prompt=query,
            system_prompt=system_prompt,
            search=None,
            messages=[],
            max_tokens=1024,
            temperature=0.7,
            config=st.session_state.my_config
        )
        
        full_response = &quot;&quot;
        for chunk in response_stream:
            full_response += chunk
        
        if not full_response.strip():
            return &quot;I apologize, but I couldn&#x27;t generate a response. Please try rephrasing your question.&quot;
            
        return full_response
        
    except Exception as e:
        logger.error(f&quot;Fallback error: {str(e)}&quot;)
        return &quot;I apologize, but I encountered an error while processing your request. Please try again.&quot;

def main():
    st.set_page_config(page_title=&quot;Local LLM-Powered Hybrid Search-RAG Assistant&quot;, layout=&quot;wide&quot;)
    
    for state_var in [&#x27;chat_history&#x27;, &#x27;documents_loaded&#x27;, &#x27;my_config&#x27;]:
        if state_var not in st.session_state:
            st.session_state[state_var] = [] if state_var == &#x27;chat_history&#x27; else False if state_var == &#x27;documents_loaded&#x27; else None

    with st.sidebar:
        st.title(&quot;Configuration&quot;)
        
        llm_path = st.text_input(
            &quot;LLM Model Path&quot;, 
            value=st.session_state.get(&#x27;llm_path&#x27;, &#x27;&#x27;),
            placeholder=&quot;TheBloke/Llama-2-7B-Chat-GGUF/llama-2-7b-chat.Q4_K_M.gguf@4096&quot;,
            help=&quot;Path to your local LLM model in GGUF format&quot;
        )
        
        embedder_path = st.text_input(
            &quot;Embedder Model Path&quot;,
            value=st.session_state.get(&#x27;embedder_path&#x27;, &#x27;&#x27;),
            placeholder=&quot;lm-kit/bge-m3-gguf/bge-m3-Q4_K_M.gguf@1024&quot;,
            help=&quot;Path to your local embedding model in GGUF format&quot;
        )
        
        db_url = st.text_input(
            &quot;Database URL&quot;,
            value=st.session_state.get(&#x27;db_url&#x27;, &#x27;&#x27;),
            placeholder=&quot;postgresql://user:pass@host:port/db&quot;,
            help=&quot;Database connection URL&quot;
        )
        
        if st.button(&quot;Save Configuration&quot;):
            try:
                if not all([llm_path, embedder_path, db_url]):
                    st.error(&quot;All fields are required!&quot;)
                    return
                
                settings = {
                    &quot;LLMPath&quot;: llm_path,
                    &quot;EmbedderPath&quot;: embedder_path,
                    &quot;DBUrl&quot;: db_url
                }
                
                st.session_state.my_config = initialize_config(settings)
                st.success(&quot;Configuration saved successfully!&quot;)
                
            except Exception as e:
                st.error(f&quot;Configuration error: {str(e)}&quot;)

    st.title(&quot;🖥️ Local RAG App with Hybrid Search&quot;)

    if st.session_state.my_config:
        uploaded_files = st.file_uploader(
            &quot;Upload PDF documents&quot;,
            type=[&quot;pdf&quot;],
            accept_multiple_files=True,
            key=&quot;pdf_uploader&quot;
        )

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
                        logger.info(&quot;No relevant documents found. Falling back to local LLM.&quot;)
                        with st.spinner(&quot;Using general knowledge to answer...&quot;):
                            full_response = handle_fallback(user_input)
                            if full_response.startswith(&quot;I apologize&quot;):
                                st.warning(&quot;No relevant documents found and fallback failed.&quot;)
                            else:
                                st.info(&quot;Answering from general knowledge.&quot;)
                    else:
                        formatted_messages = [
                            {&quot;role&quot;: &quot;user&quot; if i % 2 == 0 else &quot;assistant&quot;, &quot;content&quot;: msg}
                            for i, msg in enumerate([m for pair in st.session_state.chat_history for m in pair])
                            if msg
                        ]
                        
                        response_stream = rag(
                            prompt=user_input,
                            system_prompt=RAG_SYSTEM_PROMPT,
                            search=hybrid_search,
                            messages=formatted_messages,
                            max_contexts=5,
                            config=st.session_state.my_config
                        )
                        
                        full_response = &quot;&quot;
                        for chunk in response_stream:
                            full_response += chunk
                            message_placeholder.markdown(full_response + &quot;▌&quot;)
                    
                    message_placeholder.markdown(full_response)
                    st.session_state.chat_history.append((user_input, full_response))
                    
                except Exception as e:
                    logger.error(f&quot;Error: {str(e)}&quot;)
                    st.error(f&quot;Error: {str(e)}&quot;)
    else:
        st.info(
            &quot;Please configure your model paths and upload documents to get started.&quot;
            if not st.session_state.my_config
            else &quot;Please upload some documents to get started.&quot;
        )

if __name__ == &quot;__main__&quot;:
    main()
```