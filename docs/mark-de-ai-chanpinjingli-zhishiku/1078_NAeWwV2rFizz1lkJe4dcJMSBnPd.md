# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/GsxPdNSc4o8QsVxqITScx3HHn4C](https://qqs7y1hozd1.feishu.cn/docx/GsxPdNSc4o8QsVxqITScx3HHn4C)

```
# 🚀 Multi-MCP Intelligent Assistant

The Multi-MCP Intelligent Assistant is a powerful productivity tool that integrates multiple Model Context Protocol (MCP) servers to provide seamless access to GitHub, Perplexity, Calendar, and Gmail services through natural language interactions. This advanced AI assistant is powered by Agno&#x27;s AI Agent framework and designed to be a productivity multiplier across your digital workspace.

## Features

- **Multi-Agent System**
    - **GitHub Integration**: Complete repository management, issue tracking, and code analysis
    - **Perplexity Research**: Real-time web search and information gathering
    - **Calendar Management**: Event scheduling and meeting coordination
    - **Gmail Integration**: Email management and communication workflows

- **Core Capabilities**:
  - Repository management (create, clone, fork, search)
  - Issue &amp; PR workflow (create, update, review, merge, comment)
  - Real-time web search and research
  - Event scheduling and availability management
  - Email organization and automated responses
  - Cross-platform workflow automation

- **Advanced Features**:
  - Interactive CLI with streaming responses
  - Conversation memory and context retention
  - Tool chaining for complex workflows
  - Session-specific user and session IDs
  - Markdown-formatted responses
  - Proactive workflow suggestions

- **Productivity Focus**:
  - Cross-platform automation (GitHub issues → Calendar events)
  - Research-driven development workflows
  - Project management integration
  - Documentation and knowledge sharing

## How to Run

Follow these steps to set up and run the application:

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd awesome-llm-apps/mcp_ai_agents/multi_mcp_agent
   ```

2. **Install the dependencies**:
    ```bash
    pip install -r requirements.txt
    ```

