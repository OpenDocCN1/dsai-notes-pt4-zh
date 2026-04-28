# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/EHocd26sTo0wqYxb4w7ctB5qndN](https://qqs7y1hozd1.feishu.cn/docx/EHocd26sTo0wqYxb4w7ctB5qndN)

```
# 🐙 GitHub MCP Agent

A Streamlit application that allows you to explore and analyze GitHub repositories using natural language queries through the Model Context Protocol (MCP).

## Features

- **Natural Language Interface**: Ask questions about repositories in plain English
- **Comprehensive Analysis**: Explore issues, pull requests, repository activity, and code statistics
- **Interactive UI**: User-friendly interface with example queries and custom input
- **MCP Integration**: Leverages the Model Context Protocol to interact with GitHub&#x27;s API
- **Real-time Results**: Get immediate insights on repository activity and health

## Setup

### Requirements

- Python 3.8+
- Node.js and npm (for MCP GitHub server)
  - This is a critical requirement! The app uses `npx` to run the MCP GitHub server
  - Download and install from [nodejs.org](https://nodejs.org/)
- GitHub Personal Access Token with appropriate permissions
- OpenAI API Key

### Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd mcp-github-agent
   ```

2. Install the required Python packages:
   ```bash
   pip install -r requirements.txt
   ```

3. Verify Node.js and npm are installed:
   ```bash
   node --version
   npm --version
   npx --version
   ```
   All of these commands should return version numbers. If they don&#x27;t, please install Node.js.

4. Set up your API keys:
   - Set OpenAI API Key as an environment variable:
     ```bash
     export OPENAI_API_KEY=your-openai-api-key
     ```
   - GitHub token will be entered directly in the app interface

5. Create a GitHub Personal Access Token:
   - Visit https://github.com/settings/tokens
   - Create a new token with `repo` and `user` scopes
   - Save the token somewhere secure

### Running the App

1. Start the Streamlit app:
   ```bash
   streamlit run app.py
   ```

2. In the app interface:
   - Enter your GitHub token in the sidebar
   - Specify a repository to analyze
   - Select a query type or write your own
   - Click &quot;Run Query&quot;

### Example Queries

#### Issues
- &quot;Show me issues by label&quot;
- &quot;What issues are being actively discussed?&quot;
- &quot;Find issues labeled as bugs&quot;

#### Pull Requests
- &quot;What PRs need review?&quot;
- &quot;Show me recent merged PRs&quot;
- &quot;Find PRs with conflicts&quot;

#### Repository
- &quot;Show repository health metrics&quot;
- &quot;Show repository activity patterns&quot;
- &quot;Analyze code quality trends&quot;
```

```
import asyncio
import os
import streamlit as st
from textwrap import dedent
from agno.agent import Agent
from agno.tools.mcp import MCPTools
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

# Page config
st.set_page_config(page_title=&quot;🐙 GitHub MCP Agent&quot;, page_icon=&quot;🐙&quot;, layout=&quot;wide&quot;)

