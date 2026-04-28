# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/VmAUdyu8Io2EkRx20f6cQ12tnle](https://qqs7y1hozd1.feishu.cn/docx/VmAUdyu8Io2EkRx20f6cQ12tnle)

```
# 📑 Notion MCP Agent

A terminal-based Notion Agent for interacting with your Notion pages using natural language through the Notion MCP (Model Context Protocol) server.

## Features

- Interact with Notion pages via a command-line interface
- Perform update, insert, retrieve operations on your Notion pages
- Create and edit blocks, lists, tables, and other Notion structures
- Add comments to blocks
- Search for specific information
- Remembers conversation context for multi-turn interactions
- Session management for persistent conversations

## Prerequisites

- Python 3.9+
- A Notion account with admin permissions
- A Notion Integration token
- An OpenAI API key

## Installation

1. Clone the repository
2. Install the required Python packages:

```bash
pip install -r requirements.txt
```

3. Install the Notion MCP server (will be done automatically when you run the app)

## Setting Up Notion Integration

### Creating a Notion Integration

1. Go to [Notion Integrations](https://www.notion.so/my-integrations)
2. Click &quot;New integration&quot;
3. Name your integration (e.g., &quot;Notion Assistant&quot;)
4. Select the capabilities needed (Read &amp; Write content)
5. Submit and copy your &quot;Internal Integration Token&quot;

### Sharing Your Notion Page with the Integration

1. Open your Notion page
2. Click the three dots (⋮) in the top-right corner of the page
3. Select &quot;Add connections&quot; from the dropdown menu
4. Search for your integration name in the search box
5. Click on your integration to add it to the page
6. Confirm by clicking &quot;Confirm&quot; in the dialog that appears

Alternatively, you can also share via the &quot;Share&quot; button:
1. Click &quot;Share&quot; in the top right
2. In the sharing dialog, search for your integration name (preceded by &quot;@&quot;)
3. Click on your integration to add it
4. Click &quot;Invite&quot; to grant it access to your page

Both methods will grant your integration full access to the page and its content.

### Finding Your Notion Page ID

1. Open your Notion page in a browser
2. Copy the URL, which looks like:
   `https://www.notion.so/workspace/Your-Page-1f5b8a8ba283...`
3. The ID is the part after the last dash and before any query parameters
   Example: `1f5b8a8bad058a7e39a6`

## Configuration

You can configure the agent using environment variables:

- `NOTION_API_KEY`: Your Notion Integration token
- `OPENAI_API_KEY`: Your OpenAI API key
- `NOTION_PAGE_ID`: The ID of your Notion page

Alternatively, you can set these values directly in the script.

## Usage

Run the agent from the command line:

```bash
python notion_mcp_agent.py
```

When you start the agent, it will prompt you to enter your Notion page ID. You can:
1. Enter your page ID at the prompt
2. Press Enter without typing anything to use the default page ID (if set)
3. Provide the page ID directly as a command-line argument (bypassing the prompt):

```bash
python notion_mcp_agent.py your-page-id-here
```

### Conversation Flow

Each time you start the agent, it creates a unique user ID and session ID to maintain conversation context. This allows the agent to remember previous interactions and continue coherent conversations even after you close and restart the application.

You can exit the conversation at any time by typing `exit`, `quit`, `bye`, or `goodbye`.

## Example Queries

- &quot;What&#x27;s on my Notion page?&quot;
- &quot;Add a new paragraph saying &#x27;Meeting notes for today&#x27;&quot;
- &quot;Create a bullet list with three items: Apple, Banana, Orange&quot;
- &quot;Add a comment to the first paragraph saying &#x27;This looks good!&#x27;&quot;
- &quot;Search for any mentions of meetings&quot;
- &quot;Summarize our conversation so far&quot;

## License

MIT
```

```
import asyncio
import json
import os
import sys
import uuid
from textwrap import dedent
from agno.agent import Agent 
from agno.models.openai import OpenAIChat
from agno.tools.mcp import MCPTools 
from agno.memory.v2 import Memory
from mcp import StdioServerParameters
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

NOTION_TOKEN = os.getenv(&quot;NOTION_API_KEY&quot;)
OPENAI_API_KEY = os.getenv(&quot;OPENAI_API_KEY&quot;)

