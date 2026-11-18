
# Multi-MCP Intelligent Assistant

> 来源：https://qqs7y1hozd1.feishu.cn/docx/GsxPdNSc4o8QsVxqITScx3HHn4C

The Multi-MCP Intelligent Assistant is a powerful productivity tool that integrates multiple Model Context Protocol (MCP) servers to provide seamless access to GitHub, Perplexity, Calendar, and Gmail services through natural language interactions. This advanced AI assistant is powered by Agno's AI Agent framework and designed to be a productivity multiplier across your digital workspace.

## Features

### Multi-Agent System
- **GitHub Integration**: Complete repository management, issue tracking, and code analysis
- **Perplexity Research**: Real-time web search and information gathering
- **Calendar Management**: Event scheduling and meeting coordination
- **Gmail Integration**: Email management and communication workflows

### Core Capabilities
- Repository management (create, clone, fork, search)
- Issue & PR workflow (create, update, review, merge, comment)
- Real-time web search and research
- Event scheduling and availability management
- Email organization and automated responses
- Cross-platform workflow automation

### Advanced Features
- Interactive CLI with streaming responses
- Conversation memory and context retention
- Tool chaining for complex workflows
- Session-specific user and session IDs
- Markdown-formatted responses
- Proactive workflow suggestions

### Productivity Focus
- Cross-platform automation (GitHub issues → Calendar events)
- Research-driven development workflows
- Project management integration
- Documentation and knowledge sharing

## How to Run

Follow these steps to set up and run the application:

### Clone the Repository
```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/mcp_ai_agents/multi_mcp_agent
```

### Install the dependencies
```bash
pip install -r requirements.txt
```

### Verify Node.js installation
Node.js is required for MCP servers.
```bash
node --version
npm --version
npx --version
```
If Node.js is not installed, download it from https://nodejs.org/

### Set up your API keys
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

### Run the Multi-MCP Agent
```bash
python multi_mcp_agent.py
```

### Start Interacting
- The assistant will validate your environment variables
- Generate unique user and session IDs
- Initialize connections to all MCP servers
- Start the interactive CLI interface

## Usage

### Environment Validation
The assistant automatically checks for all required API keys and environment variables

### Session Management
Each session gets unique user and session IDs for tracking and context

### Interactive Commands
Use natural language to interact with integrated services:

#### GitHub Operations
- Show my recent GitHub repositories
- Create a new issue in my project repo
- Search for Python code in my repositories
- Review the latest pull requests

#### Research & Information
- Search for the latest AI developments
- What are the trending topics in machine learning?
- Find documentation for FastAPI
- Research best practices for microservices

#### Calendar Management
- Schedule a meeting for next week
- Show my upcoming appointments
- Find available time slots for a 2-hour meeting

#### Cross-Platform Workflows
- Create a GitHub issue and schedule a follow-up meeting
- Research a topic and create a summary document
- Find trending repositories and add them to my watchlist

### Session Control
Type `exit`, `quit`, or `bye` to end the session

## Architecture

The Multi-MCP Agent leverages:
- **Agno Framework**: For agent orchestration and tool management
- **OpenAI GPT-4o**: As the core language model
- **MCP Servers**: For external service integrations
- **Async Architecture**: For efficient concurrent operations
- **Memory System**: For context retention and conversation history

## Note

The assistant connects to multiple MCP servers using Node.js packages. Ensure you have a stable internet connection and valid API keys for all services. The tool chaining capabilities allow for complex workflows that span multiple platforms, making it a powerful productivity multiplier for developers and professionals.



