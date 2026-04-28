# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/YIK6d1Z23o2xDyxcR5jcU9umnNe](https://qqs7y1hozd1.feishu.cn/docx/YIK6d1Z23o2xDyxcR5jcU9umnNe)

```
from agno.agent import Agent
from agno.models.google import Gemini
from agno.media import Image as AgnoImage
from agno.tools.duckduckgo import DuckDuckGoTools
import streamlit as st
from typing import List, Optional
import logging
from pathlib import Path
import tempfile
import os

# Configure logging for errors only
logging.basicConfig(level=logging.ERROR)
logger = logging.getLogger(__name__)

def initialize_agents(api_key: str) -&gt; tuple[Agent, Agent, Agent, Agent]:
    try:
        model = Gemini(id=&quot;gemini-2.0-flash-exp&quot;, api_key=api_key)
        
        therapist_agent = Agent(
            model=model,
            name=&quot;Therapist Agent&quot;,
            instructions=[
                &quot;You are an empathetic therapist that:&quot;,
                &quot;1. Listens with empathy and validates feelings&quot;,
                &quot;2. Uses gentle humor to lighten the mood&quot;,
                &quot;3. Shares relatable breakup experiences&quot;,
                &quot;4. Offers comforting words and encouragement&quot;,
                &quot;5. Analyzes both text and image inputs for emotional context&quot;,
                &quot;Be supportive and understanding in your responses&quot;
            ],
            markdown=True
        )

        closure_agent = Agent(
            model=model,
            name=&quot;Closure Agent&quot;,
            instructions=[
                &quot;You are a closure specialist that:&quot;,
                &quot;1. Creates emotional messages for unsent feelings&quot;,
                &quot;2. Helps express raw, honest emotions&quot;,
                &quot;3. Formats messages clearly with headers&quot;,
                &quot;4. Ensures tone is heartfelt and authentic&quot;,
                &quot;Focus on emotional release and closure&quot;
            ],
            markdown=True
        )

        routine_planner_agent = Agent(
            model=model,
            name=&quot;Routine Planner Agent&quot;,
            instructions=[
                &quot;You are a recovery routine planner that:&quot;,
                &quot;1. Designs 7-day recovery challenges&quot;,
                &quot;2. Includes fun activities and self-care tasks&quot;,
                &quot;3. Suggests social media detox strategies&quot;,
                &quot;4. Creates empowering playlists&quot;,
                &quot;Focus on practical recovery steps&quot;
            ],
            markdown=True
        )

        brutal_honesty_agent = Agent(
            model=model,
            name=&quot;Brutal Honesty Agent&quot;,
            tools=[DuckDuckGoTools()],
            instructions=[
                &quot;You are a direct feedback specialist that:&quot;,
                &quot;1. Gives raw, objective feedback about breakups&quot;,
                &quot;2. Explains relationship failures clearly&quot;,
                &quot;3. Uses blunt, factual language&quot;,
                &quot;4. Provides reasons to move forward&quot;,
                &quot;Focus on honest insights without sugar-coating&quot;
            ],
            markdown=True
        )
        
        return therapist_agent, closure_agent, routine_planner_agent, brutal_honesty_agent
    except Exception as e:
        st.error(f&quot;Error initializing agents: {str(e)}&quot;)
        return None, None, None, None

# Set page config and UI elements
st.set_page_config(
    page_title=&quot;💔 Breakup Recovery Squad&quot;,
    page_icon=&quot;💔&quot;,
    layout=&quot;wide&quot;
)



# Sidebar for API key input
with st.sidebar:
    st.header(&quot;🔑 API Configuration&quot;)

    if &quot;api_key_input&quot; not in st.session_state:
        st.session_state.api_key_input = &quot;&quot;
        
    api_key = st.text_input(
        &quot;Enter your Gemini API Key&quot;,
        value=st.session_state.api_key_input,
        type=&quot;password&quot;,
        help=&quot;Get your API key from Google AI Studio&quot;,
        key=&quot;api_key_widget&quot;  
    )

    if api_key != st.session_state.api_key_input:
        st.session_state.api_key_input = api_key
    
    if api_key:
        st.success(&quot;API Key provided! ✅&quot;)
    else:
        st.warning(&quot;Please enter your API key to proceed&quot;)
        st.markdown(&quot;&quot;&quot;
        To get your API key:
        1. Go to [Google AI Studio](https://makersuite.google.com/app/apikey)
        2. Enable the Generative Language API in your [Google Cloud Console](https://console.developers.google.com/apis/api/generativelanguage.googleapis.com)
        &quot;&quot;&quot;)

# Main content
st.title(&quot;💔 Breakup Recovery Squad&quot;)
st.markdown(&quot;&quot;&quot;
    ### Your AI-powered breakup recovery team is here to help!
    Share your feelings and chat screenshots, and we&#x27;ll help you navigate through this tough time.
&quot;&quot;&quot;)

# Input section
col1, col2 = st.columns(2)

with col1:
    st.subheader(&quot;Share Your Feelings&quot;)
    user_input = st.text_area(
        &quot;How are you feeling? What happened?&quot;,
        height=150,
        placeholder=&quot;Tell us your story...&quot;
    )
    
with col2:
    st.subheader(&quot;Upload Chat Screenshots&quot;)
    uploaded_files = st.file_uploader(
        &quot;Upload screenshots of your chats (optional)&quot;,
        type=[&quot;jpg&quot;, &quot;jpeg&quot;, &quot;png&quot;],
        accept_multiple_files=True,
        key=&quot;screenshots&quot;
    )
    
    if uploaded_files:
        for file in uploaded_files:
            st.image(file, caption=file.name, use_container_width=True)

# Process button and API key check
if st.button(&quot;Get Recovery Plan 💝&quot;, type=&quot;primary&quot;):
    if not st.session_state.api_key_input:
        st.warning(&quot;Please enter your API key in the sidebar first!&quot;)
    else:
        therapist_agent, closure_agent, routine_planner_agent, brutal_honesty_agent = initialize_agents(st.session_state.api_key_input)
        
        if all([therapist_agent, closure_agent, routine_planner_agent, brutal_honesty_agent]):
            if user_input or uploaded_files:
                try:
                    st.header(&quot;Your Personalized Recovery Plan&quot;)
                    
                    def process_images(files):
                        processed_images = []
                        for file in files:
                            try:
                                temp_dir = tempfile.gettempdir()
                                temp_path = os.path.join(temp_dir, f&quot;temp_{file.name}&quot;)
                                
                                with open(temp_path, &quot;wb&quot;) as f:
                                    f.write(file.getvalue())
                                
                                agno_image = AgnoImage(filepath=Path(temp_path))
                                processed_images.append(agno_image)
                                
                            except Exception as e:
                                logger.error(f&quot;Error processing image {file.name}: {str(e)}&quot;)
                                continue
                        return processed_images
                    
                    all_images = process_images(uploaded_files) if uploaded_files else []
                    
                    # Therapist Analysis
                    with st.spinner(&quot;🤗 Getting empathetic support...&quot;):
                        therapist_prompt = f&quot;&quot;&quot;
                        Analyze the emotional state and provide empathetic support based on:
                        User&#x27;s message: {user_input}
                        
                        Please provide a compassionate response with:
                        1. Validation of feelings
                        2. Gentle words of comfort
                        3. Relatable experiences
                        4. Words of encouragement
                        &quot;&quot;&quot;
                        
                        response = therapist_agent.run(
                            message=therapist_prompt,
                            images=all_images
                        )
                        
                        st.subheader(&quot;🤗 Emotional Support&quot;)
                        st.markdown(response.content)
                    
                    # Closure Messages
                    with st.spinner(&quot;✍️ Crafting closure messages...&quot;):
                        closure_prompt = f&quot;&quot;&quot;
                        Help create emotional closure based on:
                        User&#x27;s feelings: {user_input}
                        
                        Please provide:
                        1. Template for unsent messages
                        2. Emotional release exercises
                        3. Closure rituals
                        4. Moving forward strategies
                        &quot;&quot;&quot;
                        
                        response = closure_agent.run(
                            message=closure_prompt,
                            images=all_images
                        )
                        
                        st.subheader(&quot;✍️ Finding Closure&quot;)
                        st.markdown(response.content)
                    
                    # Recovery Plan
                    with st.spinner(&quot;📅 Creating your recovery plan...&quot;):
                        routine_prompt = f&quot;&quot;&quot;
                        Design a 7-day recovery plan based on:
                        Current state: {user_input}
                        
                        Include:
                        1. Daily activities and challenges
                        2. Self-care routines
                        3. Social media guidelines
                        4. Mood-lifting music suggestions
                        &quot;&quot;&quot;
                        
                        response = routine_planner_agent.run(
                            message=routine_prompt,
                            images=all_images
                        )
                        
                        st.subheader(&quot;📅 Your Recovery Plan&quot;)
                        st.markdown(response.content)
                    
                    # Honest Feedback
                    with st.spinner(&quot;💪 Getting honest perspective...&quot;):
                        honesty_prompt = f&quot;&quot;&quot;
                        Provide honest, constructive feedback about:
                        Situation: {user_input}
                        
                        Include:
                        1. Objective analysis
                        2. Growth opportunities
                        3. Future outlook
                        4. Actionable steps
                        &quot;&quot;&quot;
                        
                        response = brutal_honesty_agent.run(
                            message=honesty_prompt,
                            images=all_images
                        )
                        
                        st.subheader(&quot;💪 Honest Perspective&quot;)
                        st.markdown(response.content)
                            
                except Exception as e:
                    logger.error(f&quot;Error during analysis: {str(e)}&quot;)
                    st.error(&quot;An error occurred during analysis. Please check the logs for details.&quot;)
            else:
                st.warning(&quot;Please share your feelings or upload screenshots to get help.&quot;)
        else:
            st.error(&quot;Failed to initialize agents. Please check your API key.&quot;)

# Footer
st.markdown(&quot;---&quot;)
st.markdown(&quot;&quot;&quot;
    &lt;div style=&#x27;text-align: center&#x27;&gt;
        &lt;p&gt;Made with ❤️ by the Breakup Recovery Squad&lt;/p&gt;
        &lt;p&gt;Share your recovery journey with #BreakupRecoverySquad&lt;/p&gt;
    &lt;/div&gt;
&quot;&quot;&quot;, unsafe_allow_html=True)
```