3. **Verify Node.js installation** (required for MCP servers):
    ```bash
    node --version
    npm --version
    npx --version
    ```
    If Node.js is not installed, download it from [nodejs.org](https://nodejs.org/)

4. **Set up your API keys**:
    Create a `.env` file in the project directory with the following variables:
    ```env
    OPENAI_API_KEY=your-openai-api-key
    GITHUB_PERSONAL_ACCESS_TOKEN=your-github-token
    PERPLEXITY_API_KEY=your-perplexity-api-key
    ```

    - Get an OpenAI API key from: https://platform.openai.com/api-keys
    - Get a GitHub Personal Access Token from: https://github.com/settings/tokens (with `repo`, `user`, and `admin:org` scopes)
    - Get a Perplexity API key from: https://www.perplexity.ai/
    - Configure OpenAI MCP Headers according to your setup requirements

5. **Run the Multi-MCP Agent**:
    ```bash
    python multi_mcp_agent.py
    ```

6. **Start Interacting**:
    - The assistant will validate your environment variables
    - Generate unique user and session IDs
    - Initialize connections to all MCP servers
    - Start the interactive CLI interface

## Usage

1. **Environment Validation**: The assistant automatically checks for all required API keys and environment variables
2. **Session Management**: Each session gets unique user and session IDs for tracking and context
3. **Interactive Commands**: Use natural language to interact with integrated services:

### Example Commands

**GitHub Operations**:
- &quot;Show my recent GitHub repositories&quot;
- &quot;Create a new issue in my project repo&quot;
- &quot;Search for Python code in my repositories&quot;
- &quot;Review the latest pull requests&quot;

**Research &amp; Information**:
- &quot;Search for the latest AI developments&quot;
- &quot;What are the trending topics in machine learning?&quot;
- &quot;Find documentation for FastAPI&quot;
- &quot;Research best practices for microservices&quot;

**Calendar Management**:
- &quot;Schedule a meeting for next week&quot;
- &quot;Show my upcoming appointments&quot;
- &quot;Find available time slots for a 2-hour meeting&quot;

**Cross-Platform Workflows**:
- &quot;Create a GitHub issue and schedule a follow-up meeting&quot;
- &quot;Research a topic and create a summary document&quot;
- &quot;Find trending repositories and add them to my watchlist&quot;

4. **Session Control**: Type &#x27;exit&#x27;, &#x27;quit&#x27;, or &#x27;bye&#x27; to end the session

## Architecture

The Multi-MCP Agent leverages:
- **Agno Framework**: For agent orchestration and tool management
- **OpenAI GPT-4o**: As the core language model
- **MCP Servers**: For external service integrations
- **Async Architecture**: For efficient concurrent operations
- **Memory System**: For context retention and conversation history

## Note

The assistant connects to multiple MCP servers using Node.js packages. Ensure you have a stable internet connection and valid API keys for all services. The tool chaining capabilities allow for complex workflows that span multiple platforms, making it a powerful productivity multiplier for developers and professionals.
```

```
import asyncio
import json
import os
import sys
import uuid
from typing import List, Optional
from textwrap import dedent
from agno.agent import Agent 
from agno.models.openai import OpenAIChat
from agno.tools.mcp import MultiMCPTools
from agno.memory.v2 import Memory
from mcp import StdioServerParameters
from agno.utils.pprint import apprint_run_response
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

GITHUB_TOKEN = os.getenv(&quot;GITHUB_PERSONAL_ACCESS_TOKEN&quot;)
OPENAI_API_KEY = os.getenv(&quot;OPENAI_API_KEY&quot;)
PERPLEXITY_API_KEY = os.getenv(&quot;PERPLEXITY_API_KEY&quot;)

async def main():
    print(&quot;\n&quot; + &quot;=&quot;*60)
    print(&quot;           🚀 Multi-MCP Intelligent Assistant 🚀&quot;)
    print(&quot;=&quot;*60)
    print(&quot;🔗 Connected Services: GitHub • Perplexity • Calendar&quot;)
    print(&quot;💡 Powered by OpenAI GPT-4o with Advanced Tool Integration&quot;)
    print(&quot;=&quot;*60 + &quot;\n&quot;)
    
    # Validate required environment variables
    required_vars = {
        &quot;GITHUB_PERSONAL_ACCESS_TOKEN&quot;: GITHUB_TOKEN,
        &quot;OPENAI_API_KEY&quot;: OPENAI_API_KEY,
        &quot;PERPLEXITY_API_KEY&quot;: PERPLEXITY_API_KEY,
    }
    
    missing_vars = [name for name, value in required_vars.items() if not value]
    if missing_vars:
        print(&quot;❌ ERROR: Missing required environment variables:&quot;)
        for var in missing_vars:
            print(f&quot;   • {var}&quot;)
        print(&quot;\nPlease check your .env file and ensure all required variables are set.&quot;)
        return
    
    # Generate unique user and session IDs for this terminal session
    user_id = f&quot;user_{uuid.uuid4().hex[:8]}&quot;
    session_id = f&quot;session_{uuid.uuid4().hex[:8]}&quot;
    print(f&quot;👤 User ID: {user_id}&quot;)
    print(f&quot;🔑 Session ID: {session_id}&quot;)
    
    print(&quot;\n🔌 Initializing MCP server connections...\n&quot;)
    
    # Set up environment variables for MCP servers
    env = {
        **os.environ,
        &quot;GITHUB_PERSONAL_ACCESS_TOKEN&quot;: GITHUB_TOKEN,
        &quot;PERPLEXITY_API_KEY&quot;: PERPLEXITY_API_KEY
    }

    mcp_servers = [
        &quot;npx -y @modelcontextprotocol/server-github&quot;,
        &quot;npx -y @chatmcp/server-perplexity-ask&quot;,
        &quot;npx @gongrzhe/server-calendar-autoauth-mcp&quot;,
        &quot;npx @gongrzhe/server-gmail-autoauth-mcp&quot;
    ]
    
    # Start the MCP Tools session
    async with MultiMCPTools(mcp_servers, env=env) as mcp_tools:
        print(&quot;✅ Successfully connected to all MCP servers!&quot;)
        
        # Create the agent with comprehensive instructions
        agent = Agent(
            name=&quot;MultiMCPAgent&quot;,
            model=OpenAIChat(id=&quot;gpt-4o&quot;, api_key=OPENAI_API_KEY),
            tools=[mcp_tools],
            description=&quot;Advanced AI assistant with GitHub, Perplexity, and Calendar integration&quot;,
            instructions=dedent(f&quot;&quot;&quot;
                You are an elite AI assistant with powerful integrations across multiple platforms. Your mission is to help users be incredibly productive across their digital workspace.

                🎯 CORE CAPABILITIES &amp; INSTRUCTIONS:

                1. 🔧 TOOL MASTERY
                   • You have DIRECT access to GitHub, Notion, Perplexity, and Calendar through MCP tools
                   • ALWAYS use the appropriate MCP tool calls for any requests related to these platforms
                   • Be proactive in suggesting powerful workflows and automations
                   • Chain multiple tool calls together for complex tasks

                2. 📋 GITHUB EXCELLENCE
                   • Repository management: create, clone, fork, search repositories
                   • Issue &amp; PR workflow: create, update, review, merge, comment
                   • Code analysis: search code, review diffs, suggest improvements
                   • Branch management: create, switch, merge branches
                   • Collaboration: manage teams, reviews, and project workflows

                4. 🔍 PERPLEXITY RESEARCH
                   • Real-time web search and research
                   • Current events and trending information
                   • Technical documentation and learning resources
                   • Fact-checking and verification

                5. 📅 CALENDAR INTEGRATION
                   • Event scheduling and management
                   • Meeting coordination and availability
                   • Deadline tracking and reminders

                6. 🎨 INTERACTION PRINCIPLES
                   • Be conversational, helpful, and proactive
                   • Explain what you&#x27;re doing and why
                   • Suggest follow-up actions and optimizations
                   • Handle errors gracefully with alternative solutions
                   • Ask clarifying questions when needed
                   • Provide rich, formatted responses using markdown

                7. 🚀 ADVANCED WORKFLOWS
                   • Cross-platform automation (e.g., GitHub issues → Notion tasks)
                   • Research-driven development (Perplexity → GitHub)
                   • Project management integration
                   • Documentation and knowledge sharing

                SESSION INFO:
                • User ID: {user_id}
                • Session ID: {session_id}
                • Active Services: GitHub, Notion, Perplexity, Calendar

                REMEMBER: You&#x27;re not just answering questions - you&#x27;re a productivity multiplier. Think big, suggest workflows, and help users achieve more than they imagined possible!
            &quot;&quot;&quot;),
            markdown=True,
            show_tool_calls=True,
            retries=3,
            memory=Memory(),
            add_history_to_messages=True,
            num_history_runs=10,  # Increased for better context retention
        )
        
        print(&quot;\n&quot; + &quot;🎉 &quot; + &quot;=&quot;*54 + &quot; 🎉&quot;)
        print(&quot;   Multi-MCP Assistant is READY! Let&#x27;s get productive!&quot;)
        print(&quot;🎉 &quot; + &quot;=&quot;*54 + &quot; 🎉\n&quot;)
        
        print(&quot;💡 Try these example commands:&quot;)
        print(&quot;   • &#x27;Show my recent GitHub repositories&#x27;&quot;)
        print(&quot;   • &#x27;Search for the latest AI developments&#x27;&quot;)
        print(&quot;   • &#x27;Schedule a meeting for next week&#x27;&quot;)
        
        print(&quot;⚡ Type &#x27;exit&#x27;, &#x27;quit&#x27;, or &#x27;bye&#x27; to end the session\n&quot;)
        
        # Start interactive CLI session
        await agent.acli_app(
            user_id=user_id,
            session_id=session_id,
            user=&quot;You&quot;,
            emoji=&quot;🤖&quot;,
            stream=True,
            markdown=True,
            exit_on=[&quot;exit&quot;, &quot;quit&quot;, &quot;bye&quot;, &quot;goodbye&quot;]
        )

if __name__ == &quot;__main__&quot;:
    asyncio.run(main())
```