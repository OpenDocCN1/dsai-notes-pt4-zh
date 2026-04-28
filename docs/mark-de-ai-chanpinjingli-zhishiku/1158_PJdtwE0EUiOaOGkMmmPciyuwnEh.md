# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/QpJHdF253omCXjxo9mjcRwDInTe](https://qqs7y1hozd1.feishu.cn/docx/QpJHdF253omCXjxo9mjcRwDInTe)

```
import os
from uuid import uuid4
import requests
from agno.agent import Agent, RunResponse
from agno.models.openai import OpenAIChat
from agno.tools.models_labs import FileType, ModelsLabTools
from agno.utils.log import logger
import streamlit as st

# Sidebar: User enters the API keys
st.sidebar.title(&quot;API Key Configuration&quot;)

openai_api_key = st.sidebar.text_input(&quot;Enter your OpenAI API Key&quot;, type=&quot;password&quot;)
models_lab_api_key = st.sidebar.text_input(&quot;Enter your ModelsLab API Key&quot;, type=&quot;password&quot;)

# Streamlit App UI
st.title(&quot;🎶 ModelsLab Music Generator&quot;)
prompt = st.text_area(&quot;Enter a music generation prompt:&quot;, &quot;Generate a 30 second classical music piece&quot;, height=100)

# Initialize agent only if both API keys are provided
if openai_api_key and models_lab_api_key:
    agent = Agent(
        name=&quot;ModelsLab Music Agent&quot;,
        agent_id=&quot;ml_music_agent&quot;,
        model=OpenAIChat(id=&quot;gpt-4o&quot;, api_key=openai_api_key),
        show_tool_calls=True,
        tools=[ModelsLabTools(api_key=models_lab_api_key, wait_for_completion=True, file_type=FileType.MP3)],
        description=&quot;You are an AI agent that can generate music using the ModelsLabs API.&quot;,
        instructions=[
            &quot;When generating music, use the `generate_media` tool with detailed prompts that specify:&quot;,
            &quot;- The genre and style of music (e.g., classical, jazz, electronic)&quot;,
            &quot;- The instruments and sounds to include&quot;,
            &quot;- The tempo, mood and emotional qualities&quot;,
            &quot;- The structure (intro, verses, chorus, bridge, etc.)&quot;,
            &quot;Create rich, descriptive prompts that capture the desired musical elements.&quot;,
            &quot;Focus on generating high-quality, complete instrumental pieces.&quot;,
        ],
        markdown=True,
        debug_mode=True,
    )

    if st.button(&quot;Generate Music&quot;):
        if prompt.strip() == &quot;&quot;:
            st.warning(&quot;Please enter a prompt first.&quot;)
        else:
            with st.spinner(&quot;Generating music... 🎵&quot;):
                try:
                    music: RunResponse = agent.run(prompt)

                    if music.audio and len(music.audio) &gt; 0:
                        save_dir = &quot;audio_generations&quot;
                        os.makedirs(save_dir, exist_ok=True)

                        url = music.audio[0].url
                        response = requests.get(url)

                        # 🛡️ Validate response
                        if not response.ok:
                            st.error(f&quot;Failed to download audio. Status code: {response.status_code}&quot;)
                            st.stop()

                        content_type = response.headers.get(&quot;Content-Type&quot;, &quot;&quot;)
                        if &quot;audio&quot; not in content_type:
                            st.error(f&quot;Invalid file type returned: {content_type}&quot;)
                            st.write(&quot;🔍 Debug: Downloaded content was not an audio file.&quot;)
                            st.write(&quot;🔗 URL:&quot;, url)
                            st.stop()

                        # ✅ Save audio
                        filename = f&quot;{save_dir}/music_{uuid4()}.mp3&quot;
                        with open(filename, &quot;wb&quot;) as f:
                            f.write(response.content)

                        # 🎧 Play audio
                        st.success(&quot;Music generated successfully! 🎶&quot;)
                        audio_bytes = open(filename, &quot;rb&quot;).read()
                        st.audio(audio_bytes, format=&quot;audio/mp3&quot;)

                        st.download_button(
                            label=&quot;Download Music&quot;,
                            data=audio_bytes,
                            file_name=&quot;generated_music.mp3&quot;,
                            mime=&quot;audio/mp3&quot;
                        )
                    else:
                        st.error(&quot;No audio generated. Please try again.&quot;)

                except Exception as e:
                    st.error(f&quot;An error occurred: {e}&quot;)
                    logger.error(f&quot;Streamlit app error: {e}&quot;)

else:
    st.sidebar.warning(&quot;Please enter both the OpenAI and ModelsLab API keys to use the app.&quot;)
```