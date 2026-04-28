# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/LkmbdNvXMoWBOixAkGec4HLhn9f](https://qqs7y1hozd1.feishu.cn/docx/LkmbdNvXMoWBOixAkGec4HLhn9f)

```
# 🌐 Browser MCP Agent

https://github.com/user-attachments/assets/a01e09fa-131b-479a-8df3-2d1a61fd80f3

A Streamlit application that allows you to browse and interact with websites using natural language commands through the Model Context Protocol (MCP) and [MCP-Agent](https://github.com/lastmile-ai/mcp-agent) with Playwright integration.

## Features

- **Natural Language Interface**: Control a browser with simple English commands
- **Full Browser Navigation**: Visit websites and navigate through pages
- **Interactive Elements**: Click buttons, fill forms, and scroll through content
- **Visual Feedback**: Take screenshots of webpage elements
- **Information Extraction**: Extract and summarize content from webpages
- **Multi-step Tasks**: Complete complex browsing sequences through conversation

## Setup

### Requirements

- Python 3.8+
- Node.js and npm (for Playwright)
  - This is a critical requirement! The app uses Playwright to control a headless browser
  - Download and install from [nodejs.org](https://nodejs.org/)
- OpenAI or Anthropic API Key

### Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd mcp_ai_agents/browser_mcp_agent
   ```

2. Install the required Python packages:
   ```bash
   pip install -r requirements.txt
   ```

3. Verify Node.js and npm are installed:
   ```bash
   node --version
   npm --version
   ```
   Both commands should return version numbers. If they don&#x27;t, please install Node.js.

4. Set up your API keys:
   - Set OpenAI API Key as an environment variable:
     ```bash
     export OPENAI_API_KEY=your-openai-api-key
     ```


### Running the App

1. Start the Streamlit app:
   ```bash
   streamlit run main.py
   ```

2. In the app interface:
   - Enter your browsing command
   - Click &quot;Run Command&quot;
   - View the results and screenshots

### Example Commands

#### Basic Navigation
- &quot;Go to www.mcp-agent.com&quot;
- &quot;Go back to the previous page&quot;

#### Interaction
- &quot;Click on the login button&quot;
- &quot;Scroll down to see more content&quot;

#### Content Extraction
- &quot;Summarize the main content of this page&quot;
- &quot;Extract the navigation menu items&quot;
- &quot;Take a screenshot of the hero section&quot;

#### Multi-step Tasks
- &quot;Go to the blog, find the most recent article, and summarize its key points&quot;

## Architecture

