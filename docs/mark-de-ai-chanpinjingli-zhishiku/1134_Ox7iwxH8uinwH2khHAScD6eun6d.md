# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/VC6pdCi5loFTN1xqbpecqjoencr](https://qqs7y1hozd1.feishu.cn/docx/VC6pdCi5loFTN1xqbpecqjoencr)

```
import os
from typing import List, Dict, Any, Literal, Optional
from dataclasses import dataclass
import streamlit as st
from langchain_core.documents import Document
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.document_loaders import PyPDFLoader
from langchain_community.vectorstores import Qdrant
from langchain_openai import OpenAIEmbeddings
from langchain_openai import ChatOpenAI
import tempfile
from agno.agent import Agent
from agno.models.openai import OpenAIChat
from langchain.schema import HumanMessage
from langchain.chains.combine_documents import create_stuff_documents_chain
from langchain.chains import create_retrieval_chain
from langchain import hub
from langgraph.prebuilt import create_react_agent
from langchain_community.tools import DuckDuckGoSearchRun
from langchain_core.language_models import BaseLanguageModel
from langchain.prompts import ChatPromptTemplate
from qdrant_client import QdrantClient
from qdrant_client.models import Distance, VectorParams

def init_session_state():
    &quot;&quot;&quot;Initialize session state variables&quot;&quot;&quot;
    if &#x27;openai_api_key&#x27; not in st.session_state:
        st.session_state.openai_api_key = &quot;&quot;
    if &#x27;qdrant_url&#x27; not in st.session_state:
        st.session_state.qdrant_url = &quot;&quot;
    if &#x27;qdrant_api_key&#x27; not in st.session_state:
        st.session_state.qdrant_api_key = &quot;&quot;
    if &#x27;embeddings&#x27; not in st.session_state:
        st.session_state.embeddings = None
    if &#x27;llm&#x27; not in st.session_state:
        st.session_state.llm = None
    if &#x27;databases&#x27; not in st.session_state:
        st.session_state.databases = {}

init_session_state()

DatabaseType = Literal[&quot;products&quot;, &quot;support&quot;, &quot;finance&quot;]
PERSIST_DIRECTORY = &quot;db_storage&quot;

@dataclass
class CollectionConfig:
    name: str
    description: str
    collection_name: str  # This will be used as Qdrant collection name

# Collection configurations
COLLECTIONS: Dict[DatabaseType, CollectionConfig] = {
    &quot;products&quot;: CollectionConfig(
        name=&quot;Product Information&quot;,
        description=&quot;Product details, specifications, and features&quot;,
        collection_name=&quot;products_collection&quot;
    ),
    &quot;support&quot;: CollectionConfig(
        name=&quot;Customer Support &amp; FAQ&quot;,
        description=&quot;Customer support information, frequently asked questions, and guides&quot;,
        collection_name=&quot;support_collection&quot;
    ),
    &quot;finance&quot;: CollectionConfig(
        name=&quot;Financial Information&quot;,
        description=&quot;Financial data, revenue, costs, and liabilities&quot;,
        collection_name=&quot;finance_collection&quot;
    )
}

def initialize_models():
    &quot;&quot;&quot;Initialize OpenAI models and Qdrant client&quot;&quot;&quot;
    if (st.session_state.openai_api_key and 
        st.session_state.qdrant_url and 
        st.session_state.qdrant_api_key):
        
        os.environ[&quot;OPENAI_API_KEY&quot;] = st.session_state.openai_api_key
        st.session_state.embeddings = OpenAIEmbeddings(model=&quot;text-embedding-3-small&quot;)
        st.session_state.llm = ChatOpenAI(temperature=0)
        
        try:
            client = QdrantClient(
                url=st.session_state.qdrant_url,
                api_key=st.session_state.qdrant_api_key
            )
            
            # Test connection
            client.get_collections()
            vector_size = 1536  
            st.session_state.databases = {}
            for db_type, config in COLLECTIONS.items():
                try:
                    client.get_collection(config.collection_name)
                except Exception:
                    # Create collection if it doesn&#x27;t exist
                    client.create_collection(
                        collection_name=config.collection_name,
                        vectors_config=VectorParams(size=vector_size, distance=Distance.COSINE)
                    )
                
                st.session_state.databases[db_type] = Qdrant(
                    client=client,
                    collection_name=config.collection_name,
                    embeddings=st.session_state.embeddings
                )
            
            return True
        except Exception as e:
            st.error(f&quot;Failed to connect to Qdrant: {str(e)}&quot;)
            return False
    return False

def process_document(file) -&gt; List[Document]:
    &quot;&quot;&quot;Process uploaded PDF document&quot;&quot;&quot;
    try:
        with tempfile.NamedTemporaryFile(delete=False, suffix=&#x27;.pdf&#x27;) as tmp_file:
            tmp_file.write(file.getvalue())
            tmp_path = tmp_file.name
            
        loader = PyPDFLoader(tmp_path)
        documents = loader.load()
        
        # Clean up temporary file
        os.unlink(tmp_path)
        
        text_splitter = RecursiveCharacterTextSplitter(
            chunk_size=1000,
            chunk_overlap=200
        )
        texts = text_splitter.split_documents(documents)
        
        return texts
    except Exception as e:
        st.error(f&quot;Error processing document: {e}&quot;)
        return []

def create_routing_agent() -&gt; Agent:
    &quot;&quot;&quot;Creates a routing agent using agno framework&quot;&quot;&quot;
    return Agent(
        model=OpenAIChat(
            id=&quot;gpt-4o&quot;,
            api_key=st.session_state.openai_api_key
        ),
        tools=[],
        description=&quot;&quot;&quot;You are a query routing expert. Your only job is to analyze questions and determine which database they should be routed to.
        You must respond with exactly one of these three options: &#x27;products&#x27;, &#x27;support&#x27;, or &#x27;finance&#x27;. The user&#x27;s question is: {question}&quot;&quot;&quot;,
        instructions=[
            &quot;Follow these rules strictly:&quot;,
            &quot;1. For questions about products, features, specifications, or item details, or product manuals → return &#x27;products&#x27;&quot;,
            &quot;2. For questions about help, guidance, troubleshooting, or customer service, FAQ, or guides → return &#x27;support&#x27;&quot;,
            &quot;3. For questions about costs, revenue, pricing, or financial data, or financial reports and investments → return &#x27;finance&#x27;&quot;,
            &quot;4. Return ONLY the database name, no other text or explanation&quot;,
            &quot;5. If you&#x27;re not confident about the routing, return an empty response&quot;
        ],
        markdown=False,
        show_tool_calls=False
    )

def route_query(question: str) -&gt; Optional[DatabaseType]:
    &quot;&quot;&quot;Route query by searching all databases and comparing relevance scores.
    Returns None if no suitable database is found.&quot;&quot;&quot;
    try:
        best_score = -1
        best_db_type = None
        all_scores = {}  # Store all scores for debugging
        
        # Search each database and compare relevance scores
        for db_type, db in st.session_state.databases.items():
            results = db.similarity_search_with_score(
                question,
                k=3
            )
            
            if results:
                avg_score = sum(score for _, score in results) / len(results)
                all_scores[db_type] = avg_score
                
                if avg_score &gt; best_score:
                    best_score = avg_score
                    best_db_type = db_type
        
        confidence_threshold = 0.5
        if best_score &gt;= confidence_threshold and best_db_type:
            st.success(f&quot;Using vector similarity routing: {best_db_type} (confidence: {best_score:.3f})&quot;)
            return best_db_type
            
        st.warning(f&quot;Low confidence scores (below {confidence_threshold}), falling back to LLM routing&quot;)
        
        # Fallback to LLM routing
        routing_agent = create_routing_agent()
        response = routing_agent.run(question)
        
        db_type = (response.content
                  .strip()
                  .lower()
                  .translate(str.maketrans(&#x27;&#x27;, &#x27;&#x27;, &#x27;`\&#x27;&quot;&#x27;)))
        
        if db_type in COLLECTIONS:
            st.success(f&quot;Using LLM routing decision: {db_type}&quot;)
            return db_type
            
        st.warning(&quot;No suitable database found, will use web search fallback&quot;)
        return None
        
    except Exception as e:
        st.error(f&quot;Routing error: {str(e)}&quot;)
        return None

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

