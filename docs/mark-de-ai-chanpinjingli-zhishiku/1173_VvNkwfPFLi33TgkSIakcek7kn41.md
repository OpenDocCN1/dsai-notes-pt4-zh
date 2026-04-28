# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/GW6Cd1KNyoJKahxUJ40cnep4nGd](https://qqs7y1hozd1.feishu.cn/docx/GW6Cd1KNyoJKahxUJ40cnep4nGd)

```
import streamlit as st
from agno.agent import Agent
from agno.models.google import Gemini
import tempfile
import os

def main():
    # Set up the reasoning agent
    agent = Agent(
        model=Gemini(id=&quot;gemini-2.0-flash-thinking-exp-1219&quot;), 
        markdown=True
    )

    # Streamlit app title
    st.title(&quot;Multimodal Reasoning AI Agent 🧠&quot;)

    # Instruction
    st.write(
        &quot;Upload an image and provide a reasoning-based task for the AI Agent. &quot;
        &quot;The AI Agent will analyze the image and respond based on your input.&quot;
    )

    # File uploader for image
    uploaded_file = st.file_uploader(&quot;Upload Image&quot;, type=[&quot;jpg&quot;, &quot;jpeg&quot;, &quot;png&quot;])

    if uploaded_file is not None:
        try:
            # Save uploaded file to temporary file
            with tempfile.NamedTemporaryFile(delete=False, suffix=&#x27;.jpg&#x27;) as tmp_file:
                tmp_file.write(uploaded_file.getvalue())
                temp_path = tmp_file.name

            # Display the uploaded image
            st.image(uploaded_file, caption=&quot;Uploaded Image&quot;, use_container_width=True)

            # Input for dynamic task
            task_input = st.text_area(
                &quot;Enter your task/question for the AI Agent:&quot;
            )

            # Button to process the image and task
            if st.button(&quot;Analyze Image&quot;) and task_input:
                with st.spinner(&quot;AI is thinking... 🤖&quot;):
                    try:
                        # Call the agent with the dynamic task and image path
                        response = agent.run(task_input, images=[temp_path])
                        
                        # Display the response from the model
                        st.markdown(&quot;### AI Response:&quot;)
                        st.markdown(response.content)
                    except Exception as e:
                        st.error(f&quot;An error occurred during analysis: {str(e)}&quot;)
                    finally:
                        # Clean up temp file
                        if os.path.exists(temp_path):
                            os.unlink(temp_path)

        except Exception as e:
            st.error(f&quot;An error occurred while processing the image: {str(e)}&quot;)

if __name__ == &quot;__main__&quot;:
    main()
```

```
import streamlit as st
from agno.agent import Agent
from agno.models.google import Gemini
from agno.media import Video
import time
from pathlib import Path
import tempfile

st.set_page_config(
    page_title=&quot;Multimodal AI Agent&quot;,
    page_icon=&quot;🧬&quot;,
    layout=&quot;wide&quot;
)

st.title(&quot;Multimodal AI Agent 🧬&quot;)

# Get Gemini API key from user
gemini_api_key = st.text_input(&quot;Enter your Gemini API Key&quot;, type=&quot;password&quot;)

# Initialize single agent with both capabilities
@st.cache_resource
def initialize_agent(api_key):
    return Agent(
        name=&quot;Multimodal Analyst&quot;,
        model=Gemini(id=&quot;gemini-2.0-flash&quot;, api_key=api_key),
        markdown=True,
    )

if gemini_api_key:
    agent = initialize_agent(gemini_api_key)

    # File uploader
    uploaded_file = st.file_uploader(&quot;Upload a video file&quot;, type=[&#x27;mp4&#x27;, &#x27;mov&#x27;, &#x27;avi&#x27;])

    if uploaded_file:
        with tempfile.NamedTemporaryFile(delete=False, suffix=&#x27;.mp4&#x27;) as tmp_file:
            tmp_file.write(uploaded_file.read())
            video_path = tmp_file.name
        
        st.video(video_path)
        
        user_prompt = st.text_area(
            &quot;What would you like to know?&quot;,
            placeholder=&quot;Ask any question related to the video - the AI Agent will analyze it and search the web if needed&quot;,
            help=&quot;You can ask questions about the video content and get relevant information from the web&quot;
        )
        
        if st.button(&quot;Analyze &amp; Research&quot;):
            if not user_prompt:
                st.warning(&quot;Please enter your question.&quot;)
            else:
                try:
                    with st.spinner(&quot;Processing video and researching...&quot;):
                        video = Video(filepath=video_path)
                        
                        prompt = f&quot;&quot;&quot;
                        First analyze this video and then answer the following question using both 
                        the video analysis and web research: {user_prompt}
                        
                        Provide a comprehensive response focusing on practical, actionable information.
                        &quot;&quot;&quot;
                        
                        result = agent.run(prompt, videos=[video])
                        
                    st.subheader(&quot;Result&quot;)
                    st.markdown(result.content)

                except Exception as e:
                    st.error(f&quot;An error occurred: {str(e)}&quot;)
                finally:
                    Path(video_path).unlink(missing_ok=True)
    else:
        st.info(&quot;Please upload a video to begin analysis.&quot;)
else:
    st.warning(&quot;Please enter your Gemini API key to continue.&quot;)

st.markdown(&quot;&quot;&quot;
    &lt;style&gt;
    .stTextArea textarea {
        height: 100px;
    }
    &lt;/style&gt;
    &quot;&quot;&quot;, unsafe_allow_html=True)
```