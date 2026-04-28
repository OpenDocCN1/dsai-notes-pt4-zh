# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/BxDPdRqBtoLiASxs23FcGnKXnne](https://qqs7y1hozd1.feishu.cn/docx/BxDPdRqBtoLiASxs23FcGnKXnne)

```
from typing import List, Dict, Optional, Tuple
import os
import tempfile
from datetime import datetime
import uuid
import asyncio

import streamlit as st
from dotenv import load_dotenv
from qdrant_client import QdrantClient
from qdrant_client.http import models
from qdrant_client.http.models import Distance, VectorParams
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.document_loaders import PyPDFLoader
from fastembed import TextEmbedding
from openai import AsyncOpenAI
from openai.helpers import LocalAudioPlayer
from agents import Agent, Runner

load_dotenv()

# Constants
COLLECTION_NAME = &quot;voice-rag-agent&quot;

def init_session_state() -&gt; None:
    &quot;&quot;&quot;Initialize Streamlit session state with default values.&quot;&quot;&quot;
    defaults = {
        &quot;initialized&quot;: False,
        &quot;qdrant_url&quot;: &quot;&quot;,
        &quot;qdrant_api_key&quot;: &quot;&quot;,
        &quot;openai_api_key&quot;: &quot;&quot;,
        &quot;setup_complete&quot;: False,
        &quot;client&quot;: None,
        &quot;embedding_model&quot;: None,
        &quot;processor_agent&quot;: None,
        &quot;tts_agent&quot;: None,
        &quot;selected_voice&quot;: &quot;coral&quot;,
        &quot;processed_documents&quot;: []
    }
    
    for key, value in defaults.items():
        if key not in st.session_state:
            st.session_state[key] = value

def setup_sidebar() -&gt; None:
    &quot;&quot;&quot;Configure sidebar with API settings and voice options.&quot;&quot;&quot;
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
        st.session_state.openai_api_key = st.text_input(
            &quot;OpenAI API Key&quot;,
            value=st.session_state.openai_api_key,
            type=&quot;password&quot;
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

def setup_qdrant() -&gt; Tuple[QdrantClient, TextEmbedding]:
    &quot;&quot;&quot;Initialize Qdrant client and embedding model.&quot;&quot;&quot;
    if not all([st.session_state.qdrant_url, st.session_state.qdrant_api_key]):
        raise ValueError(&quot;Qdrant credentials not provided&quot;)
    
    client = QdrantClient(
        url=st.session_state.qdrant_url,
        api_key=st.session_state.qdrant_api_key
    )
    
    embedding_model = TextEmbedding()
    test_embedding = list(embedding_model.embed([&quot;test&quot;]))[0]
    embedding_dim = len(test_embedding)
    
    try:
        client.create_collection(
            collection_name=COLLECTION_NAME,
            vectors_config=VectorParams(
                size=embedding_dim,
                distance=Distance.COSINE
            )
        )
    except Exception as e:
        if &quot;already exists&quot; not in str(e):
            raise e
    
    return client, embedding_model

def process_pdf(file) -&gt; List:
    &quot;&quot;&quot;Process PDF file and split into chunks with metadata.&quot;&quot;&quot;
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

def store_embeddings(
    client: QdrantClient,
    embedding_model: TextEmbedding,
    documents: List,
    collection_name: str
) -&gt; None:
    &quot;&quot;&quot;Store document embeddings in Qdrant.&quot;&quot;&quot;
    for doc in documents:
        embedding = list(embedding_model.embed([doc.page_content]))[0]
        client.upsert(
            collection_name=collection_name,
            points=[
                models.PointStruct(
                    id=str(uuid.uuid4()),
                    vector=embedding.tolist(),
                    payload={
                        &quot;content&quot;: doc.page_content,
                        **doc.metadata
                    }
                )
            ]
        )

def setup_agents(openai_api_key: str) -&gt; Tuple[Agent, Agent]:
    &quot;&quot;&quot;Initialize the processor and TTS agents.&quot;&quot;&quot;
    os.environ[&quot;OPENAI_API_KEY&quot;] = openai_api_key
    
    processor_agent = Agent(
        name=&quot;Documentation Processor&quot;,
        instructions=&quot;&quot;&quot;You are a helpful documentation assistant. Your task is to:
        1. Analyze the provided documentation content
        2. Answer the user&#x27;s question clearly and concisely
        3. Include relevant examples when available
        4. Cite the source files when referencing specific content
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
        model=&quot;gpt-4o&quot;
    )
    
    return processor_agent, tts_agent

async def process_query(
    query: str,
    client: QdrantClient,
    embedding_model: TextEmbedding,
    collection_name: str,
    openai_api_key: str,
    voice: str
) -&gt; Dict:
    &quot;&quot;&quot;Process user query and generate voice response.&quot;&quot;&quot;
    try:
        st.info(&quot;🔄 Step 1: Generating query embedding and searching documents...&quot;)
        # Get query embedding and search
        query_embedding = list(embedding_model.embed([query]))[0]
        st.write(f&quot;Generated embedding of size: {len(query_embedding)}&quot;)
        
        search_response = client.query_points(
            collection_name=collection_name,
            query=query_embedding.tolist(),
            limit=3,
            with_payload=True
        )
        
        search_results = search_response.points if hasattr(search_response, &#x27;points&#x27;) else []
        st.write(f&quot;Found {len(search_results)} relevant documents&quot;)
        
        if not search_results:
            raise Exception(&quot;No relevant documents found in the vector database&quot;)
        
        st.info(&quot;🔄 Step 2: Preparing context from search results...&quot;)
        # Prepare context from search results
        context = &quot;Based on the following documentation:\n\n&quot;
        for i, result in enumerate(search_results, 1):
            payload = result.payload
            if not payload:
                continue
            content = payload.get(&#x27;content&#x27;, &#x27;&#x27;)
            source = payload.get(&#x27;file_name&#x27;, &#x27;Unknown Source&#x27;)
            context += f&quot;From {source}:\n{content}\n\n&quot;
            st.write(f&quot;Document {i} from: {source}&quot;)
        
        context += f&quot;\nUser Question: {query}\n\n&quot;
        context += &quot;Please provide a clear, concise answer that can be easily spoken out loud.&quot;
        
        st.info(&quot;🔄 Step 3: Setting up agents...&quot;)
        # Setup agents if not already done
        if not st.session_state.processor_agent or not st.session_state.tts_agent:
            processor_agent, tts_agent = setup_agents(openai_api_key)
            st.session_state.processor_agent = processor_agent
            st.session_state.tts_agent = tts_agent
            st.write(&quot;Initialized new processor and TTS agents&quot;)
        else:
            st.write(&quot;Using existing agents&quot;)
        
        st.info(&quot;🔄 Step 4: Generating text response...&quot;)
        # Generate text response using processor agent
        processor_result = await Runner.run(st.session_state.processor_agent, context)
        text_response = processor_result.final_output
        st.write(f&quot;Generated text response of length: {len(text_response)}&quot;)
        
        st.info(&quot;🔄 Step 5: Generating voice instructions...&quot;)
        # Generate voice instructions using TTS agent
        tts_result = await Runner.run(st.session_state.tts_agent, text_response)
        voice_instructions = tts_result.final_output
        st.write(f&quot;Generated voice instructions of length: {len(voice_instructions)}&quot;)
        
        st.info(&quot;🔄 Step 6: Generating and playing audio...&quot;)
        # Generate and play audio with streaming
        async_openai = AsyncOpenAI(api_key=openai_api_key)
        
        # First create streaming response
        async with async_openai.audio.speech.with_streaming_response.create(
            model=&quot;gpt-4o-mini-tts&quot;,
            voice=voice,
            input=text_response,
            instructions=voice_instructions,
            response_format=&quot;pcm&quot;,
        ) as stream_response:
            st.write(&quot;Starting audio playback...&quot;)
            # Play audio directly using LocalAudioPlayer
            await LocalAudioPlayer().play(stream_response)
            st.write(&quot;Audio playback complete&quot;)
            
            st.write(&quot;Generating downloadable MP3 version...&quot;)
            # Also save as MP3 for download
            audio_response = await async_openai.audio.speech.create(
                model=&quot;gpt-4o-mini-tts&quot;,
                voice=voice,
                input=text_response,
                instructions=voice_instructions,
                response_format=&quot;mp3&quot;
            )
            
            temp_dir = tempfile.gettempdir()
            audio_path = os.path.join(temp_dir, f&quot;response_{uuid.uuid4()}.mp3&quot;)
            
            with open(audio_path, &quot;wb&quot;) as f:
                f.write(audio_response.content)
            st.write(f&quot;Saved MP3 file to: {audio_path}&quot;)
        
        st.success(&quot;✅ Query processing complete!&quot;)
        return {
            &quot;status&quot;: &quot;success&quot;,
            &quot;text_response&quot;: text_response,
            &quot;voice_instructions&quot;: voice_instructions,
            &quot;audio_path&quot;: audio_path,
            &quot;sources&quot;: [r.payload.get(&#x27;file_name&#x27;, &#x27;Unknown Source&#x27;) for r in search_results if r.payload]
        }
    
    except Exception as e:
        st.error(f&quot;❌ Error during query processing: {str(e)}&quot;)
        return {
            &quot;status&quot;: &quot;error&quot;,
            &quot;error&quot;: str(e),
            &quot;query&quot;: query
        }

def main() -&gt; None:
    &quot;&quot;&quot;Main application function.&quot;&quot;&quot;
    st.set_page_config(
        page_title=&quot;Voice RAG Agent&quot;,
        page_icon=&quot;🎙️&quot;,
        layout=&quot;wide&quot;
    )
    
    init_session_state()
    setup_sidebar()
    
    st.title(&quot;🎙️ Voice RAG Agent&quot;)
    st.info(&quot;Get voice-powered answers to your documentation questions by configuring your API keys and uploading PDF documents. Then, simply ask questions to receive both text and voice responses!&quot;)
    
    # File upload section
    uploaded_file = st.file_uploader(&quot;Upload PDF&quot;, type=[&quot;pdf&quot;])
    
    if uploaded_file:
        file_name = uploaded_file.name
        if file_name not in st.session_state.processed_documents:
            with st.spinner(&#x27;Processing PDF...&#x27;):
                try:
                    # Setup Qdrant if not already done
                    if not st.session_state.client:
                        client, embedding_model = setup_qdrant()
                        st.session_state.client = client
                        st.session_state.embedding_model = embedding_model
                    
                    # Process and store document
                    documents = process_pdf(uploaded_file)
                    if documents:
                        store_embeddings(
                            st.session_state.client,
                            st.session_state.embedding_model,
                            documents,
                            COLLECTION_NAME
                        )
                        st.session_state.processed_documents.append(file_name)
                        st.success(f&quot;✅ Added PDF: {file_name}&quot;)
                        st.session_state.setup_complete = True
                except Exception as e:
                    st.error(f&quot;Error processing document: {str(e)}&quot;)
    
    # Display processed documents
    if st.session_state.processed_documents:
        st.sidebar.header(&quot;📚 Processed Documents&quot;)
        for doc in st.session_state.processed_documents:
            st.sidebar.text(f&quot;📄 {doc}&quot;)
    
    # Query interface
    query = st.text_input(
        &quot;What would you like to know about the documentation?&quot;,
        placeholder=&quot;e.g., How do I authenticate API requests?&quot;,
        disabled=not st.session_state.setup_complete
    )
    
    if query and st.session_state.setup_complete:
        with st.status(&quot;Processing your query...&quot;, expanded=True) as status:
            try:
                result = asyncio.run(process_query(
                    query,
                    st.session_state.client,
                    st.session_state.embedding_model,
                    COLLECTION_NAME,
                    st.session_state.openai_api_key,
                    st.session_state.selected_voice
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
        st.info(&quot;👈 Please configure the system and upload documents first!&quot;)

if __name__ == &quot;__main__&quot;:
    main()
```