```python
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

GITHUB_TOKEN = os.getenv("GITHUB_PERSONAL_ACCESS_TOKEN")
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")
PERPLEXITY_API_KEY = os.getenv("PERPLEXITY_API_KEY")

async def main():
    print("\n" + "="*60)
    print("           🚀 Multi-MCP Intelligent Assistant 🚀")
    print("="*60)
    print("🔗 Connected Services: GitHub • Perplexity • Calendar")
    print("💡 Powered by OpenAI GPT-4o with Advanced Tool Integration")
    print("="*60 + "\n")
    
    # Validate required environment variables
    required_vars = {
        "GITHUB_PERSONAL_ACCESS_TOKEN": GITHUB_TOKEN,
        "OPENAI_API_KEY": OPENAI_API_KEY,
        "PERPLEXITY_API_KEY": PERPLEXITY_API_KEY,
    }
    
    missing_vars = [name for name, value in required_vars.items() if not value]
    if missing_vars:
        print("❌ ERROR: Missing required environment variables:")
        for var in missing_vars:
            print(f"   • {var}")
        print("\nPlease check your .env file and ensure all required variables are set.")
        return
    
    # Generate unique user and session IDs for this terminal session
    user_id = f"user_{uuid.uuid4().hex[:8]}"
    session_id = f"session_{uuid.uuid4().hex[:8]}"
    print(f"👤 User ID: {user_id}")
    print(f"🔑 Session ID: {session_id}")
    
    print("\n🔌 Initializing MCP server connections...\n")
    
    # Set up environment variables for MCP servers
    env = {
        **os.environ,
        "GITHUB_PERSONAL_ACCESS_TOKEN": GITHUB_TOKEN,
        "PERPLEXITY_API_KEY": PERPLEXITY_API_KEY
    }

    mcp_servers = [
        "npx -y @modelcontextprotocol/server-github",
        "npx -y @chatmcp/server-perplexity-ask",
        "npx @gongrzhe/server-calendar-autoauth-mcp",
        "npx @gongrzhe/server-gmail-autoauth-mcp"
    ]
    
    # Start the MCP Tools session
    async with MultiMCPTools(mcp_servers, env=env) as mcp_tools:
        print("✅ Successfully connected to all MCP servers!")
        
        # Create the agent with comprehensive instructions
        agent = Agent(
            name="MultiMCPAgent",
            model=OpenAIChat(id="gpt-4o", api_key=OPENAI_API_KEY),
            tools=[mcp_tools],
            description="Advanced AI assistant with GitHub, Perplexity, and Calendar integration",
            instructions=dedent(f"""
                You are an elite AI assistant with powerful integrations across multiple platforms. Your mission is to help users be incredibly productive across their digital workspace.

                🎯 CORE CAPABILITIES & INSTRUCTIONS:

                1. 🔧 TOOL MASTERY
                   • You have DIRECT access to GitHub, Notion, Perplexity, and Calendar through MCP tools
                   • ALWAYS use the appropriate MCP tool calls for any requests related to these platforms
                   • Be proactive in suggesting powerful workflows and automations
                   • Chain multiple tool calls together for complex tasks

                2. 📋 GITHUB EXCELLENCE
                   • Repository management: create, clone, fork, search repositories
                   • Issue & PR workflow: create, update, review, merge, comment
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
                   • Explain what you're doing and why
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

                REMEMBER: You're not just answering questions - you're a productivity multiplier. Think big, suggest workflows, and help users achieve more than they imagined possible!
            """),
            markdown=True,
            show_tool_calls=True,
            retries=3,
            memory=Memory(),
            add_history_to_messages=True,
            num_history_runs=10,  # Increased for better context retention
        )
        
        print("\n" + "🎉 " + "="*54 + " 🎉")
        print("   Multi-MCP Assistant is READY! Let's get productive!")
        print("🎉 " + "="*54 + " 🎉\n")
        
        print("💡 Try these example commands:")
        print("   • 'Show my recent GitHub repositories'")
        print("   • 'Search for the latest AI developments'")
        print("   • 'Schedule a meeting for next week'")
        
        print("⚡ Type 'exit', 'quit', or 'bye' to end the session\n")
        
        # Start interactive CLI session
        await agent.acli_app(
            user_id=user_id,
            session_id=session_id,
            user="You",
            emoji="🤖",
            stream=True,
            markdown=True,
            exit_on=["exit", "quit", "bye", "goodbye"]
        )

if __name__ == "__main__":
    asyncio.run(main())
```
