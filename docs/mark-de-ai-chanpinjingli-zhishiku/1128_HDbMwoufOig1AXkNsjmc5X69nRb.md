# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/LzJkd4DWPoaaIOx22xxc1VXpnxh](https://qqs7y1hozd1.feishu.cn/docx/LzJkd4DWPoaaIOx22xxc1VXpnxh)

```
import os
import streamlit as st
from langchain_community.document_loaders import PyPDFLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_cohere import CohereEmbeddings, ChatCohere
from langchain_qdrant import QdrantVectorStore
from qdrant_client import QdrantClient
from qdrant_client.models import Distance, VectorParams
from langchain.chains.combine_documents import create_stuff_documents_chain
from langchain.chains import create_retrieval_chain
from langchain import hub
import tempfile
from langgraph.prebuilt import create_react_agent
from langchain_community.tools import DuckDuckGoSearchRun
from typing import TypedDict, List
from langchain_core.language_models import BaseLanguageModel
from langchain_core.messages import AIMessage, HumanMessage, SystemMessage
from time import sleep
from tenacity import retry, wait_exponential, stop_after_attempt


def init_session_state():
    if &#x27;api_keys_submitted&#x27; not in st.session_state:
        st.session_state.api_keys_submitted = False
    if &#x27;chat_history&#x27; not in st.session_state:
        st.session_state.chat_history = []
    if &#x27;vectorstore&#x27; not in st.session_state:
        st.session_state.vectorstore = None
    if &#x27;qdrant_api_key&#x27; not in st.session_state:
        st.session_state.qdrant_api_key = &quot;&quot;
    if &#x27;qdrant_url&#x27; not in st.session_state:
        st.session_state.qdrant_url = &quot;&quot;

def sidebar_api_form():
    with st.sidebar:
        st.header(&quot;API Credentials&quot;)
        
        if st.session_state.api_keys_submitted:
            st.success(&quot;API credentials verified&quot;)
            if st.button(&quot;Reset Credentials&quot;):
                st.session_state.clear()
                st.rerun()
            return True
        
        with st.form(&quot;api_credentials&quot;):
            cohere_key = st.text_input(&quot;Cohere API Key&quot;, type=&quot;password&quot;)
            qdrant_key = st.text_input(&quot;Qdrant API Key&quot;, type=&quot;password&quot;, help=&quot;Enter your Qdrant API key&quot;)
            qdrant_url = st.text_input(&quot;Qdrant URL&quot;, 
                                     placeholder=&quot;https://xyz-example.eu-central.aws.cloud.qdrant.io:6333&quot;,
                                     help=&quot;Enter your Qdrant instance URL&quot;)
            
            if st.form_submit_button(&quot;Submit Credentials&quot;):
                try:
                    client = QdrantClient(url=qdrant_url, api_key=qdrant_key, timeout=60)
                    client.get_collections()
                    
                    st.session_state.cohere_api_key = cohere_key
                    st.session_state.qdrant_api_key = qdrant_key
                    st.session_state.qdrant_url = qdrant_url
                    st.session_state.api_keys_submitted = True
                    
                    st.success(&quot;Credentials verified!&quot;)
                    st.rerun()
                except Exception as e:
                    st.error(f&quot;Qdrant connection failed: {str(e)}&quot;)
        return False

def init_qdrant() -&gt; QdrantClient:
    if not st.session_state.get(&quot;qdrant_api_key&quot;):
        raise ValueError(&quot;Qdrant API key not provided&quot;)
    if not st.session_state.get(&quot;qdrant_url&quot;):
        raise ValueError(&quot;Qdrant URL not provided&quot;)
    
    return QdrantClient(url=st.session_state.qdrant_url,
                       api_key=st.session_state.qdrant_api_key,
                       timeout=60)

init_session_state()

if not sidebar_api_form():
    st.info(&quot;Please enter your API credentials in the sidebar to continue.&quot;)
    st.stop()

embedding = CohereEmbeddings(model=&quot;embed-english-v3.0&quot;,
                            cohere_api_key=st.session_state.cohere_api_key)

chat_model = ChatCohere(model=&quot;command-r7b-12-2024&quot;,
                       temperature=0.1,
                       max_tokens=512,
                       verbose=True,
                       cohere_api_key=st.session_state.cohere_api_key)

client = init_qdrant()

def process_document(file):
    try:
        with tempfile.NamedTemporaryFile(delete=False, suffix=&#x27;.pdf&#x27;) as tmp_file:
            tmp_file.write(file.getvalue())
            tmp_path = tmp_file.name
            
        loader = PyPDFLoader(tmp_path)
        documents = loader.load()
        text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
        texts = text_splitter.split_documents(documents)
        
        os.unlink(tmp_path)
        
        return texts
    except Exception as e:
        st.error(f&quot;Error processing document: {e}&quot;)
        return []

COLLECTION_NAME = &quot;cohere_rag&quot;

def create_vector_stores(texts):
    &quot;&quot;&quot;Create and populate vector store with documents.&quot;&quot;&quot;
    try:
        try:
            client.create_collection(collection_name=COLLECTION_NAME,
                                   vectors_config=VectorParams(size=1024,
                                                            distance=Distance.COSINE))
            st.success(f&quot;Created new collection: {COLLECTION_NAME}&quot;)
        except Exception as e:
            if &quot;already exists&quot; not in str(e).lower():
                raise e
        
        vector_store = QdrantVectorStore(client=client,
                                       collection_name=COLLECTION_NAME,
                                       embedding=embedding)
        
        with st.spinner(&#x27;Storing documents in Qdrant...&#x27;):
            vector_store.add_documents(texts)
            st.success(&quot;Documents successfully stored in Qdrant!&quot;)
        
        return vector_store
        
    except Exception as e:
        st.error(f&quot;Error in vector store creation: {str(e)}&quot;)
        return None

# Define the state schema using TypedDict
class AgentState(TypedDict):
    &quot;&quot;&quot;State schema for the agent.&quot;&quot;&quot;
    messages: List[HumanMessage | AIMessage | SystemMessage]
    is_last_step: bool

class RateLimitedDuckDuckGo(DuckDuckGoSearchRun):
    @retry(wait=wait_exponential(multiplier=1, min=4, max=10),
           stop=stop_after_attempt(3))
    def run(self, query: str) -&gt; str:
        &quot;&quot;&quot;Run search with rate limiting.&quot;&quot;&quot;
        try:
            sleep(2)  # Add delay between requests
            return super().run(query)
        except Exception as e:
            if &quot;Ratelimit&quot; in str(e):
                sleep(5)  # Longer delay on rate limit
                return super().run(query)
            raise e

def create_fallback_agent(chat_model: BaseLanguageModel):
    &quot;&quot;&quot;Create a LangGraph agent for web research.&quot;&quot;&quot;
    
    def web_research(query: str) -&gt; str:
        &quot;&quot;&quot;Web search with result formatting.&quot;&quot;&quot;
        try:
            search = DuckDuckGoSearchRun(num_results=5)
            results = search.run(query)
            return results
        except Exception as e:
            return f&quot;Search failed: {str(e)}. Providing answer based on general knowledge.&quot;

    tools = [web_research]
    
    agent = create_react_agent(model=chat_model,
                             tools=tools,
                             debug=False)
    
    return agent

def process_query(vectorstore, query) -&gt; tuple[str, list]:
    &quot;&quot;&quot;Process a query using RAG with fallback to web search.&quot;&quot;&quot;
    try:
        retriever = vectorstore.as_retriever(
            search_type=&quot;similarity_score_threshold&quot;,
            search_kwargs={
                &quot;k&quot;: 10,
                &quot;score_threshold&quot;: 0.7
            }
        )

        relevant_docs = retriever.get_relevant_documents(query)

        if relevant_docs:
            retrieval_qa_prompt = hub.pull(&quot;langchain-ai/retrieval-qa-chat&quot;)
            combine_docs_chain = create_stuff_documents_chain(chat_model, retrieval_qa_prompt)
            retrieval_chain = create_retrieval_chain(retriever, combine_docs_chain)
            response = retrieval_chain.invoke({&quot;input&quot;: query})
            return response[&#x27;answer&#x27;], relevant_docs
            
        else:
            st.info(&quot;No relevant documents found. Searching web...&quot;)
            fallback_agent = create_fallback_agent(chat_model)
            
            with st.spinner(&#x27;Researching...&#x27;):
                agent_input = {
                    &quot;messages&quot;: [
                        HumanMessage(content=f&quot;&quot;&quot;Please thoroughly research the question: &#x27;{query}&#x27; and provide a detailed and comprehensive response. Make sure to gather the latest information from credible sources. Minimum 400 words.&quot;&quot;&quot;)
                    ],
                    &quot;is_last_step&quot;: False
                }
                
                config = {&quot;recursion_limit&quot;: 100}
                
                try:
                    response = fallback_agent.invoke(agent_input, config=config)
                    
                    if isinstance(response, dict) and &quot;messages&quot; in response:
                        last_message = response[&quot;messages&quot;][-1]
                        answer = last_message.content if hasattr(last_message, &#x27;content&#x27;) else str(last_message)
                        
                        return f&quot;&quot;&quot;Web Search Result:
{answer}
&quot;&quot;&quot;, []
                    
                except Exception as agent_error:
                    fallback_response = chat_model.invoke(f&quot;Please provide a general answer to: {query}&quot;).content
                    return f&quot;Web search unavailable. General response: {fallback_response}&quot;, []

    except Exception as e:
        st.error(f&quot;Error: {str(e)}&quot;)
        return &quot;I encountered an error. Please try rephrasing your question.&quot;, []

def post_process(answer, sources):
    &quot;&quot;&quot;Post-process the answer and format sources.&quot;&quot;&quot;
    answer = answer.strip()

    # Summarize long answers
    if len(answer) &gt; 500:
        summary_prompt = f&quot;Summarize the following answer in 2-3 sentences: {answer}&quot;
        summary = chat_model.invoke(summary_prompt).content
        answer = f&quot;{summary}\n\nFull Answer: {answer}&quot;
    
    formatted_sources = []
    for i, source in enumerate(sources, 1):
        formatted_source = f&quot;{i}. {source.page_content[:200]}...&quot;
        formatted_sources.append(formatted_source)
    return answer, formatted_sources

st.title(&quot;RAG Agent with Cohere ⌘R&quot;)

uploaded_file = st.file_uploader(&quot;Choose a PDF or Image File&quot;, type=[&quot;pdf&quot;, &quot;jpg&quot;, &quot;jpeg&quot;])

if uploaded_file is not None and &#x27;processed_file&#x27; not in st.session_state:
    with st.spinner(&#x27;Processing file... This may take a while for images.&#x27;):
        texts = process_document(uploaded_file)
        vectorstore = create_vector_stores(texts)
        if vectorstore:
            st.session_state.vectorstore = vectorstore
            st.session_state.processed_file = True
            st.success(&#x27;File uploaded and processed successfully!&#x27;)
        else:
            st.error(&#x27;Failed to process file. Please try again.&#x27;)

for message in st.session_state.chat_history:
    with st.chat_message(message[&quot;role&quot;]):
        st.markdown(message[&quot;content&quot;])

if query := st.chat_input(&quot;Ask a question about the document:&quot;):
    st.session_state.chat_history.append({&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: query})
    with st.chat_message(&quot;user&quot;):
        st.markdown(query)

    if st.session_state.vectorstore:
        with st.chat_message(&quot;assistant&quot;):
            try:
                answer, sources = process_query(st.session_state.vectorstore, query)
                st.markdown(answer)
                
                if sources:
                    with st.expander(&quot;Sources&quot;):
                        for source in sources:
                            st.markdown(f&quot;- {source.page_content[:200]}...&quot;)
                
                st.session_state.chat_history.append({
                    &quot;role&quot;: &quot;assistant&quot;,
                    &quot;content&quot;: answer
                })
                
            except Exception as e:
                st.error(f&quot;Error: {str(e)}&quot;)
                st.info(&quot;Please try asking your question again.&quot;)
    else:
        st.error(&quot;Please upload a document first.&quot;)

with st.sidebar:
    st.divider()
    col1, col2 = st.columns(2)
    with col1:
        if st.button(&#x27;Clear Chat History&#x27;):
            st.session_state.chat_history = []
            st.rerun()
    with col2:
        if st.button(&#x27;Clear All Data&#x27;):
            try:
                collections = client.get_collections().collections
                collection_names = [col.name for col in collections]
                
                if COLLECTION_NAME in collection_names:
                    client.delete_collection(COLLECTION_NAME)
                if f&quot;{COLLECTION_NAME}_compressed&quot; in collection_names:
                    client.delete_collection(f&quot;{COLLECTION_NAME}_compressed&quot;)
                
                st.session_state.vectorstore = None
                st.session_state.chat_history = []
                st.success(&quot;All data cleared successfully!&quot;)
                st.rerun()
            except Exception as e:
                st.error(f&quot;Error clearing data: {str(e)}&quot;)
```