
# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/V6bOdX8fZoffFSxT7f0c4XvOnTe](https://qqs7y1hozd1.feishu.cn/docx/V6bOdX8fZoffFSxT7f0c4XvOnTe)



```
import os
import uuid
import asyncio
import streamlit as st
from datetime import datetime
from dotenv import load_dotenv

from agents import (
    Agent, 
    Runner, 
    WebSearchTool, 
    function_tool, 
    handoff, 
    trace,
)

from pydantic import BaseModel

# Load environment variables
load_dotenv()

# Set up page configuration
st.set_page_config(
    page_title=&quot;OpenAI Researcher Agent&quot;,
    page_icon=&quot;📰&quot;,
    layout=&quot;wide&quot;,
    initial_sidebar_state=&quot;expanded&quot;
)

# Make sure API key is set
if not os.environ.get(&quot;OPENAI_API_KEY&quot;):
    st.error(&quot;Please set your OPENAI_API_KEY environment variable&quot;)
    st.stop()

# App title and description
st.title(&quot;📰 OpenAI Researcher Agent&quot;)
st.subheader(&quot;Powered by OpenAI Agents SDK&quot;)
st.markdown(&quot;&quot;&quot;
This app demonstrates the power of OpenAI&#x27;s Agents SDK by creating a multi-agent system 
that researches news topics and generates comprehensive research reports.
&quot;&quot;&quot;)

# Define data models
class ResearchPlan(BaseModel):
    topic: str
    search_queries: list[str]
    focus_areas: list[str]

class ResearchReport(BaseModel):
    title: str
    outline: list[str]
    report: str
    sources: list[str]
    word_count: int

# Custom tool for saving facts found during research
@function_tool
def save_important_fact(fact: str, source: str = None) -&gt; str:
    &quot;&quot;&quot;Save an important fact discovered during research.
    
    Args:
        fact: The important fact to save
        source: Optional source of the fact
    
    Returns:
        Confirmation message
    &quot;&quot;&quot;
    if &quot;collected_facts&quot; not in st.session_state:
        st.session_state.collected_facts = []
    
    st.session_state.collected_facts.append({
        &quot;fact&quot;: fact,
        &quot;source&quot;: source or &quot;Not specified&quot;,
        &quot;timestamp&quot;: datetime.now().strftime(&quot;%H:%M:%S&quot;)
    })
    
    return f&quot;Fact saved: {fact}&quot;

# Define the agents
research_agent = Agent(
    name=&quot;Research Agent&quot;,
    instructions=&quot;You are a research assistant. Given a search term, you search the web for that term and&quot;
    &quot;produce a concise summary of the results. The summary must 2-3 paragraphs and less than 300&quot;
    &quot;words. Capture the main points. Write succintly, no need to have complete sentences or good&quot;
    &quot;grammar. This will be consumed by someone synthesizing a report, so its vital you capture the&quot;
    &quot;essence and ignore any fluff. Do not include any additional commentary other than the summary&quot;
    &quot;itself.&quot;,
    model=&quot;gpt-4o-mini&quot;,
    tools=[
        WebSearchTool(),
        save_important_fact
    ],
)

editor_agent = Agent(
    name=&quot;Editor Agent&quot;,
    handoff_description=&quot;A senior researcher who writes comprehensive research reports&quot;,
    instructions=&quot;You are a senior researcher tasked with writing a cohesive report for a research query. &quot;
    &quot;You will be provided with the original query, and some initial research done by a research &quot;
    &quot;assistant.\n&quot;
    &quot;You should first come up with an outline for the report that describes the structure and &quot;
    &quot;flow of the report. Then, generate the report and return that as your final output.\n&quot;
    &quot;The final output should be in markdown format, and it should be lengthy and detailed. Aim &quot;
    &quot;for 5-10 pages of content, at least 1000 words.&quot;,
    model=&quot;gpt-4o-mini&quot;,
    output_type=ResearchReport,
)

triage_agent = Agent(
    name=&quot;Triage Agent&quot;,
    instructions=&quot;&quot;&quot;You are the coordinator of this research operation. Your job is to:
    1. Understand the user&#x27;s research topic
    2. Create a research plan with the following elements:
       - topic: A clear statement of the research topic
       - search_queries: A list of 3-5 specific search queries that will help gather information
       - focus_areas: A list of 3-5 key aspects of the topic to investigate
    3. Hand off to the Research Agent to collect information
    4. After research is complete, hand off to the Editor Agent who will write a comprehensive report
    
    Make sure to return your plan in the expected structured format with topic, search_queries, and focus_areas.
    &quot;&quot;&quot;,
    handoffs=[
        handoff(research_agent),
        handoff(editor_agent)
    ],
    model=&quot;gpt-4o-mini&quot;,
    output_type=ResearchPlan,
)

# Create sidebar for input and controls
with st.sidebar:
    st.header(&quot;Research Topic&quot;)
    user_topic = st.text_input(
        &quot;Enter a topic to research:&quot;,
    )
    
    start_button = st.button(&quot;Start Research&quot;, type=&quot;primary&quot;, disabled=not user_topic)
    
    st.divider()
    st.subheader(&quot;Example Topics&quot;)
    example_topics = [
        &quot;What are the best cruise lines in USA for first-time travelers who have never been on a cruise?&quot;,
        &quot;What are the best affordable espresso machines for someone upgrading from a French press?&quot;,
        &quot;What are the best off-the-beaten-path destinations in India for a first-time solo traveler?&quot;
    ]
    
    for topic in example_topics:
        if st.button(topic):
            user_topic = topic
            start_button = True

# Main content area with two tabs
tab1, tab2 = st.tabs([&quot;Research Process&quot;, &quot;Report&quot;])

# Initialize session state for storing results
if &quot;conversation_id&quot; not in st.session_state:
    st.session_state.conversation_id = str(uuid.uuid4().hex[:16])
if &quot;collected_facts&quot; not in st.session_state:
    st.session_state.collected_facts = []
if &quot;research_done&quot; not in st.session_state:
    st.session_state.research_done = False
if &quot;report_result&quot; not in st.session_state:
    st.session_state.report_result = None

# Main research function
async def run_research(topic):
    # Reset state for new research
    st.session_state.collected_facts = []
    st.session_state.research_done = False
    st.session_state.report_result = None
    
    with tab1:
        message_container = st.container()
        
    # Create error handling container
    error_container = st.empty()
        
    # Create a trace for the entire workflow
    with trace(&quot;News Research&quot;, group_id=st.session_state.conversation_id):
        # Start with the triage agent
        with message_container:
            st.write(&quot;🔍 **Triage Agent**: Planning research approach...&quot;)
        
        triage_result = await Runner.run(
            triage_agent,
            f&quot;Research this topic thoroughly: {topic}. This research will be used to create a comprehensive research report.&quot;
        )
        
        # Check if the result is a ResearchPlan object or a string
        if hasattr(triage_result.final_output, &#x27;topic&#x27;):
            research_plan = triage_result.final_output
            plan_display = {
                &quot;topic&quot;: research_plan.topic,
                &quot;search_queries&quot;: research_plan.search_queries,
                &quot;focus_areas&quot;: research_plan.focus_areas
            }
        else:
            # Fallback if we don&#x27;t get the expected output type
            research_plan = {
                &quot;topic&quot;: topic,
                &quot;search_queries&quot;: [&quot;Researching &quot; + topic],
                &quot;focus_areas&quot;: [&quot;General information about &quot; + topic]
            }
            plan_display = research_plan
        
        with message_container:
            st.write(&quot;📋 **Research Plan**:&quot;)
            st.json(plan_display)
        
        # Display facts as they&#x27;re collected
        fact_placeholder = message_container.empty()
        
        # Check for new facts periodically
        previous_fact_count = 0
        for i in range(15):  # Check more times to allow for more comprehensive research
            current_facts = len(st.session_state.collected_facts)
            if current_facts &gt; previous_fact_count:
                with fact_placeholder.container():
                    st.write(&quot;📚 **Collected Facts**:&quot;)
                    for fact in st.session_state.collected_facts:
                        st.info(f&quot;**Fact**: {fact[&#x27;fact&#x27;]}\n\n**Source**: {fact[&#x27;source&#x27;]}&quot;)
                previous_fact_count = current_facts
            await asyncio.sleep(1)
        
        # Editor Agent phase
        with message_container:
            st.write(&quot;📝 **Editor Agent**: Creating comprehensive research report...&quot;)
        
        try:
            report_result = await Runner.run(
                editor_agent,
                triage_result.to_input_list()
            )
            
            st.session_state.report_result = report_result.final_output
            
            with message_container:
                st.write(&quot;✅ **Research Complete! Report Generated.**&quot;)
                
                # Preview a snippet of the report
                if hasattr(report_result.final_output, &#x27;report&#x27;):
                    report_preview = report_result.final_output.report[:300] + &quot;...&quot;
                else:
                    report_preview = str(report_result.final_output)[:300] + &quot;...&quot;
                    
                st.write(&quot;📄 **Report Preview**:&quot;)
                st.markdown(report_preview)
                st.write(&quot;*See the Report tab for the full document.*&quot;)
                
        except Exception as e:
            st.error(f&quot;Error generating report: {str(e)}&quot;)
            # Fallback to display raw agent response
            if hasattr(triage_result, &#x27;new_items&#x27;):
                messages = [item for item in triage_result.new_items if hasattr(item, &#x27;content&#x27;)]
                if messages:
                    raw_content = &quot;\n\n&quot;.join([str(m.content) for m in messages if m.content])
                    st.session_state.report_result = raw_content
                    
                    with message_container:
                        st.write(&quot;⚠️ **Research completed but there was an issue generating the structured report.**&quot;)
                        st.write(&quot;Raw research results are available in the Report tab.&quot;)
    
    st.session_state.research_done = True

# Run the research when the button is clicked
if start_button:
    with st.spinner(f&quot;Researching: {user_topic}&quot;):
        try:
            asyncio.run(run_research(user_topic))
        except Exception as e:
            st.error(f&quot;An error occurred during research: {str(e)}&quot;)
            # Set a basic report result so the user gets something
            st.session_state.report_result = f&quot;# Research on {user_topic}\n\nUnfortunately, an error occurred during the research process. Please try again later or with a different topic.\n\nError details: {str(e)}&quot;
            st.session_state.research_done = True

# Display results in the Report tab
with tab2:
    if st.session_state.research_done and st.session_state.report_result:
        report = st.session_state.report_result
        
        # Handle different possible types of report results
        if hasattr(report, &#x27;title&#x27;):
            # We have a properly structured ResearchReport object
            title = report.title
            
            # Display outline if available
            if hasattr(report, &#x27;outline&#x27;) and report.outline:
                with st.expander(&quot;Report Outline&quot;, expanded=True):
                    for i, section in enumerate(report.outline):
                        st.markdown(f&quot;{i+1}. {section}&quot;)
            
            # Display word count if available
            if hasattr(report, &#x27;word_count&#x27;):
                st.info(f&quot;Word Count: {report.word_count}&quot;)
            
            # Display the full report in markdown
            if hasattr(report, &#x27;report&#x27;):
                report_content = report.report
                st.markdown(report_content)
            else:
                report_content = str(report)
                st.markdown(report_content)
            
            # Display sources if available
            if hasattr(report, &#x27;sources&#x27;) and report.sources:
                with st.expander(&quot;Sources&quot;):
                    for i, source in enumerate(report.sources):
                        st.markdown(f&quot;{i+1}. {source}&quot;)
            
            # Add download button for the report
            st.download_button(
                label=&quot;Download Report&quot;,
                data=report_content,
                file_name=f&quot;{title.replace(&#x27; &#x27;, &#x27;_&#x27;)}.md&quot;,
                mime=&quot;text/markdown&quot;
            )
        else:
            # Handle string or other type of response
            report_content = str(report)
            title = user_topic.title()
            
            st.title(f&quot;{title}&quot;)
            st.markdown(report_content)
            
            # Add download button for the report
            st.download_button(
                label=&quot;Download Report&quot;,
                data=report_content,
                file_name=f&quot;{title.replace(&#x27; &#x27;, &#x27;_&#x27;)}.md&quot;,
                mime=&quot;text/markdown&quot;
            )
```
