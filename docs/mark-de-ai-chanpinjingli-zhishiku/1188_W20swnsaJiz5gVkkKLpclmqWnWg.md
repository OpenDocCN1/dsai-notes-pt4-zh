# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/FLN1dd4RWomvhkxKPWRcwlHunfg](https://qqs7y1hozd1.feishu.cn/docx/FLN1dd4RWomvhkxKPWRcwlHunfg)

```
from typing import List, Dict, Optional
from pathlib import Path
import os
from firecrawl import FirecrawlApp
from qdrant_client import QdrantClient
from qdrant_client.http import models
from qdrant_client.http.models import Distance, VectorParams
from fastembed import TextEmbedding
from agents import Agent, Runner
from openai import AsyncOpenAI
import tempfile
import uuid
from datetime import datetime
import time
import streamlit as st
from dotenv import load_dotenv
import asyncio

load_dotenv()

def init_session_state():
    defaults = {
        &quot;initialized&quot;: False,
        &quot;qdrant_url&quot;: &quot;&quot;,
        &quot;qdrant_api_key&quot;: &quot;&quot;,
        &quot;firecrawl_api_key&quot;: &quot;&quot;,
        &quot;openai_api_key&quot;: &quot;&quot;,
        &quot;doc_url&quot;: &quot;&quot;,
        &quot;setup_complete&quot;: False,
        &quot;client&quot;: None,
        &quot;embedding_model&quot;: None,
        &quot;processor_agent&quot;: None,
        &quot;tts_agent&quot;: None,
        &quot;selected_voice&quot;: &quot;coral&quot;
    }
    
    for key, value in defaults.items():
        if key not in st.session_state:
            st.session_state[key] = value

def sidebar_config():
    with st.sidebar:
        st.title(&quot;🔑 Configuration&quot;)
        st.markdown(&quot;---&quot;)
        
        st.session_state.qdrant_url = st.text_input(
            &quot;Qdrant URL&quot;,
            value=st.session_state.qdrant_url,
            type=&quot;password&quot;
        )
        st.session_state.qdrant_api_key = st.text_input(
            &quot;Qdrant API Key&quot;,
            value=st.session_state.qdrant_api_key,
            type=&quot;password&quot;
        )
        st.session_state.firecrawl_api_key = st.text_input(
            &quot;Firecrawl API Key&quot;,
            value=st.session_state.firecrawl_api_key,
            type=&quot;password&quot;
        )
        st.session_state.openai_api_key = st.text_input(
            &quot;OpenAI API Key&quot;,
            value=st.session_state.openai_api_key,
            type=&quot;password&quot;
        )
        
        st.markdown(&quot;---&quot;)
        st.session_state.doc_url = st.text_input(
            &quot;Documentation URL&quot;,
            value=st.session_state.doc_url,
            placeholder=&quot;https://docs.example.com&quot;
        )
        
        st.markdown(&quot;---&quot;)
        st.markdown(&quot;### 🎤 Voice Settings&quot;)
        voices = [&quot;alloy&quot;, &quot;ash&quot;, &quot;ballad&quot;, &quot;coral&quot;, &quot;echo&quot;, &quot;fable&quot;, &quot;onyx&quot;, &quot;nova&quot;, &quot;sage&quot;, &quot;shimmer&quot;, &quot;verse&quot;]
        st.session_state.selected_voice = st.selectbox(
            &quot;Select Voice&quot;,
            options=voices,
            index=voices.index(st.session_state.selected_voice),
            help=&quot;Choose the voice for the audio response&quot;
        )
        
        if st.button(&quot;Initialize System&quot;, type=&quot;primary&quot;):
            if all([
                st.session_state.qdrant_url,
                st.session_state.qdrant_api_key,
                st.session_state.firecrawl_api_key,
                st.session_state.openai_api_key,
                st.session_state.doc_url
            ]):
                progress_placeholder = st.empty()
                with progress_placeholder.container():
                    try:
                        st.markdown(&quot;🔄 Setting up Qdrant connection...&quot;)
                        client, embedding_model = setup_qdrant_collection(
                            st.session_state.qdrant_url,
                            st.session_state.qdrant_api_key
                        )
                        st.session_state.client = client
                        st.session_state.embedding_model = embedding_model
                        st.markdown(&quot;✅ Qdrant setup complete!&quot;)
                        
                        st.markdown(&quot;🔄 Crawling documentation pages...&quot;)
                        pages = crawl_documentation(
                            st.session_state.firecrawl_api_key,
                            st.session_state.doc_url
                        )
                        st.markdown(f&quot;✅ Crawled {len(pages)} documentation pages!&quot;)
                        
                        store_embeddings(
                            client,
                            embedding_model,
                            pages,
                            &quot;docs_embeddings&quot;
                        )
                        
                        processor_agent, tts_agent = setup_agents(
                            st.session_state.openai_api_key
                        )
                        st.session_state.processor_agent = processor_agent
                        st.session_state.tts_agent = tts_agent
                        
                        st.session_state.setup_complete = True
                        st.success(&quot;✅ System initialized successfully!&quot;)
                        
                    except Exception as e:
                        st.error(f&quot;Error during setup: {str(e)}&quot;)
            else:
                st.error(&quot;Please fill in all the required fields!&quot;)

def setup_qdrant_collection(qdrant_url: str, qdrant_api_key: str, collection_name: str = &quot;docs_embeddings&quot;):
    client = QdrantClient(url=qdrant_url, api_key=qdrant_api_key)
    embedding_model = TextEmbedding()
    test_embedding = list(embedding_model.embed([&quot;test&quot;]))[0]
    embedding_dim = len(test_embedding)
    
    try:
        client.create_collection(
            collection_name=collection_name,
            vectors_config=VectorParams(size=embedding_dim, distance=Distance.COSINE)
        )
    except Exception as e:
        if &quot;already exists&quot; not in str(e):
            raise e
    
    return client, embedding_model

def crawl_documentation(firecrawl_api_key: str, url: str, output_dir: Optional[str] = None):
    firecrawl = FirecrawlApp(api_key=firecrawl_api_key)
    pages = []
    
    if output_dir:
        os.makedirs(output_dir, exist_ok=True)
    
    response = firecrawl.crawl_url(
        url,
        params={
            &#x27;limit&#x27;: 5,
            &#x27;scrapeOptions&#x27;: {
                &#x27;formats&#x27;: [&#x27;markdown&#x27;, &#x27;html&#x27;]
            }
        }
    )
    
    while True:
        for page in response.get(&#x27;data&#x27;, []):
            content = page.get(&#x27;markdown&#x27;) or page.get(&#x27;html&#x27;, &#x27;&#x27;)
            metadata = page.get(&#x27;metadata&#x27;, {})
            source_url = metadata.get(&#x27;sourceURL&#x27;, &#x27;&#x27;)
            
            if output_dir and content:
                filename = f&quot;{uuid.uuid4()}.md&quot;
                filepath = os.path.join(output_dir, filename)
                with open(filepath, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as f:
                    f.write(content)
            
            pages.append({
                &quot;content&quot;: content,
                &quot;url&quot;: source_url,
                &quot;metadata&quot;: {
                    &quot;title&quot;: metadata.get(&#x27;title&#x27;, &#x27;&#x27;),
                    &quot;description&quot;: metadata.get(&#x27;description&#x27;, &#x27;&#x27;),
                    &quot;language&quot;: metadata.get(&#x27;language&#x27;, &#x27;en&#x27;),
                    &quot;crawl_date&quot;: datetime.now().isoformat()
                }
            })
        
        next_url = response.get(&#x27;next&#x27;)
        if not next_url:
            break
            
        response = firecrawl.get(next_url)
        time.sleep(1)
    
    return pages

def store_embeddings(client: QdrantClient, embedding_model: TextEmbedding, pages: List[Dict], collection_name: str):
    for page in pages:
        embedding = list(embedding_model.embed([page[&quot;content&quot;]]))[0]
        client.upsert(
            collection_name=collection_name,
            points=[
                models.PointStruct(
                    id=str(uuid.uuid4()),
                    vector=embedding.tolist(),
                    payload={
                        &quot;content&quot;: page[&quot;content&quot;],
                        &quot;url&quot;: page[&quot;url&quot;],
                        **page[&quot;metadata&quot;]
                    }
                )
            ]
        )

def setup_agents(openai_api_key: str):
    os.environ[&quot;OPENAI_API_KEY&quot;] = openai_api_key
    
    processor_agent = Agent(
        name=&quot;Documentation Processor&quot;,
        instructions=&quot;&quot;&quot;You are a helpful documentation assistant. Your task is to:
        1. Analyze the provided documentation content
        2. Answer the user&#x27;s question clearly and concisely
        3. Include relevant examples when available
        4. Cite the source URLs when referencing specific content
        5. Keep responses natural and conversational
        6. Format your response in a way that&#x27;s easy to speak out loud&quot;&quot;&quot;,
        model=&quot;gpt-4o&quot;
    )

    tts_agent = Agent(
        name=&quot;Text-to-Speech Agent&quot;,
        instructions=&quot;&quot;&quot;You are a text-to-speech agent. Your task is to:
        1. Convert the processed documentation response into natural speech
        2. Maintain proper pacing and emphasis
        3. Handle technical terms clearly
        4. Keep the tone professional but friendly
        5. Use appropriate pauses for better comprehension
        6. Ensure the speech is clear and well-articulated&quot;&quot;&quot;,
        model=&quot;gpt-4o-mini-tts&quot;
    )
    
    return processor_agent, tts_agent

async def process_query(
    query: str,
    client: QdrantClient,
    embedding_model: TextEmbedding,
    processor_agent: Agent,
    tts_agent: Agent,
    collection_name: str,
    openai_api_key: str
):
    try:
        query_embedding = list(embedding_model.embed([query]))[0]
        search_response = client.query_points(
            collection_name=collection_name,
            query=query_embedding.tolist(),
            limit=3,
            with_payload=True
        )
        
        search_results = search_response.points if hasattr(search_response, &#x27;points&#x27;) else []
        
        if not search_results:
            raise Exception(&quot;No relevant documents found in the vector database&quot;)
        
        context = &quot;Based on the following documentation:\n\n&quot;
        for result in search_results:
            payload = result.payload
            if not payload:
                continue
            url = payload.get(&#x27;url&#x27;, &#x27;Unknown URL&#x27;)
            content = payload.get(&#x27;content&#x27;, &#x27;&#x27;)
            context += f&quot;From {url}:\n{content}\n\n&quot;
        
        context += f&quot;\nUser Question: {query}\n\n&quot;
        context += &quot;Please provide a clear, concise answer that can be easily spoken out loud.&quot;
        
        processor_result = await Runner.run(processor_agent, context)
        processor_response = processor_result.final_output
        
        tts_result = await Runner.run(tts_agent, processor_response)
        tts_response = tts_result.final_output
        
        async_openai = AsyncOpenAI(api_key=openai_api_key)
        audio_response = await async_openai.audio.speech.create(
            model=&quot;gpt-4o-mini-tts&quot;,
            voice=st.session_state.selected_voice,
            input=processor_response,
            instructions=tts_response,
            response_format=&quot;mp3&quot;
        )
        
        temp_dir = tempfile.gettempdir()
        audio_path = os.path.join(temp_dir, f&quot;response_{uuid.uuid4()}.mp3&quot;)
        
        with open(audio_path, &quot;wb&quot;) as f:
            f.write(audio_response.content)
                
        return {
            &quot;status&quot;: &quot;success&quot;,
            &quot;text_response&quot;: processor_response,
            &quot;tts_instructions&quot;: tts_response,
            &quot;audio_path&quot;: audio_path,
            &quot;sources&quot;: [r.payload.get(&quot;url&quot;, &quot;Unknown URL&quot;) for r in search_results if r.payload],
            &quot;query_details&quot;: {
                &quot;vector_size&quot;: len(query_embedding),
                &quot;results_found&quot;: len(search_results),
                &quot;collection_name&quot;: collection_name
            }
        }
    
    except Exception as e:
        return {
            &quot;status&quot;: &quot;error&quot;,
            &quot;error&quot;: str(e),
            &quot;query&quot;: query
        }

def run_streamlit():
    st.set_page_config(
        page_title=&quot;Customer Support Voice Agent&quot;,
        page_icon=&quot;🎙️&quot;,
        layout=&quot;wide&quot;
    )
    
    init_session_state()
    sidebar_config()
    
    st.title(&quot;🎙️ Customer Support Voice Agent&quot;)
    st.markdown(&quot;&quot;&quot;
    Get OpenAI SDK voice-powered answers to your documentation questions! Simply:
    1. Configure your API keys in the sidebar
    2. Enter the documentation URL you want to learn about or have questions about
    3. Ask your question below and get both text and voice responses
    &quot;&quot;&quot;)
    
    query = st.text_input(
        &quot;What would you like to know about the documentation?&quot;,
        placeholder=&quot;e.g., How do I authenticate API requests?&quot;,
        disabled=not st.session_state.setup_complete
    )
    
    if query and st.session_state.setup_complete:
        with st.status(&quot;Processing your query...&quot;, expanded=True) as status:
            try:
                st.markdown(&quot;🔄 Searching documentation and generating response...&quot;)
                result = asyncio.run(process_query(
                    query,
                    st.session_state.client,
                    st.session_state.embedding_model,
                    st.session_state.processor_agent,
                    st.session_state.tts_agent,
                    &quot;docs_embeddings&quot;,
                    st.session_state.openai_api_key
                ))
                
                if result[&quot;status&quot;] == &quot;success&quot;:
                    status.update(label=&quot;✅ Query processed!&quot;, state=&quot;complete&quot;)
                    
                    st.markdown(&quot;### Response:&quot;)
                    st.write(result[&quot;text_response&quot;])
                    
                    if &quot;audio_path&quot; in result:
                        st.markdown(f&quot;### 🔊 Audio Response (Voice: {st.session_state.selected_voice})&quot;)
                        st.audio(result[&quot;audio_path&quot;], format=&quot;audio/mp3&quot;, start_time=0)
                        
                        with open(result[&quot;audio_path&quot;], &quot;rb&quot;) as audio_file:
                            audio_bytes = audio_file.read()
                            st.download_button(
                                label=&quot;📥 Download Audio Response&quot;,
                                data=audio_bytes,
                                file_name=f&quot;voice_response_{st.session_state.selected_voice}.mp3&quot;,
                                mime=&quot;audio/mp3&quot;
                            )
                    
                    st.markdown(&quot;### Sources:&quot;)
                    for source in result[&quot;sources&quot;]:
                        st.markdown(f&quot;- {source}&quot;)
                else:
                    status.update(label=&quot;❌ Error processing query&quot;, state=&quot;error&quot;)
                    st.error(f&quot;Error: {result.get(&#x27;error&#x27;, &#x27;Unknown error occurred&#x27;)}&quot;)
                    
            except Exception as e:
                status.update(label=&quot;❌ Error processing query&quot;, state=&quot;error&quot;)
                st.error(f&quot;Error processing query: {str(e)}&quot;)
    
    elif not st.session_state.setup_complete:
        st.info(&quot;👈 Please configure the system using the sidebar first!&quot;)

if __name__ == &quot;__main__&quot;:
    run_streamlit()
```