# Title and description
st.markdown(&quot;&lt;h1 class=&#x27;main-header&#x27;&gt;🐙 GitHub MCP Agent&lt;/h1&gt;&quot;, unsafe_allow_html=True)
st.markdown(&quot;Explore GitHub repositories with natural language using the Model Context Protocol&quot;)

# Setup sidebar for API key
with st.sidebar:
    st.header(&quot;🔑 Authentication&quot;)
    github_token = st.text_input(&quot;GitHub Token&quot;, type=&quot;password&quot;, 
                                help=&quot;Create a token with repo scope at github.com/settings/tokens&quot;)
    
    if github_token:
        os.environ[&quot;GITHUB_TOKEN&quot;] = github_token
    
    st.markdown(&quot;---&quot;)
    st.markdown(&quot;### Example Queries&quot;)
    
    st.markdown(&quot;**Issues**&quot;)
    st.markdown(&quot;- Show me issues by label&quot;)
    st.markdown(&quot;- What issues are being actively discussed?&quot;)
    
    st.markdown(&quot;**Pull Requests**&quot;)
    st.markdown(&quot;- What PRs need review?&quot;)
    st.markdown(&quot;- Show me recent merged PRs&quot;)
    
    st.markdown(&quot;**Repository**&quot;)
    st.markdown(&quot;- Show repository health metrics&quot;)
    st.markdown(&quot;- Show repository activity patterns&quot;)
    
    st.markdown(&quot;---&quot;)
    st.caption(&quot;Note: Always specify the repository in your query if not already selected in the main input.&quot;)

# Query input
col1, col2 = st.columns([3, 1])
with col1:
    repo = st.text_input(&quot;Repository&quot;, value=&quot;Shubhamsaboo/awesome-llm-apps&quot;, help=&quot;Format: owner/repo&quot;)
with col2:
    query_type = st.selectbox(&quot;Query Type&quot;, [
        &quot;Issues&quot;, &quot;Pull Requests&quot;, &quot;Repository Activity&quot;, &quot;Custom&quot;
    ])

# Create predefined queries based on type
if query_type == &quot;Issues&quot;:
    query_template = f&quot;Find issues labeled as bugs in {repo}&quot;
elif query_type == &quot;Pull Requests&quot;:
    query_template = f&quot;Show me recent merged PRs in {repo}&quot;
elif query_type == &quot;Repository Activity&quot;:
    query_template = f&quot;Analyze code quality trends in {repo}&quot;
else:
    query_template = &quot;&quot;

query = st.text_area(&quot;Your Query&quot;, value=query_template, 
                     placeholder=&quot;What would you like to know about this repository?&quot;)

# Main function to run agent
async def run_github_agent(message):
    if not os.getenv(&quot;GITHUB_TOKEN&quot;):
        return &quot;Error: GitHub token not provided&quot;
    
    try:
        server_params = StdioServerParameters(
            command=&quot;npx&quot;,
            args=[&quot;-y&quot;, &quot;@modelcontextprotocol/server-github&quot;],
        )
        
        # Create client session
        async with stdio_client(server_params) as (read, write):
            async with ClientSession(read, write) as session:
                # Initialize MCP toolkit
                mcp_tools = MCPTools(session=session)
                await mcp_tools.initialize()
                
                # Create agent
                agent = Agent(
                    tools=[mcp_tools],
                    instructions=dedent(&quot;&quot;&quot;\
                        You are a GitHub assistant. Help users explore repositories and their activity.
                        - Provide organized, concise insights about the repository
                        - Focus on facts and data from the GitHub API
                        - Use markdown formatting for better readability
                        - Present numerical data in tables when appropriate
                        - Include links to relevant GitHub pages when helpful
                    &quot;&quot;&quot;),
                    markdown=True,
                    show_tool_calls=True,
                )
                
                # Run agent
                response = await agent.arun(message)
                return response.content
    except Exception as e:
        return f&quot;Error: {str(e)}&quot;

# Run button
if st.button(&quot;🚀 Run Query&quot;, type=&quot;primary&quot;, use_container_width=True):
    if not github_token:
        st.error(&quot;Please enter your GitHub token in the sidebar&quot;)
    elif not query:
        st.error(&quot;Please enter a query&quot;)
    else:
        with st.spinner(&quot;Analyzing GitHub repository...&quot;):
            # Ensure the repository is explicitly mentioned in the query
            if repo and repo not in query:
                full_query = f&quot;{query} in {repo}&quot;
            else:
                full_query = query
                
            result = asyncio.run(run_github_agent(full_query))
        
        # Display results in a nice container
        st.markdown(&quot;### Results&quot;)
        st.markdown(result)

# Display help text for first-time users
if &#x27;result&#x27; not in locals():
    st.markdown(
        &quot;&quot;&quot;&lt;div class=&#x27;info-box&#x27;&gt;
        &lt;h4&gt;How to use this app:&lt;/h4&gt;
        &lt;ol&gt;
            &lt;li&gt;Enter your GitHub token in the sidebar&lt;/li&gt;
            &lt;li&gt;Specify a repository (e.g., Shubhamsaboo/awesome-llm-apps)&lt;/li&gt;
            &lt;li&gt;Select a query type or write your own&lt;/li&gt;
            &lt;li&gt;Click &#x27;Run Query&#x27; to see results&lt;/li&gt;
        &lt;/ol&gt;
        &lt;p&gt;&lt;strong&gt;Important Notes:&lt;/strong&gt;&lt;/p&gt;
        &lt;ul&gt;
            &lt;li&gt;The Model Context Protocol (MCP) provides real-time access to GitHub repositories&lt;/li&gt;
            &lt;li&gt;Queries work best when they focus on specific aspects like issues, PRs, or repository info&lt;/li&gt;
            &lt;li&gt;More specific queries yield better results&lt;/li&gt;
            &lt;li&gt;This app requires Node.js to be installed (for the npx command)&lt;/li&gt;
        &lt;/ul&gt;
        &lt;/div&gt;&quot;&quot;&quot;, 
        unsafe_allow_html=True
    )

# Footer
st.markdown(&quot;---&quot;)
st.write(&quot;Built with Streamlit, Agno, and Model Context Protocol ❤️&quot;)
```