async def main():
    print(&quot;\n========================================&quot;)
    print(&quot;      Notion MCP Terminal Agent&quot;)
    print(&quot;========================================\n&quot;)
    
    # Get configuration from environment or use defaults
    notion_token = NOTION_TOKEN
    openai_api_key = OPENAI_API_KEY
    
    # Prompt for page ID first
    if len(sys.argv) &gt; 1:
        # Use command-line argument if provided
        page_id = sys.argv[1]
        print(f&quot;Using provided page ID from command line: {page_id}&quot;)
    else:
        # Ask the user for the page ID
        print(&quot;Please enter your Notion page ID:&quot;)
        print(&quot;(You can find this in your page URL, e.g., https://www.notion.so/workspace/Your-Page-1f5b8a8ba283...)&quot;)
        print(&quot;The ID is the part after the last dash and before any query parameters&quot;)
        
        user_input = input(&quot;&gt; &quot;)
        
        # If user input is empty, use default
        if user_input.strip():
            page_id = user_input.strip()
            print(f&quot;Using provided page ID: {page_id}&quot;)
        else:
            print(f&quot;Using default page ID: {page_id}&quot;)
    
    # Generate unique user and session IDs for this terminal session
    user_id = f&quot;user_{uuid.uuid4().hex[:8]}&quot;
    session_id = f&quot;session_{uuid.uuid4().hex[:8]}&quot;
    print(f&quot;User ID: {user_id}&quot;)
    print(f&quot;Session ID: {session_id}&quot;)
    
    print(&quot;\nConnecting to Notion MCP server...\n&quot;)
    
    # Configure the MCP Tools
    server_params = StdioServerParameters(
        command=&quot;npx&quot;,
        args=[&quot;-y&quot;, &quot;@notionhq/notion-mcp-server&quot;],
        env={
            &quot;OPENAPI_MCP_HEADERS&quot;: json.dumps(
                {&quot;Authorization&quot;: f&quot;Bearer {notion_token}&quot;, &quot;Notion-Version&quot;: &quot;2022-06-28&quot;}
            )
        }
    )
    
    # Start the MCP Tools session
    async with MCPTools(server_params=server_params) as mcp_tools:
        print(&quot;Connected to Notion MCP server successfully!&quot;)
        
        # Create the agent
        agent = Agent(
            name=&quot;NotionDocsAgent&quot;,
            model=OpenAIChat(id=&quot;gpt-4o&quot;, api_key=openai_api_key),
            tools=[mcp_tools],
            description=&quot;Agent to query and modify Notion docs via MCP&quot;,
            instructions=dedent(f&quot;&quot;&quot;
                You are an expert Notion assistant that helps users interact with their Notion pages.
                
                IMPORTANT INSTRUCTIONS:
                1. You have direct access to Notion documents through MCP tools - make full use of them.
                2. ALWAYS use the page ID: {page_id} for all operations unless the user explicitly provides another ID.
                3. When asked to update, read, or search pages, ALWAYS use the appropriate MCP tool calls.
                4. Be proactive in suggesting actions users can take with their Notion documents.
                5. When making changes, explain what you did and confirm the changes were made.
                6. If a tool call fails, explain the issue and suggest alternatives.
                
                Example tasks you can help with:
                - Reading page content
                - Searching for specific information
                - Adding new content or updating existing content
                - Creating lists, tables, and other Notion blocks
                - Explaining page structure
                - Adding comments to specific blocks
                
                The user&#x27;s current page ID is: {page_id}
            &quot;&quot;&quot;),
            markdown=True,
            show_tool_calls=True,
            retries=3,
            memory=Memory(),  # Use Memory v2 for better multi-session support
            add_history_to_messages=True,  # Include conversation history
            num_history_runs=5,  # Keep track of the last 5 interactions
        )
        
        print(&quot;\n\nNotion MCP Agent is ready! Start chatting with your Notion pages.\n&quot;)
        print(&quot;Type &#x27;exit&#x27; or &#x27;quit&#x27; to end the conversation.\n&quot;)
        
        # Start interactive CLI session with memory and proper session management
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