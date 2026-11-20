
# Code

> 来源：`[https://qqs7y1hozd1.feishu.cn/docx/MkLXd3H29oGjYFxKSGRcdBk4n5B](https://qqs7y1hozd1.feishu.cn/docx/MkLXd3H29oGjYFxKSGRcdBk4n5B)`

```python
import os
from uuid import uuid4
from agno.agent import Agent
from agno.models.openai import OpenAIChat
from agno.tools.eleven_labs import ElevenLabsTools
from agno.tools.firecrawl import FirecrawlTools
from agno.agent import Agent, RunResponse
from agno.utils.audio import write_audio_to_file
from agno.utils.log import logger
import streamlit as st

# Streamlit Page Setup
st.set_page_config(page_title=&quot;📰 ➡️ 🎙️ Blog to Podcast Agent&quot;, page_icon=&quot;🎙️&quot;)
st.title(&quot;📰 ➡️ 🎙️ Blog to Podcast Agent&quot;)

# Sidebar: API Keys
st.sidebar.header(&quot;🔑 API Keys&quot;)

openai_api_key = st.sidebar.text_input(&quot;OpenAI API Key&quot;, type=&quot;password&quot;)
elevenlabs_api_key = st.sidebar.text_input(&quot;ElevenLabs API Key&quot;, type=&quot;password&quot;)
firecrawl_api_key = st.sidebar.text_input(&quot;Firecrawl API Key&quot;, type=&quot;password&quot;)

# Check if all keys are provided
keys_provided = all([openai_api_key, elevenlabs_api_key, firecrawl_api_key])

# Input: Blog URL
url = st.text_input(&quot;Enter the Blog URL:&quot;, &quot;&quot;)

# Button: Generate Podcast
generate_button = st.button(&quot;🎙️ Generate Podcast&quot;, disabled=not keys_provided)

if not keys_provided:
    st.warning(&quot;Please enter all required API keys to enable podcast generation.&quot;)

if generate_button:
    if url.strip() == &quot;&quot;:
        st.warning(&quot;Please enter a blog URL first.&quot;)
    else:
        # Set API keys as environment variables for Agno and Tools
        os.environ[&quot;OPENAI_API_KEY&quot;] = openai_api_key
        os.environ[&quot;ELEVEN_LABS_API_KEY&quot;] = elevenlabs_api_key
        os.environ[&quot;FIRECRAWL_API_KEY&quot;] = firecrawl_api_key

        with st.spinner(&quot;Processing... Scraping blog, summarizing and generating podcast 🎶&quot;):
            try:
                blog_to_podcast_agent = Agent(
                    name=&quot;Blog to Podcast Agent&quot;,
                    agent_id=&quot;blog_to_podcast_agent&quot;,
                    model=OpenAIChat(id=&quot;gpt-4o&quot;),
                    tools=[
                        ElevenLabsTools(
                            voice_id=&quot;JBFqnCBsd6RMkjVDRZzb&quot;,
                            model_id=&quot;eleven_multilingual_v2&quot;,
                            target_directory=&quot;audio_generations&quot;,
                        ),
                        FirecrawlTools(),
                    ],
                    description=&quot;You are an AI agent that can generate audio using the ElevenLabs API.&quot;,
                    instructions=[
                        &quot;When the user provides a blog URL:&quot;,
                        &quot;1. Use FirecrawlTools to scrape the blog content&quot;,
                        &quot;2. Create a concise summary of the blog content that is NO MORE than 2000 characters long&quot;,
                        &quot;3. The summary should capture the main points while being engaging and conversational&quot;,
                        &quot;4. Use the ElevenLabsTools to convert the summary to audio&quot;,
                        &quot;Ensure the summary is within the 2000 character limit to avoid ElevenLabs API limits&quot;,
                    ],
                    markdown=True,
                    debug_mode=True,
                )

                podcast: RunResponse = blog_to_podcast_agent.run(
                    f&quot;Convert the blog content to a podcast: {url}&quot;
                )

                save_dir = &quot;audio_generations&quot;
                os.makedirs(save_dir, exist_ok=True)

                if podcast.audio and len(podcast.audio) &gt; 0:
                    filename = f&quot;{save_dir}/podcast_{uuid4()}.wav&quot;
                    write_audio_to_file(
                        audio=podcast.audio[0].base64_audio,
                        filename=filename
                    )

                    st.success(&quot;Podcast generated successfully! 🎧&quot;)
                    audio_bytes = open(filename, &quot;rb&quot;).read()
                    st.audio(audio_bytes, format=&quot;audio/wav&quot;)

                    st.download_button(
                        label=&quot;Download Podcast&quot;,
                        data=audio_bytes,
                        file_name=&quot;generated_podcast.wav&quot;,
                        mime=&quot;audio/wav&quot;
                    )
                else:
                    st.error(&quot;No audio was generated. Please try again.&quot;)

            except Exception as e:
                st.error(f&quot;An error occurred: {e}&quot;)
                logger.error(f&quot;Streamlit app error: {e}&quot;)
```