def query_database(db: Qdrant, question: str) -&gt; tuple[str, list]:
    &quot;&quot;&quot;Query the database and return answer and relevant documents&quot;&quot;&quot;
    try:
        retriever = db.as_retriever(
            search_type=&quot;similarity&quot;,
            search_kwargs={&quot;k&quot;: 4}
        )

        relevant_docs = retriever.get_relevant_documents(question)

        if relevant_docs:
            # Use simpler chain creation with hub prompt
            retrieval_qa_prompt = ChatPromptTemplate.from_messages([
                (&quot;system&quot;, &quot;&quot;&quot;You are a helpful AI assistant that answers questions based on provided context.
                             Always be direct and concise in your responses.
                             If the context doesn&#x27;t contain enough information to fully answer the question, acknowledge this limitation.
                             Base your answers strictly on the provided context and avoid making assumptions.&quot;&quot;&quot;),
                (&quot;human&quot;, &quot;Here is the context:\n{context}&quot;),
                (&quot;human&quot;, &quot;Question: {input}&quot;),
                (&quot;assistant&quot;, &quot;I&#x27;ll help answer your question based on the context provided.&quot;),
                (&quot;human&quot;, &quot;Please provide your answer:&quot;),
            ])
            combine_docs_chain = create_stuff_documents_chain(st.session_state.llm, retrieval_qa_prompt)
            retrieval_chain = create_retrieval_chain(retriever, combine_docs_chain)
            
            response = retrieval_chain.invoke({&quot;input&quot;: question})
            return response[&#x27;answer&#x27;], relevant_docs
        
        raise ValueError(&quot;No relevant documents found in database&quot;)

    except Exception as e:
        st.error(f&quot;Error: {str(e)}&quot;)
        return &quot;I encountered an error. Please try rephrasing your question.&quot;, []
    
def _handle_web_fallback(question: str) -&gt; tuple[str, list]:
    st.info(&quot;No relevant documents found. Searching web...&quot;)
    fallback_agent = create_fallback_agent(st.session_state.llm)
    
    with st.spinner(&#x27;Researching...&#x27;):
        agent_input = {
            &quot;messages&quot;: [
                HumanMessage(content=f&quot;Research and provide a detailed answer for: &#x27;{question}&#x27;&quot;)
            ],
            &quot;is_last_step&quot;: False
        }
        
        try:
            response = fallback_agent.invoke(agent_input, config={&quot;recursion_limit&quot;: 100})
            if isinstance(response, dict) and &quot;messages&quot; in response:
                answer = response[&quot;messages&quot;][-1].content
                return f&quot;Web Search Result:\n{answer}&quot;, []
                
        except Exception:
            # Fallback to general LLM response
            fallback_response = st.session_state.llm.invoke(question).content
            return f&quot;Web search unavailable. General response: {fallback_response}&quot;, []

def main():
    &quot;&quot;&quot;Main application function.&quot;&quot;&quot;
    st.set_page_config(page_title=&quot;RAG Agent with Database Routing&quot;, page_icon=&quot;📚&quot;)
    st.title(&quot;📠 RAG Agent with Database Routing&quot;)
    
    # Sidebar for API keys and configuration
    with st.sidebar:
        st.header(&quot;Configuration&quot;)
        
        # OpenAI API Key
        api_key = st.text_input(
            &quot;Enter OpenAI API Key:&quot;,
            type=&quot;password&quot;,
            value=st.session_state.openai_api_key,
            key=&quot;api_key_input&quot;
        )
        
        # Qdrant Configuration
        qdrant_url = st.text_input(
            &quot;Enter Qdrant URL:&quot;,
            value=st.session_state.qdrant_url,
            help=&quot;Example: https://your-cluster.qdrant.tech&quot;
        )
        
        qdrant_api_key = st.text_input(
            &quot;Enter Qdrant API Key:&quot;,
            type=&quot;password&quot;,
            value=st.session_state.qdrant_api_key
        )
        
        # Update session state
        if api_key:
            st.session_state.openai_api_key = api_key
        if qdrant_url:
            st.session_state.qdrant_url = qdrant_url
        if qdrant_api_key:
            st.session_state.qdrant_api_key = qdrant_api_key
            
        # Initialize models if all credentials are provided
        if (st.session_state.openai_api_key and 
            st.session_state.qdrant_url and 
            st.session_state.qdrant_api_key):
            if initialize_models():
                st.success(&quot;Connected to OpenAI and Qdrant successfully!&quot;)
            else:
                st.error(&quot;Failed to initialize. Please check your credentials.&quot;)
        else:
            st.warning(&quot;Please enter all required credentials to continue&quot;)
            st.stop()

        st.markdown(&quot;---&quot;)

    st.header(&quot;Document Upload&quot;)
    st.info(&quot;Upload documents to populate the databases. Each tab corresponds to a different database.&quot;)
    tabs = st.tabs([collection_config.name for collection_config in COLLECTIONS.values()])
    
    for (collection_type, collection_config), tab in zip(COLLECTIONS.items(), tabs):
        with tab:
            st.write(collection_config.description)
            uploaded_files = st.file_uploader(
                f&quot;Upload PDF documents to {collection_config.name}&quot;,
                type=&quot;pdf&quot;,
                key=f&quot;upload_{collection_type}&quot;,
                accept_multiple_files=True  
            )
            
            if uploaded_files:
                with st.spinner(&#x27;Processing documents...&#x27;):
                    all_texts = []
                    for uploaded_file in uploaded_files:
                        texts = process_document(uploaded_file)
                        all_texts.extend(texts)
                    
                    if all_texts:
                        db = st.session_state.databases[collection_type]
                        db.add_documents(all_texts)
                        st.success(&quot;Documents processed and added to the database!&quot;)
    
    # Query section
    st.header(&quot;Ask Questions&quot;)
    st.info(&quot;Enter your question below to find answers from the relevant database.&quot;)
    question = st.text_input(&quot;Enter your question:&quot;)
    
    if question:
        with st.spinner(&#x27;Finding answer...&#x27;):
            # Route the question
            collection_type = route_query(question)
            
            if collection_type is None:
                # Use web search fallback directly
                answer, relevant_docs = _handle_web_fallback(question)
                st.write(&quot;### Answer (from web search)&quot;)
                st.write(answer)
            else:
                # Display routing information and query the database
                st.info(f&quot;Routing question to: {COLLECTIONS[collection_type].name}&quot;)
                db = st.session_state.databases[collection_type]
                answer, relevant_docs = query_database(db, question)
                st.write(&quot;### Answer&quot;)
                st.write(answer)

if __name__ == &quot;__main__&quot;:
    main()
```