The application uses:
- Streamlit for the user interface
- MCP (Model Context Protocol) to connect the LLM with tools
- Playwright for browser automation
- [MCP-Agent](https://github.com/lastmile-ai/mcp-agent/) for the Agentic Framework
- OpenAI&#x27;s models to interpret commands and generate responses
```

```
import asyncio
import os
import streamlit as st
from textwrap import dedent

from mcp_agent.app import MCPApp
from mcp_agent.agents.agent import Agent
from mcp_agent.workflows.llm.augmented_llm_openai import OpenAIAugmentedLLM
from mcp_agent.workflows.llm.augmented_llm import RequestParams

# Page config
st.set_page_config(page_title=&quot;Browser MCP Agent&quot;, page_icon=&quot;🌐&quot;, layout=&quot;wide&quot;)

# Title and description
st.markdown(&quot;&lt;h1 class=&#x27;main-header&#x27;&gt;🌐 Browser MCP Agent&lt;/h1&gt;&quot;, unsafe_allow_html=True)
st.markdown(&quot;Interact with a powerful web browsing agent that can navigate and interact with websites&quot;)

# Setup sidebar with example commands
with st.sidebar:
    st.markdown(&quot;### Example Commands&quot;)
    
    st.markdown(&quot;**Navigation**&quot;)
    st.markdown(&quot;- Go to github.com/Shubhamsaboo/awesome-llm-apps&quot;)
    
    st.markdown(&quot;**Interactions**&quot;)
    st.markdown(&quot;- click on mcp_ai_agents&quot;)
    st.markdown(&quot;- Scroll down to view more content&quot;)
    
    st.markdown(&quot;**Multi-step Tasks**&quot;)
    st.markdown(&quot;- Navigate to github.com/Shubhamsaboo/awesome-llm-apps, scroll down, and report details&quot;)
    st.markdown(&quot;- Scroll down and summarize the github readme&quot;)
    
    st.markdown(&quot;---&quot;)
    st.caption(&quot;Note: The agent uses Playwright to control a real browser.&quot;)

# Query input
query = st.text_area(&quot;Your Command&quot;, 
                   placeholder=&quot;Ask the agent to navigate to websites and interact with them&quot;)

# Initialize app and agent
if &#x27;initialized&#x27; not in st.session_state:
    st.session_state.initialized = False
    st.session_state.mcp_app = MCPApp(name=&quot;streamlit_mcp_agent&quot;)
    st.session_state.mcp_context = None
    st.session_state.mcp_agent_app = None
    st.session_state.browser_agent = None
    st.session_state.llm = None
    st.session_state.loop = asyncio.new_event_loop()
    asyncio.set_event_loop(st.session_state.loop)
    st.session_state.is_processing = False

# Setup function that runs only once
async def setup_agent():
    if not st.session_state.initialized:
        try:
            # Create context manager and store it in session state
            st.session_state.mcp_context = st.session_state.mcp_app.run()
            st.session_state.mcp_agent_app = await st.session_state.mcp_context.__aenter__()
            
            # Create and initialize agent
            st.session_state.browser_agent = Agent(
                name=&quot;browser&quot;,
                instruction=&quot;&quot;&quot;You are a helpful web browsing assistant that can interact with websites using playwright.
                    - Navigate to websites and perform browser actions (click, scroll, type)
                    - Extract information from web pages 
                    - Take screenshots of page elements when useful
                    - Provide concise summaries of web content using markdown
                    - Follow multi-step browsing sequences to complete tasks
                    
                Respond back with a status update on completing the commands.&quot;&quot;&quot;,
                server_names=[&quot;playwright&quot;],
            )
            
            # Initialize agent and attach LLM
            await st.session_state.browser_agent.initialize()
            st.session_state.llm = await st.session_state.browser_agent.attach_llm(OpenAIAugmentedLLM)
            
            # List tools once
            logger = st.session_state.mcp_agent_app.logger
            tools = await st.session_state.browser_agent.list_tools()
            logger.info(&quot;Tools available:&quot;, data=tools)
            
            # Mark as initialized
            st.session_state.initialized = True
        except Exception as e:
            return f&quot;Error during initialization: {str(e)}&quot;
    return None

# Main function to run agent
async def run_mcp_agent(message):
    if not os.getenv(&quot;OPENAI_API_KEY&quot;):
        return &quot;Error: OpenAI API key not provided&quot;
    
    try:
        # Make sure agent is initialized
        error = await setup_agent()
        if error:
            return error
        
        # Generate response without recreating agents
        # Switch use_history to False to reduce the passed context
        result = await st.session_state.llm.generate_str(
            message=message, 
            request_params=RequestParams(use_history=True, maxTokens=10000)
            )
        return result
    except Exception as e:
        return f&quot;Error: {str(e)}&quot;

# Defaults
if &#x27;is_processing&#x27; not in st.session_state:
    st.session_state.is_processing = False
if &#x27;last_result&#x27; not in st.session_state:
    st.session_state.last_result = None

def start_run():
    st.session_state.is_processing = True

# Button (use a callback so the click just flips state)
st.button(
    &quot;🚀 Run Command&quot;,
    type=&quot;primary&quot;,
    use_container_width=True,
    disabled=st.session_state.is_processing,
    on_click=start_run,
)

# If we’re in a processing run, do the work now
if st.session_state.is_processing:
    with st.spinner(&quot;Processing your request...&quot;):
        result = st.session_state.loop.run_until_complete(run_mcp_agent(query))
    # persist result across the next rerun
    st.session_state.last_result = result
    # unlock the button and refresh UI
    st.session_state.is_processing = False
    st.rerun()

# Render the most recent result (after the rerun)
if st.session_state.last_result:
    st.markdown(&quot;### Response&quot;)
    st.markdown(st.session_state.last_result)
else:
    # (your existing help text here)
    pass

# Display help text for first-time users
if &#x27;result&#x27; not in locals():
    st.markdown(
        &quot;&quot;&quot;&lt;div style=&#x27;padding: 20px; background-color: #f0f2f6; border-radius: 10px;&#x27;&gt;
        &lt;h4&gt;How to use this app:&lt;/h4&gt;
        &lt;ol&gt;
            &lt;li&gt;Enter your OpenAI API key in your mcp_agent.secrets.yaml file&lt;/li&gt;
            &lt;li&gt;Type a command for the agent to navigate and interact with websites&lt;/li&gt;
            &lt;li&gt;Click &#x27;Run Command&#x27; to see results&lt;/li&gt;
        &lt;/ol&gt;
        &lt;p&gt;&lt;strong&gt;Capabilities:&lt;/strong&gt;&lt;/p&gt;
        &lt;ul&gt;
            &lt;li&gt;Navigate to websites using Playwright&lt;/li&gt;
            &lt;li&gt;Click on elements, scroll, and type text&lt;/li&gt;
            &lt;li&gt;Take screenshots of specific elements&lt;/li&gt;
            &lt;li&gt;Extract information from web pages&lt;/li&gt;
            &lt;li&gt;Perform multi-step browsing tasks&lt;/li&gt;
        &lt;/ul&gt;
        &lt;/div&gt;&quot;&quot;&quot;, 
        unsafe_allow_html=True
    )

# Footer
st.markdown(&quot;---&quot;)
st.write(&quot;Built with Streamlit, Playwright, and [MCP-Agent](https://www.github.com/lastmile-ai/mcp-agent) Framework ❤️&quot;)
```