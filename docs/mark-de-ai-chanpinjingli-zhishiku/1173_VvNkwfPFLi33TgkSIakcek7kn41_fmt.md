
# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/GW6Cd1KNyoJKahxUJ40cnep4nGd](https://qqs7y1hozd1.feishu.cn/docx/GW6Cd1KNyoJKahxUJ40cnep4nGd)

```python
import streamlit as st
from agno.agent import Agent
from agno.models.google import Gemini
import tempfile
import os

def main():
    # Set up the reasoning agent
    agent = Agent(
        model=Gemini(id="gemini-2.0-flash-thinking-exp-1219"), 
        markdown=True
    )

    # Streamlit app title
    st.title("Multimodal Reasoning AI Agent 🧠")

    # Instruction
    st.write(
        "Upload an image and provide a reasoning-based task for the AI Agent. "
        "The AI Agent will analyze the image and respond based on your input."
    )

    # File uploader for image
    uploaded_file = st.file_uploader("Upload Image", type=["jpg", "jpeg", "png"])

    if uploaded_file is not None:
        try:
            # Save uploaded file to temporary file
            with tempfile.NamedTemporaryFile(delete=False, suffix='.jpg') as tmp_file:
                tmp_file.write(uploaded_file.getvalue())
                temp_path = tmp_file.name

            # Display the uploaded image
            st.image(uploaded_file, caption="Uploaded Image", use_container_width=True)

            # Input for dynamic task
            task_input = st.text_area(
                "Enter your task/question for the AI Agent:"
            )

            # Button to process the image and task
            if st.button("Analyze Image") and task_input:
                with st.spinner("AI is = agent.run(task_input, images=[temp_path])
                        
                        # Display the response from the model
                        st.markdown("### AI Response:")
                        st.markdown(response.content)
                    except Exception as e:
                        st.error(f"An error occurred during analysis: {str(e)}")
                    finally:
                        # Clean up temp file
                        if os.path.exists(temp_path):
                            os.unlink(temp_path)

        except Exception as e:
            st.error(f"An error occurred while processing the image: {str(e)}")

if __name__ == "__main__":
    main()
```

```python
import streamlit as st
from agno.agent import Agent
from agno.models.google import Gemini
from agno.media import Video
import time
from pathlib import Path
import tempfile

st.set_page_config(
    page_title="Multimodal AI Agent",
    page_icon="🧬",
    layout="wide"
)

st.title("Multimodal AI Agent 🧬")

# Get Gemini API key from user
gemini_api_key = st.text_input("Enter your Gemini API Key", type="password")

# Initialize single agent with both capabilities
@st.cache_resource
def initialize_agent(api_key):
    return Agent(
        name="Multimodal Analyst",
        model=Gemini(id="gemini-2.0-flash", api_key=api_key),
        markdown=True,
    )

if gemini_api_key:
    agent = initialize_agent(gemini_api_key)

    # File uploader
    uploaded_file = st.file_uploader("Upload a video file", type=['mp4', 'mov', 'avi'])

    if uploaded_file:
        with tempfile.NamedTemporaryFile(delete=False, suffix='.mp4') as tmp_file:
            tmp_file.write(uploaded_file.read())
            video_path = tmp_file.name
        
        st.video(video_path)
        
        user_prompt = st.text_area(
            "What would you like to know?",
            placeholder="Ask any question related to the video - the AI Agent will analyze it and search the web if needed",
            help="You can ask questions about the video content and get relevant information from the web"
        )
        
        if st.button("Analyze & Research"):
            if not user_prompt:
                st.warning("Please enter your question.")
            else:
                try:
                    with st.spinner("Processing video and researching..."):
                        video = Video(filepath=video_path)
                        
                        prompt = """
                        First analyze this video and then answer the following question using both 
                        the video analysis and web research: {user_prompt}
                        
                        Provide a comprehensive response focusing on practical, actionable information.
                        """
                        
                        result = agent.run(prompt, videos=[video])
                        
                    st.subheader("Result")
                    st.markdown(result.content)

                except Exception as e:
                    st.error(f"An error occurred: {str(e)}")
                finally:
                    Path(video_path).unlink(missing_ok=True)
    else:
        st.info("Please upload a video to begin analysis.")
else:
    st.warning("Please enter your Gemini API key to continue.")

st.markdown("""
    <style>
    .stTextArea textarea {
        height: 100px;
    }
    </style>
    """, unsafe_allow_html=True)
```
