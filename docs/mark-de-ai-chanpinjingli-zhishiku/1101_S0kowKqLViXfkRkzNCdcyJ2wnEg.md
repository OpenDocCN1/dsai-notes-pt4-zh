# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/WNeEdi0ZDoUKWnxZgy1cSEkUn7d](https://qqs7y1hozd1.feishu.cn/docx/WNeEdi0ZDoUKWnxZgy1cSEkUn7d)

```
from langchain import hub
from langchain.output_parsers import PydanticOutputParser
from langchain_core.output_parsers import StrOutputParser
from langchain.schema import Document
from pydantic import BaseModel, Field
import streamlit as st
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.document_loaders import PyPDFLoader, TextLoader, WebBaseLoader
from langchain_community.tools import TavilySearchResults
from langchain_community.vectorstores import Qdrant
from langchain_openai import OpenAIEmbeddings, ChatOpenAI
from langchain_core.messages import HumanMessage        
from langgraph.graph import END, StateGraph
from typing import Dict, TypedDict
from langchain_core.prompts import PromptTemplate
import pprint
import yaml
import nest_asyncio
from qdrant_client import QdrantClient
from qdrant_client.models import Distance, VectorParams
import tempfile
import os
from langchain_anthropic import ChatAnthropic
from tenacity import retry, stop_after_attempt, wait_exponential


nest_asyncio.apply()

retriever = None

def initialize_session_state():
    &quot;&quot;&quot;Initialize session state variables for API keys and URLs.&quot;&quot;&quot;
    if &#x27;initialized&#x27; not in st.session_state:
        st.session_state.initialized = False
        # Initialize API keys and URLs
        st.session_state.anthropic_api_key = &quot;&quot;
        st.session_state.openai_api_key = &quot;&quot;
        st.session_state.tavily_api_key = &quot;&quot;
        st.session_state.qdrant_api_key = &quot;&quot;
        st.session_state.qdrant_url = &quot;http://localhost:6333&quot;
        st.session_state.doc_url = &quot;https://arxiv.org/pdf/2307.09288.pdf&quot;  
        
def setup_sidebar():
    &quot;&quot;&quot;Setup sidebar for API keys and configuration.&quot;&quot;&quot;
    with st.sidebar:
        st.subheader(&quot;API Configuration&quot;)
        st.session_state.anthropic_api_key = st.text_input(&quot;Anthropic API Key&quot;, value=st.session_state.anthropic_api_key, type=&quot;password&quot;, help=&quot;Required for Claude 3 model&quot;)
        st.session_state.openai_api_key = st.text_input(&quot;OpenAI API Key&quot;, value=st.session_state.openai_api_key, type=&quot;password&quot;)
        st.session_state.tavily_api_key = st.text_input(&quot;Tavily API Key&quot;, value=st.session_state.tavily_api_key, type=&quot;password&quot;)
        st.session_state.qdrant_url = st.text_input(&quot;Qdrant URL&quot;, value=st.session_state.qdrant_url)
        st.session_state.qdrant_api_key = st.text_input(&quot;Qdrant API Key&quot;, value=st.session_state.qdrant_api_key, type=&quot;password&quot;)
        st.session_state.doc_url = st.text_input(&quot;Document URL&quot;, value=st.session_state.doc_url)
        
        if not all([st.session_state.openai_api_key, st.session_state.anthropic_api_key, st.session_state.qdrant_url]):
            st.warning(&quot;Please provide the required API keys and URLs&quot;)
            st.stop()
        
        st.session_state.initialized = True

initialize_session_state()
setup_sidebar()

# Use session state variables instead of config
openai_api_key = st.session_state.openai_api_key
tavily_api_key = st.session_state.tavily_api_key
anthropic_api_key = st.session_state.anthropic_api_key

# Update embeddings initialization
embeddings = OpenAIEmbeddings(
    model=&quot;text-embedding-3-small&quot;,
    api_key=st.session_state.openai_api_key
)

# Update Qdrant client initialization
client = QdrantClient(
    url=st.session_state.qdrant_url,
    api_key=st.session_state.qdrant_api_key
)

@retry(stop=stop_after_attempt(3), wait=wait_exponential(multiplier=1, min=4, max=10))
def execute_tavily_search(tool, query):
    return tool.invoke({&quot;query&quot;: query})

def web_search(state):
    &quot;&quot;&quot;Web search based on the re-phrased question using Tavily API.&quot;&quot;&quot;
    print(&quot;~-web search-~&quot;)
    state_dict = state[&quot;keys&quot;]
    question = state_dict[&quot;question&quot;]
    documents = state_dict[&quot;documents&quot;]
    
    # Create progress placeholder
    progress_placeholder = st.empty()
    progress_placeholder.info(&quot;Initiating web search...&quot;)
    
    try:
        # Validate Tavily API key
        if not st.session_state.tavily_api_key:
            progress_placeholder.warning(&quot;Tavily API key not provided - skipping web search&quot;)
            return {&quot;keys&quot;: {&quot;documents&quot;: documents, &quot;question&quot;: question}}
        
        progress_placeholder.info(&quot;Configuring search tool...&quot;)
        
        # Initialize Tavily search tool
        tool = TavilySearchResults(
            api_key=st.session_state.tavily_api_key,
            max_results=3,
            search_depth=&quot;advanced&quot;
        )
        
        # Execute search with retry logic
        progress_placeholder.info(&quot;Executing search query...&quot;)
        try:
            search_results = execute_tavily_search(tool, question)
        except Exception as search_error:
            progress_placeholder.error(f&quot;Search failed after retries: {str(search_error)}&quot;)
            return {&quot;keys&quot;: {&quot;documents&quot;: documents, &quot;question&quot;: question}}
        
        if not search_results:
            progress_placeholder.warning(&quot;No search results found&quot;)
            return {&quot;keys&quot;: {&quot;documents&quot;: documents, &quot;question&quot;: question}}
        
        # Process results
        progress_placeholder.info(&quot;Processing search results...&quot;)
        web_results = []
        for result in search_results:
            # Extract and format relevant information
            content = (
                f&quot;Title: {result.get(&#x27;title&#x27;, &#x27;No title&#x27;)}\n&quot;
                f&quot;Content: {result.get(&#x27;content&#x27;, &#x27;No content&#x27;)}\n&quot;
            )
            web_results.append(content)
        
        # Create document from results
        web_document = Document(
            page_content=&quot;\n\n&quot;.join(web_results),
            metadata={
                &quot;source&quot;: &quot;tavily_search&quot;,
                &quot;query&quot;: question,
                &quot;result_count&quot;: len(web_results)
            }
        )
        documents.append(web_document)
        
        progress_placeholder.success(f&quot;Successfully added {len(web_results)} search results&quot;)
        
    except Exception as error:
        error_msg = f&quot;Web search error: {str(error)}&quot;
        print(error_msg)
        progress_placeholder.error(error_msg)
    finally:
        progress_placeholder.empty()
    return {&quot;keys&quot;: {&quot;documents&quot;: documents, &quot;question&quot;: question}}


def load_documents(file_or_url: str, is_url: bool = True) -&gt; list:
    try:
        if is_url:
            loader = WebBaseLoader(file_or_url)
            loader.requests_per_second = 1
        else:
            file_extension = os.path.splitext(file_or_url)[1].lower()
            if file_extension == &#x27;.pdf&#x27;:
                loader = PyPDFLoader(file_or_url)
            elif file_extension in [&#x27;.txt&#x27;, &#x27;.md&#x27;]:
                loader = TextLoader(file_or_url)
            else:
                raise ValueError(f&quot;Unsupported file type: {file_extension}&quot;)
        
        return loader.load()
    except Exception as e:
        st.error(f&quot;Error loading document: {str(e)}&quot;)
        return []

st.subheader(&quot;Document Input&quot;)
input_option = st.radio(&quot;Choose input method:&quot;, [&quot;URL&quot;, &quot;File Upload&quot;])

docs = None  

if input_option == &quot;URL&quot;:
    url = st.text_input(&quot;Enter document URL:&quot;, value=st.session_state.doc_url)
    if url:
        docs = load_documents(url, is_url=True)
else:
    uploaded_file = st.file_uploader(&quot;Upload a document&quot;, type=[&#x27;pdf&#x27;, &#x27;txt&#x27;, &#x27;md&#x27;])
    if uploaded_file:
        # Create a temporary file to store the upload
        with tempfile.NamedTemporaryFile(delete=False, suffix=os.path.splitext(uploaded_file.name)[1]) as tmp_file:
            tmp_file.write(uploaded_file.getvalue())
            docs = load_documents(tmp_file.name, is_url=False)
        # Clean up the temporary file
        os.unlink(tmp_file.name)

if docs:
    text_splitter = RecursiveCharacterTextSplitter.from_tiktoken_encoder(
        chunk_size=500, chunk_overlap=100
    )
    all_splits = text_splitter.split_documents(docs)

    client = QdrantClient(url=st.session_state.qdrant_url, api_key=st.session_state.qdrant_api_key)
    collection_name = &quot;rag-qdrant&quot;

    try:
        # Try to delete the collection if it exists
        client.delete_collection(collection_name)
    except Exception:
        pass  

    client.create_collection(
        collection_name=collection_name,
        vectors_config=VectorParams(size=1536, distance=Distance.COSINE),
    )

    # Create vectorstore
    vectorstore = Qdrant(
        client=client,
        collection_name=collection_name,
        embeddings=embeddings,
    )

    # Add documents to the vectorstore
    vectorstore.add_documents(all_splits)
    retriever = vectorstore.as_retriever()


class GraphState(TypedDict):
    keys: Dict[str, any]


def retrieve(state):
    print(&quot;~-retrieve-~&quot;)
    state_dict = state[&quot;keys&quot;]
    question = state_dict[&quot;question&quot;]
    
    if retriever is None:
        return {&quot;keys&quot;: {&quot;documents&quot;: [], &quot;question&quot;: question}}
        
    documents = retriever.get_relevant_documents(question)
    return {&quot;keys&quot;: {&quot;documents&quot;: documents, &quot;question&quot;: question}}


def generate(state):
    &quot;&quot;&quot;Generate answer using Claude 3 model&quot;&quot;&quot;
    print(&quot;~-generate-~&quot;)
    state_dict = state[&quot;keys&quot;]
    question, documents = state_dict[&quot;question&quot;], state_dict[&quot;documents&quot;]
    try:
        prompt = PromptTemplate(template=&quot;&quot;&quot;Based on the following context, please answer the question.
            Context: {context}
            Question: {question}
            Answer:&quot;&quot;&quot;, input_variables=[&quot;context&quot;, &quot;question&quot;])
        llm = ChatAnthropic(model=&quot;claude-3-5-sonnet-20241022&quot;, api_key=st.session_state.anthropic_api_key,
                           temperature=0, max_tokens=1000)
        context = &quot;\n\n&quot;.join(doc.page_content for doc in documents)

        # Create and run chain
        rag_chain = (
            {&quot;context&quot;: lambda x: context, &quot;question&quot;: lambda x: question} 
            | prompt 
            | llm 
            | StrOutputParser()
        )

        generation = rag_chain.invoke({})

        return {
            &quot;keys&quot;: {
                &quot;documents&quot;: documents,
                &quot;question&quot;: question,
                &quot;generation&quot;: generation
            }
        }

    except Exception as e:
        error_msg = f&quot;Error in generate function: {str(e)}&quot;
        print(error_msg)
        st.error(error_msg)
        return {&quot;keys&quot;: {&quot;documents&quot;: documents, &quot;question&quot;: question, 
                &quot;generation&quot;: &quot;Sorry, I encountered an error while generating the response.&quot;}}

def grade_documents(state):
    &quot;&quot;&quot;Determines whether the retrieved documents are relevant.&quot;&quot;&quot;
    print(&quot;~-check relevance-~&quot;)
    state_dict = state[&quot;keys&quot;]
    question = state_dict[&quot;question&quot;]
    documents = state_dict[&quot;documents&quot;]

    llm = ChatAnthropic(model=&quot;claude-3-5-sonnet-20241022&quot;, api_key=st.session_state.anthropic_api_key,
                       temperature=0, max_tokens=1000)

    prompt = PromptTemplate(template=&quot;&quot;&quot;You are grading the relevance of a retrieved document to a user question.
        Return ONLY a JSON object with a &quot;score&quot; field that is either &quot;yes&quot; or &quot;no&quot;.
        Do not include any other text or explanation.
        
        Document: {context}
        Question: {question}
        
        Rules:
        - Check for related keywords or semantic meaning
        - Use lenient grading to only filter clear mismatches
        - Return exactly like this example: {{&quot;score&quot;: &quot;yes&quot;}} or {{&quot;score&quot;: &quot;no&quot;}}&quot;&quot;&quot;,
        input_variables=[&quot;context&quot;, &quot;question&quot;])

    chain = (
        prompt 
        | llm 
        | StrOutputParser()
    )

    filtered_docs = []
    search = &quot;No&quot;
    
    for d in documents:
        try:
            response = chain.invoke({&quot;question&quot;: question, &quot;context&quot;: d.page_content})
            import re
            json_match = re.search(r&#x27;\{.*\}&#x27;, response)
            if json_match:
                response = json_match.group()
            
            import json
            score = json.loads(response)
            
            if score.get(&quot;score&quot;) == &quot;yes&quot;:
                print(&quot;~-grade: document relevant-~&quot;)
                filtered_docs.append(d)
            else:
                print(&quot;~-grade: document not relevant-~&quot;)
                search = &quot;Yes&quot;
                
        except Exception as e:
            print(f&quot;Error grading document: {str(e)}&quot;)
            # On error, keep the document to be safe
            filtered_docs.append(d)
            continue

    return {&quot;keys&quot;: {&quot;documents&quot;: filtered_docs, &quot;question&quot;: question, &quot;run_web_search&quot;: search}}


def transform_query(state):
    &quot;&quot;&quot;Transform the query to produce a better question.&quot;&quot;&quot;
    print(&quot;~-transform query-~&quot;)
    state_dict = state[&quot;keys&quot;]
    question = state_dict[&quot;question&quot;]
    documents = state_dict[&quot;documents&quot;]

    # Create a prompt template
    prompt = PromptTemplate(
        template=&quot;&quot;&quot;Generate a search-optimized version of this question by 
        analyzing its core semantic meaning and intent.
        \n ------- \n
        {question}
        \n ------- \n
        Return only the improved question with no additional text:&quot;&quot;&quot;,
        input_variables=[&quot;question&quot;],
    )

    # Use Claude instead of Gemini
    llm = ChatAnthropic(
        model=&quot;claude-3-5-sonnet-20240620&quot;,
        anthropic_api_key=st.session_state.anthropic_api_key,
        temperature=0,
        max_tokens=1000
    )

    # Prompt
    chain = prompt | llm | StrOutputParser()
    better_question = chain.invoke({&quot;question&quot;: question})

    return {
        &quot;keys&quot;: {&quot;documents&quot;: documents, &quot;question&quot;: better_question}
    }


def decide_to_generate(state):
    print(&quot;~-decide to generate-~&quot;)
    state_dict = state[&quot;keys&quot;]
    search = state_dict[&quot;run_web_search&quot;]

    if search == &quot;Yes&quot;:
     
        print(&quot;~-decision: transform query and run web search-~&quot;)
        return &quot;transform_query&quot;
    else:
        print(&quot;~-decision: generate-~&quot;)
        return &quot;generate&quot;
    
def format_document(doc: Document) -&gt; str:
    return f&quot;&quot;&quot;
    Source: {doc.metadata.get(&#x27;source&#x27;, &#x27;Unknown&#x27;)}
    Title: {doc.metadata.get(&#x27;title&#x27;, &#x27;No title&#x27;)}
    Content: {doc.page_content[:200]}...
    &quot;&quot;&quot;

def format_state(state: dict) -&gt; str:
    formatted = {}
    
    for key, value in state.items():
        if key == &quot;documents&quot;:
            formatted[key] = [format_document(doc) for doc in value]
        else:
            formatted[key] = value
            
    return formatted


workflow = StateGraph(GraphState)

# Define the nodes by langgraph
workflow.add_node(&quot;retrieve&quot;, retrieve) 
workflow.add_node(&quot;grade_documents&quot;, grade_documents)  
workflow.add_node(&quot;generate&quot;, generate) 
workflow.add_node(&quot;transform_query&quot;, transform_query)  
workflow.add_node(&quot;web_search&quot;, web_search) 

# Build graph
workflow.set_entry_point(&quot;retrieve&quot;)
workflow.add_edge(&quot;retrieve&quot;, &quot;grade_documents&quot;)
workflow.add_conditional_edges(
    &quot;grade_documents&quot;,
    decide_to_generate,
    {
        &quot;transform_query&quot;: &quot;transform_query&quot;,
        &quot;generate&quot;: &quot;generate&quot;,
    },
)
workflow.add_edge(&quot;transform_query&quot;, &quot;web_search&quot;)
workflow.add_edge(&quot;web_search&quot;, &quot;generate&quot;)
workflow.add_edge(&quot;generate&quot;, END)

app = workflow.compile()

st.title(&quot;🔄 Corrective RAG Agent&quot;)

st.text(&quot;A possible query: What are the experiment results and ablation studies in this research paper?&quot;)

# User input
user_question = st.text_input(&quot;Please enter your question:&quot;)

if user_question:
    inputs = {
        &quot;keys&quot;: {
            &quot;question&quot;: user_question,
        }
    }

    for output in app.stream(inputs):
        for key, value in output.items():
            with st.expander(f&quot;Step &#x27;{key}&#x27;:&quot;):
                st.text(pprint.pformat(format_state(value[&quot;keys&quot;]), indent=2, width=80))

    final_generation = value[&#x27;keys&#x27;].get(&#x27;generation&#x27;, &#x27;No final generation produced.&#x27;)
    st.subheader(&quot;Final Generation:&quot;)
    st.write(final_generation)
```