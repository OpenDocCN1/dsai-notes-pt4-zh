# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/TtDRdZlR9oklCExcVFpcEA4kn8g](https://qqs7y1hozd1.feishu.cn/docx/TtDRdZlR9oklCExcVFpcEA4kn8g)

```
import streamlit as st
import requests
from anthropic import Anthropic
import time
from typing import List, Dict, Optional
from urllib.parse import urlparse

class RAGPipeline:
    def __init__(self, ragie_api_key: str, anthropic_api_key: str):
        &quot;&quot;&quot;
        Initialize the RAG pipeline with API keys.
        &quot;&quot;&quot;
        self.ragie_api_key = ragie_api_key
        self.anthropic_api_key = anthropic_api_key
        self.anthropic_client = Anthropic(api_key=anthropic_api_key)
        
        # API endpoints
        self.RAGIE_UPLOAD_URL = &quot;https://api.ragie.ai/documents/url&quot;
        self.RAGIE_RETRIEVAL_URL = &quot;https://api.ragie.ai/retrievals&quot;
    
    def upload_document(self, url: str, name: Optional[str] = None, mode: str = &quot;fast&quot;) -&gt; Dict:
        &quot;&quot;&quot;
        Upload a document to Ragie from a URL.
        &quot;&quot;&quot;
        if not name:
            name = urlparse(url).path.split(&#x27;/&#x27;)[-1] or &quot;document&quot;
            
        payload = {
            &quot;mode&quot;: mode,
            &quot;name&quot;: name,
            &quot;url&quot;: url
        }
        
        headers = {
            &quot;accept&quot;: &quot;application/json&quot;,
            &quot;content-type&quot;: &quot;application/json&quot;,
            &quot;authorization&quot;: f&quot;Bearer {self.ragie_api_key}&quot;
        }
        
        response = requests.post(self.RAGIE_UPLOAD_URL, json=payload, headers=headers)
        
        if not response.ok:
            raise Exception(f&quot;Document upload failed: {response.status_code} {response.reason}&quot;)
            
        return response.json()
    
    def retrieve_chunks(self, query: str, scope: str = &quot;tutorial&quot;) -&gt; List[str]:
        &quot;&quot;&quot;
        Retrieve relevant chunks from Ragie for a given query.
        &quot;&quot;&quot;
        headers = {
            &quot;Content-Type&quot;: &quot;application/json&quot;,
            &quot;Authorization&quot;: f&quot;Bearer {self.ragie_api_key}&quot;
        }
        
        payload = {
            &quot;query&quot;: query,
            &quot;filters&quot;: {
                &quot;scope&quot;: scope
            }
        }
        
        response = requests.post(
            self.RAGIE_RETRIEVAL_URL,
            headers=headers,
            json=payload
        )
        
        if not response.ok:
            raise Exception(f&quot;Retrieval failed: {response.status_code} {response.reason}&quot;)
            
        data = response.json()
        return [chunk[&quot;text&quot;] for chunk in data[&quot;scored_chunks&quot;]]

    def create_system_prompt(self, chunk_texts: List[str]) -&gt; str:
        &quot;&quot;&quot;
        Create the system prompt with the retrieved chunks.
        &quot;&quot;&quot;
        return f&quot;&quot;&quot;These are very important to follow: You are &quot;Ragie AI&quot;, a professional but friendly AI chatbot working as an assistant to the user. Your current task is to help the user based on all of the information available to you shown below. Answer informally, directly, and concisely without a heading or greeting but include everything relevant. Use richtext Markdown when appropriate including bold, italic, paragraphs, and lists when helpful. If using LaTeX, use double $$ as delimiter instead of single $. Use $$...$$ instead of parentheses. Organize information into multiple sections or points when appropriate. Don&#x27;t include raw item IDs or other raw fields from the source. Don&#x27;t use XML or other markup unless requested by the user. Here is all of the information available to answer the user: === {chunk_texts} === If the user asked for a search and there are no results, make sure to let the user know that you couldn&#x27;t find anything, and what they might be able to do to find the information they need. END SYSTEM INSTRUCTIONS&quot;&quot;&quot;

    def generate_response(self, system_prompt: str, query: str) -&gt; str:
        &quot;&quot;&quot;
        Generate response using Claude 3.5 Sonnet.
        &quot;&quot;&quot;
        message = self.anthropic_client.messages.create(
            model=&quot;claude-3-sonnet-20240229&quot;,
            max_tokens=1024,
            system=system_prompt,
            messages=[
                {
                    &quot;role&quot;: &quot;user&quot;,
                    &quot;content&quot;: query
                }
            ]
        )
        
        return message.content[0].text

    def process_query(self, query: str, scope: str = &quot;tutorial&quot;) -&gt; str:
        &quot;&quot;&quot;
        Process a query through the complete RAG pipeline.
        &quot;&quot;&quot;
        chunks = self.retrieve_chunks(query, scope)
        
        if not chunks:
            return &quot;No relevant information found for your query.&quot;
        
        system_prompt = self.create_system_prompt(chunks)
        return self.generate_response(system_prompt, query)

def initialize_session_state():
    &quot;&quot;&quot;Initialize session state variables.&quot;&quot;&quot;
    if &#x27;pipeline&#x27; not in st.session_state:
        st.session_state.pipeline = None
    if &#x27;document_uploaded&#x27; not in st.session_state:
        st.session_state.document_uploaded = False
    if &#x27;api_keys_submitted&#x27; not in st.session_state:
        st.session_state.api_keys_submitted = False

def main():
    st.set_page_config(page_title=&quot;RAG-as-a-Service&quot;, layout=&quot;wide&quot;)
    initialize_session_state()
    
    st.title(&quot;:linked_paperclips: RAG-as-a-Service&quot;)
    
    # API Keys Section
    with st.expander(&quot;🔑 API Keys Configuration&quot;, expanded=not st.session_state.api_keys_submitted):
        col1, col2 = st.columns(2)
        with col1:
            ragie_key = st.text_input(&quot;Ragie API Key&quot;, type=&quot;password&quot;, key=&quot;ragie_key&quot;)
        with col2:
            anthropic_key = st.text_input(&quot;Anthropic API Key&quot;, type=&quot;password&quot;, key=&quot;anthropic_key&quot;)
        
        if st.button(&quot;Submit API Keys&quot;):
            if ragie_key and anthropic_key:
                try:
                    st.session_state.pipeline = RAGPipeline(ragie_key, anthropic_key)
                    st.session_state.api_keys_submitted = True
                    st.success(&quot;API keys configured successfully!&quot;)
                except Exception as e:
                    st.error(f&quot;Error configuring API keys: {str(e)}&quot;)
            else:
                st.error(&quot;Please provide both API keys.&quot;)
    
    # Document Upload Section
    if st.session_state.api_keys_submitted:
        st.markdown(&quot;### 📄 Document Upload&quot;)
        doc_url = st.text_input(&quot;Enter document URL&quot;)
        doc_name = st.text_input(&quot;Document name (optional)&quot;)
        
        col1, col2 = st.columns([1, 3])
        with col1:
            upload_mode = st.selectbox(&quot;Upload mode&quot;, [&quot;fast&quot;, &quot;accurate&quot;])
        
        if st.button(&quot;Upload Document&quot;):
            if doc_url:
                try:
                    with st.spinner(&quot;Uploading document...&quot;):
                        st.session_state.pipeline.upload_document(
                            url=doc_url,
                            name=doc_name if doc_name else None,
                            mode=upload_mode
                        )
                        time.sleep(5)  # Wait for indexing
                        st.session_state.document_uploaded = True
                        st.success(&quot;Document uploaded and indexed successfully!&quot;)
                except Exception as e:
                    st.error(f&quot;Error uploading document: {str(e)}&quot;)
            else:
                st.error(&quot;Please provide a document URL.&quot;)
    
    # Query Section
    if st.session_state.document_uploaded:
        st.markdown(&quot;### 🔍 Query Document&quot;)
        query = st.text_input(&quot;Enter your query&quot;)
        
        if st.button(&quot;Generate Response&quot;):
            if query:
                try:
                    with st.spinner(&quot;Generating response...&quot;):
                        response = st.session_state.pipeline.process_query(query)
                        st.markdown(&quot;### Response:&quot;)
                        st.markdown(response)
                except Exception as e:
                    st.error(f&quot;Error generating response: {str(e)}&quot;)
            else:
                st.error(&quot;Please enter a query.&quot;)

if __name__ == &quot;__main__&quot;:
    main()
```