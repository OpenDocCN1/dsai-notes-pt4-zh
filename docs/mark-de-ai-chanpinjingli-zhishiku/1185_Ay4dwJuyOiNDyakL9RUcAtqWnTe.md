# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Rr3edYxLjoqXFsxuujdcMuvrnUc](https://qqs7y1hozd1.feishu.cn/docx/Rr3edYxLjoqXFsxuujdcMuvrnUc)

```
from pydantic import BaseModel
from agents import Agent, WebSearchTool
from agents.model_settings import ModelSettings

ARCHITECTURE_AGENT_INSTRUCTIONS = (&quot;&quot;&quot;
You are the Architecture agent for a self-guided audio tour system. Given a location and the areas of interest of user, your role is to:
1. Describe architectural styles, notable buildings, urban planning, and design elements
2. Provide technical insights balanced with accessible explanations
3. Highlight the most visually striking or historically significant structures
4. Adopt a detailed, descriptive voice style when delivering architectural content
5. Make sure not to add any headings like ## Architecture. Just provide the content
6. Make sure the details are conversational and don&#x27;t include any formatting or headings. It will be directly used in a audio model for converting to speech and the entire content should feel like natural speech.
7. Make sure the content is strictly between the upper and lower Word Limit as specified. For example, If the word limit is 100 to 120, it should be within that, not less than 100 or greater than 120

NOTE: Given a location, use web search to retrieve up‑to‑date context and architectural information about the location

NOTE: Do not add any Links or Hyperlinks in your answer or never cite any source

Help users see and appreciate architectural details they might otherwise miss. Make it as detailed and elaborative as possible
&quot;&quot;&quot;)

class Architecture(BaseModel):
    output: str

architecture_agent = Agent(
    name=&quot;ArchitectureAgent&quot;,
    instructions=ARCHITECTURE_AGENT_INSTRUCTIONS,
    model=&quot;gpt-4o-mini&quot;,
    tools=[WebSearchTool()],
    model_settings=ModelSettings(tool_choice=&quot;required&quot;),
    output_type=Architecture
)

CULINARY_AGENT_INSTRUCTIONS = (&quot;&quot;&quot;
You are the Culinary agent for a self-guided audio tour system. Given a location and the areas of interest of user, your role is to:
1. Highlight local food specialties, restaurants, markets, and culinary traditions in the user&#x27;s location
2. Explain the historical and cultural significance of local dishes and ingredients
3. Suggest food stops suitable for the tour duration
4. Adopt an enthusiastic, passionate voice style when delivering culinary content
5. Make sure not to add any headings like ## Culinary. Just provide the content
6. Make sure the details are conversational and don&#x27;t include any formatting or headings. It will be directly used in a audio model for converting to speech and the entire content should feel like natural speech.
7. Make sure the content is strictly between the upper and lower Word Limit as specified. For example, If the word limit is 100 to 120, it should be within that, not less than 100 or greater than 120

NOTE: Given a location, use web search to retrieve up‑to‑date context and culinary information about the location

NOTE: Do not add any Links or Hyperlinks in your answer or never cite any source

Make your descriptions vivid and appetizing. Include practical information like operating hours when relevant. Make it as detailed and elaborative as possible
&quot;&quot;&quot;)

class Culinary(BaseModel):
    output: str


culinary_agent = Agent(
    name=&quot;CulinaryAgent&quot;,
    instructions=CULINARY_AGENT_INSTRUCTIONS,
    model=&quot;gpt-4o-mini&quot;,
    tools=[WebSearchTool()],
    model_settings=ModelSettings(tool_choice=&quot;required&quot;),
    output_type=Culinary
)

CULTURE_AGENT_INSTRUCTIONS = (&quot;&quot;&quot;
You are the Culture agent for a self-guided audio tour system. Given a location and the areas of interest of user, your role is to:
1. Provide information about local traditions, customs, arts, music, and cultural practices
2. Highlight cultural venues and events relevant to the user&#x27;s interests
3. Explain cultural nuances and significance that enhance the visitor&#x27;s understanding
4. Adopt a warm, respectful voice style when delivering cultural content
5. Make sure not to add any headings like ## Culture. Just provide the content
6. Make sure the details are conversational and don&#x27;t include any formatting or headings. It will be directly used in a audio model for converting to speech and the entire content should feel like natural speech.
7. Make sure the content is strictly between the upper and lower Word Limit as specified. For example, If the word limit is 100 to 120, it should be within that, not less than 100 or greater than 120

NOTE: Given a location, use web search to retrieve up‑to‑date context and all the cultural information about the location

NOTE: Do not add any Links or Hyperlinks in your answer or never cite any source

Focus on authentic cultural insights that help users appreciate local ways of life. Make it as detailed and elaborative as possible
&quot;&quot;&quot;)

class Culture(BaseModel):
    output: str

culture_agent = Agent(
    name=&quot;CulturalAgent&quot;,
    instructions=CULTURE_AGENT_INSTRUCTIONS,
    model=&quot;gpt-4o-mini&quot;,
    tools=[WebSearchTool()],
    model_settings=ModelSettings(tool_choice=&quot;required&quot;),
    output_type=Culture
)

HISTORY_AGENT_INSTRUCTIONS = (&quot;&quot;&quot;
You are the History agent for a self-guided audio tour system. Given a location and the areas of interest of user, your role is to:
1. Provide historically accurate information about landmarks, events, and people related to the user&#x27;s location
2. Prioritize the most significant historical aspects based on the user&#x27;s time constraints
3. Include interesting historical facts and stories that aren&#x27;t commonly known
4. Adopt an authoritative, professorial voice style when delivering historical content
5. Make sure not to add any headings like ## History. Just provide the content
6. Make sure the details are conversational and don&#x27;t include any formatting or headings. It will be directly used in a audio model for converting to speech and the entire content should feel like natural speech.
7. Make sure the content is strictly between the upper and lower Word Limit as specified. For example, If the word limit is 100 to 120, it should be within that, not less than 100 or greater than 120

NOTE: Given a location, use web search to retrieve up‑to‑date context and historical information about the location

NOTE: Do not add any Links or Hyperlinks in your answer or never cite any source

Focus on making history come alive through engaging narratives. Keep descriptions concise but informative. Make it as detailed and elaborative as possible
&quot;&quot;&quot;)

class History(BaseModel):
    output: str

historical_agent = Agent(
    name=&quot;HistoricalAgent&quot;,
    instructions=HISTORY_AGENT_INSTRUCTIONS,
    model=&quot;gpt-4o-mini&quot;,
    output_type=History,
    tools=[WebSearchTool()],
    model_settings=ModelSettings(tool_choice=&quot;required&quot;),
)

ORCHESTRATOR_INSTRUCTIONS = (&quot;&quot;&quot;
Your Role
You are the Orchestrator Agent for a self-guided audio tour system. Your task is to assemble a comprehensive and engaging tour for a single location by integrating pre-timed content from four specialist agents (Architecture, History, Culinary, and Culture), while adding introduction and conclusion elements.

Input Parameters
- User Location: The specific location for the tour (e.g., a landmark, neighborhood, or district)
- User Interests: User&#x27;s preference across categories (Architecture, History, Culinary, Culture)
- Specialist Agent Outputs: Pre-sized content from each domain expert (Architecture, History, Culinary, Culture)
- Specialist Agent Word Limit: Word Limit from each domain expert (Architecture, History, Culinary, Culture)

Your Tasks

1. Introduction Creation (1-2 minutes)
Create an engaging and warm introduction that:
- Welcomes the user to the specific location
- Briefly outlines what the tour will cover
- Highlights which categories are emphasized based on user interests
- Sets the tone for the experience (conversational and immersive)

2. Content Integration with Deduplication
Integrate the content from all four agents in the correct order:
- Architecture → History → Culture → Culinary
- Maintain each agent&#x27;s voice and expertise
- Ensure all content fits within its allocated time budget
- Don&#x27;t edit anything from your end and just accumulate the content from the specialised agents

3. Transition Development
Develop smooth transitions between the sections:
- Use natural language to move from one domain to another
- Connect themes when possible (e.g., how architecture influenced culture, or how history shaped food)

4. Conclusion Creation
Write a thoughtful concise and short conclusion that:
- Summarizes key highlights from the tour
- Reinforces the uniqueness of the location
- Connects the explored themes holistically
- Encourages the listener to explore further based on their interests

5. Final Assembly
Assemble the complete tour in the following order:
- Introduction
- Architecture
- History
- Culture
- Culinary
- Conclusion

Ensure:
- Transitions are smooth
- Content is free from redundancy
- Total duration respects the time allocation plan
- The entire output sounds like one cohesive guided experience
&quot;&quot;&quot;)


class FinalTour(BaseModel):
    introduction: str
    &quot;&quot;&quot;A short introduction of the Tour.&quot;&quot;&quot;

    architecture: str
    &quot;&quot;&quot;The Architectural Content&quot;&quot;&quot;

    history: str
    &quot;&quot;&quot;The Historical Content&quot;&quot;&quot;
    
    culture: str
    &quot;&quot;&quot;The Culture Content&quot;&quot;&quot;
    
    culinary: str
    &quot;&quot;&quot;The Culinary Content&quot;&quot;&quot;

    conclusion: str
    &quot;&quot;&quot;A short conclusion of the Tour.&quot;&quot;&quot;


orchestrator_agent = Agent(
    name=&quot;OrchestratorAgent&quot;,
    instructions=ORCHESTRATOR_INSTRUCTIONS,
    model=&quot;gpt-4o-mini&quot;,
    output_type=FinalTour,
)

PLANNER_INSTRUCTIONS = (&quot;&quot;&quot;

Your Role
You are the Planner Agent for a self-guided tour system. Your primary responsibility is to analyze the user&#x27;s location, interests, and requested tour duration to create an optimal time allocation plan for content generation by specialist agents (Architecture, History, Culture, and Culinary).
Input Parameters

User Location: The specific location for the tour
User Interests: User&#x27;s ranked preferences across categories (Architecture, History, Culture, Culinary)
Tour Duration: User&#x27;s selected time (15, 30, or 60 minutes)

Your Tasks
1. Interest Analysis

Evaluate the user&#x27;s interest preferences
Assign weight to each category based on expressed interest level
If no specific preferences are provided, assume equal interest in all categories

2. Location Assessment

Analyze the significance of the specified location for each category
Determine if the location has stronger relevance in particular categories

Example: A cathedral might warrant more time for Architecture and History than Culinary



3. Time Allocation Calculation

Calculate the total content time (excluding introduction and conclusion)
Reserve 1-2 minutes for introduction and 1 minute for conclusion
Distribute the remaining time among the four categories based on:

User interest weights (primary factor)
Location relevance to each category (secondary factor)


Ensure minimum time thresholds for each category (even low-interest categories get some coverage)

4. Scaling for Different Durations

15-minute tour:

Introduction: ~1 minute
Content sections: ~12-13 minutes total (divided among categories)
Conclusion: ~1 minute
Each category gets at least 1 minute, with preferred categories getting more


30-minute tour:

Introduction: ~1.5 minutes
Content sections: ~27 minutes total (divided among categories)
Conclusion: ~1.5 minutes
Each category gets at least 3 minutes, with preferred categories getting more


60-minute tour:

Introduction: ~2 minutes
Content sections: ~56 minutes total (divided among categories)
Conclusion: ~2 minutes
Each category gets at least 5 minutes, with preferred categories getting more      


Your output must be a JSON object with numeric time allocations (in minutes) for each section:

- introduction
- architecture
- history
- culture
- culinary
- conclusion

Only return the number of minutes allocated to each section. Do not include explanations or text descriptions.
Example:
{
  &quot;introduction&quot;: 2,
  &quot;architecture&quot;: 15,
  &quot;history&quot;: 20,
  &quot;culture&quot;: 10,
  &quot;culinary&quot;: 9,
  &quot;conclusion&quot;: 2
}         

Make sure the time allocation adheres to the interests, and the interested section is allocated more time than others.  
&quot;&quot;&quot;)


class Planner(BaseModel):
    introduction: float
    architecture: float
    history: float
    culture: float
    culinary: float
    conclusion: float


planner_agent = Agent(
    name=&quot;PlannerAgent&quot;,
    instructions=PLANNER_INSTRUCTIONS,
    model=&quot;gpt-4o&quot;,
    output_type=Planner,
)
```

```
import streamlit as st
import asyncio
from manager import TourManager
from agents import set_default_openai_key
import json

def tts(text):
    from pathlib import Path
    from openai import OpenAI

    client = OpenAI()
    speech_file_path = Path(__file__).parent / f&quot;speech_tour.mp3&quot;
        
    response = client.audio.speech.create(
        model=&quot;gpt-4o-mini-tts&quot;,
        voice=&quot;nova&quot;,
        input=text,
        instructions=&quot;&quot;&quot;You are a friendly and engaging tour guide. Speak naturally and conversationally, as if you&#x27;re walking alongside the visitor. 
        Use a warm, inviting tone throughout. Avoid robotic or formal language. Make the tour feel like a casual conversation with a knowledgeable friend.
        Use natural transitions between topics and maintain an enthusiastic but relaxed pace.&quot;&quot;&quot;
        )
    response.stream_to_file(speech_file_path)
    return speech_file_path

def run_async(func, *args, **kwargs):
    try:
        return asyncio.run(func(*args, **kwargs))
    except RuntimeError:
        loop = asyncio.get_event_loop()
        return loop.run_until_complete(func(*args, **kwargs))

# Set page config for a better UI
st.set_page_config(
    page_title=&quot;AI Audio Tour Agent&quot;,
    page_icon=&quot;🎧&quot;,
    layout=&quot;wide&quot;,
    initial_sidebar_state=&quot;collapsed&quot;
)

# Sidebar for API key
with st.sidebar:
    st.title(&quot;🔑 Settings&quot;)
    api_key = st.text_input(&quot;OpenAI API Key:&quot;, type=&quot;password&quot;)
    if api_key:
        st.session_state[&quot;OPENAI_API_KEY&quot;] = api_key
        st.success(&quot;API key saved!&quot;)

set_default_openai_key(api_key)

# Main content
st.title(&quot;🎧 AI Audio Tour Agent&quot;)
st.markdown(&quot;&quot;&quot;
    &lt;div class=&#x27;welcome-card&#x27;&gt;
        &lt;h3&gt;Welcome to your personalized audio tour guide!&lt;/h3&gt;
        &lt;p&gt;I&#x27;ll help you explore any location with an engaging, natural-sounding tour tailored to your interests.&lt;/p&gt;
    &lt;/div&gt;
&quot;&quot;&quot;, unsafe_allow_html=True)

# Create a clean layout with cards
col1, col2 = st.columns([2, 1])

with col1:
    st.markdown(&quot;### 📍 Where would you like to explore?&quot;)
    location = st.text_input(&quot;&quot;, placeholder=&quot;Enter a city, landmark, or location...&quot;)
    
    st.markdown(&quot;### 🎯 What interests you?&quot;)
    interests = st.multiselect(
        &quot;&quot;,
        options=[&quot;History&quot;, &quot;Architecture&quot;, &quot;Culinary&quot;, &quot;Culture&quot;],
        default=[&quot;History&quot;, &quot;Architecture&quot;],
        help=&quot;Select the topics you&#x27;d like to learn about&quot;
    )

with col2:
    st.markdown(&quot;### ⏱️ Tour Settings&quot;)
    duration = st.slider(
        &quot;Tour Duration (minutes)&quot;,
        min_value=5,
        max_value=60,
        value=10,
        step=5,
        help=&quot;Choose how long you&#x27;d like your tour to be&quot;
    )
    
    st.markdown(&quot;### 🎙️ Voice Settings&quot;)
    voice_style = st.selectbox(
        &quot;Guide&#x27;s Voice Style&quot;,
        options=[&quot;Friendly &amp; Casual&quot;, &quot;Professional &amp; Detailed&quot;, &quot;Enthusiastic &amp; Energetic&quot;],
        help=&quot;Select the personality of your tour guide&quot;
    )

# Generate Tour Button
if st.button(&quot;🎧 Generate Tour&quot;, type=&quot;primary&quot;):
    if &quot;OPENAI_API_KEY&quot; not in st.session_state:
        st.error(&quot;Please enter your OpenAI API key in the sidebar.&quot;)
    elif not location:
        st.error(&quot;Please enter a location.&quot;)
    elif not interests:
        st.error(&quot;Please select at least one interest.&quot;)
    else:
        with st.spinner(f&quot;Creating your personalized tour of {location}...&quot;):
            mgr = TourManager()
            final_tour = run_async(
                mgr.run, location, interests, duration
            )

            # Display the tour content in an expandable section
            with st.expander(&quot;📝 Tour Content&quot;, expanded=True):
                st.markdown(final_tour)
            
            # Add a progress bar for audio generation
            with st.spinner(&quot;🎙️ Generating audio tour...&quot;):
                progress_bar = st.progress(0)
                tour_audio = tts(final_tour)
                progress_bar.progress(100)
            
            # Display audio player with custom styling
            st.markdown(&quot;### 🎧 Listen to Your Tour&quot;)
            st.audio(tour_audio, format=&quot;audio/mp3&quot;)
            
            # Add download button for the audio
            with open(tour_audio, &quot;rb&quot;) as file:
                st.download_button(
                    label=&quot;📥 Download Audio Tour&quot;,
                    data=file,
                    file_name=f&quot;{location.lower().replace(&#x27; &#x27;, &#x27;_&#x27;)}_tour.mp3&quot;,
                    mime=&quot;audio/mp3&quot;
                )
```

```
from __future__ import annotations

import asyncio
import time
import json
from collections.abc import Sequence

from rich.console import Console

from agents import Runner, RunResult, custom_span, gen_trace_id, trace

from agent import History, historical_agent
from agent import Culinary,culinary_agent
from agent import Culture,culture_agent
from agent import Architecture,architecture_agent
from agent import Planner, planner_agent
from agent import FinalTour, orchestrator_agent
from printer import Printer


class TourManager:
    &quot;&quot;&quot;
    Orchestrates the full flow
    &quot;&quot;&quot;

    def __init__(self) -&gt; None:
        self.console = Console()
        self.printer = Printer(self.console)

    async def run(self, query: str, interests: list, duration: str) -&gt; None:
        trace_id = gen_trace_id()
        with trace(&quot;Tour Research trace&quot;, trace_id=trace_id):
            self.printer.update_item(
                &quot;trace_id&quot;,
                &quot;View trace: https://platform.openai.com/traces/{}&quot;.format(trace_id),
                is_done=True,
                hide_checkmark=True,
            )
            self.printer.update_item(&quot;start&quot;, &quot;Starting tour research...&quot;, is_done=True)
            
            # Get plan based on selected interests
            planner = await self._get_plan(query, interests, duration)
            
            # Initialize research results
            research_results = {}
            
            # Calculate word limits based on duration
            # Assuming average speaking rate of 150 words per minute
            words_per_minute = 150
            total_words = int(duration) * words_per_minute
            words_per_section = total_words // len(interests)
            
            # Only research selected interests
            if &quot;Architecture&quot; in interests:
                research_results[&quot;architecture&quot;] = await self._get_architecture(query, interests, words_per_section)
            
            if &quot;History&quot; in interests:
                research_results[&quot;history&quot;] = await self._get_history(query, interests, words_per_section)
            
            if &quot;Culinary&quot; in interests:
                research_results[&quot;culinary&quot;] = await self._get_culinary(query, interests, words_per_section)
            
            if &quot;Culture&quot; in interests:
                research_results[&quot;culture&quot;] = await self._get_culture(query, interests, words_per_section)
            
            # Get final tour with only selected interests
            final_tour = await self._get_final_tour(
                query, 
                interests, 
                duration, 
                research_results
            )
            
            self.printer.update_item(&quot;final_report&quot;, &quot;&quot;, is_done=True)
            self.printer.end()

        # Build final tour content based on selected interests
        sections = []
        
        # Add selected interest sections without headers
        if &quot;Architecture&quot; in interests:
            sections.append(final_tour.architecture)
        if &quot;History&quot; in interests:
            sections.append(final_tour.history)
        if &quot;Culture&quot; in interests:
            sections.append(final_tour.culture)
        if &quot;Culinary&quot; in interests:
            sections.append(final_tour.culinary)
        
        # Format final tour with natural transitions
        final = &quot;&quot;
        for i, content in enumerate(sections):
            if i &gt; 0:
                final += &quot;\n\n&quot;  # Add spacing between sections
            final += content
            
        return final
        
    async def _get_plan(self, query: str, interests: list, duration: str) -&gt; Planner:
        self.printer.update_item(&quot;Planner&quot;, &quot;Planning your personalized tour...&quot;)
        result = await Runner.run(
            planner_agent, 
            &quot;Query: {} Interests: {} Duration: {}&quot;.format(query, &#x27;, &#x27;.join(interests), duration)
        )
        self.printer.update_item(
            &quot;Planner&quot;,
            &quot;Completed planning&quot;,
            is_done=True,
        )
        return result.final_output_as(Planner)
    
    async def _get_history(self, query: str, interests: list, word_limit: int) -&gt; History:
        self.printer.update_item(&quot;History&quot;, &quot;Researching historical highlights...&quot;)
        result = await Runner.run(
            historical_agent, 
            &quot;Query: {} Interests: {} Word Limit: {} - {}\n\nInstructions: Create engaging historical content for an audio tour. Focus on interesting stories and personal connections. Make it conversational and include specific details that would be interesting to hear while walking. Include specific locations and landmarks where possible. The content should be approximately {} words when spoken at a natural pace.&quot;.format(query, &#x27;, &#x27;.join(interests), word_limit, word_limit + 20, word_limit)
        )
        self.printer.update_item(
            &quot;History&quot;,
            &quot;Completed history research&quot;,
            is_done=True,
        )
        return result.final_output_as(History)

    async def _get_architecture(self, query: str, interests: list, word_limit: int):
        self.printer.update_item(&quot;Architecture&quot;, &quot;Exploring architectural wonders...&quot;)
        result = await Runner.run(
            architecture_agent, 
            &quot;Query: {} Interests: {} Word Limit: {} - {}\n\nInstructions: Create engaging architectural content for an audio tour. Focus on visual descriptions and interesting design details. Make it conversational and include specific buildings and their unique features. Describe what visitors should look for and why it matters. The content should be approximately {} words when spoken at a natural pace.&quot;.format(query, &#x27;, &#x27;.join(interests), word_limit, word_limit + 20, word_limit)
        )
        self.printer.update_item(
            &quot;Architecture&quot;,
            &quot;Completed architecture research&quot;,
            is_done=True,
        )
        return result.final_output_as(Architecture)
    
    async def _get_culinary(self, query: str, interests: list, word_limit: int):
        self.printer.update_item(&quot;Culinary&quot;, &quot;Discovering local flavors...&quot;)
        result = await Runner.run(
            culinary_agent, 
            &quot;Query: {} Interests: {} Word Limit: {} - {}\n\nInstructions: Create engaging culinary content for an audio tour. Focus on local specialties, food history, and interesting stories about restaurants and dishes. Make it conversational and include specific recommendations. Describe the flavors and cultural significance of the food. The content should be approximately {} words when spoken at a natural pace.&quot;.format(query, &#x27;, &#x27;.join(interests), word_limit, word_limit + 20, word_limit)
        )
        self.printer.update_item(
            &quot;Culinary&quot;,
            &quot;Completed culinary research&quot;,
            is_done=True,
        )
        return result.final_output_as(Culinary)
    
    async def _get_culture(self, query: str, interests: list, word_limit: int):
        self.printer.update_item(&quot;Culture&quot;, &quot;Exploring cultural highlights...&quot;)
        result = await Runner.run(
            culture_agent, 
            &quot;Query: {} Interests: {} Word Limit: {} - {}\n\nInstructions: Create engaging cultural content for an audio tour. Focus on local traditions, arts, and community life. Make it conversational and include specific cultural venues and events. Describe the atmosphere and significance of cultural landmarks. The content should be approximately {} words when spoken at a natural pace.&quot;.format(query, &#x27;, &#x27;.join(interests), word_limit, word_limit + 20, word_limit)
        )
        self.printer.update_item(
            &quot;Culture&quot;,
            &quot;Completed culture research&quot;,
            is_done=True,
        )
        return result.final_output_as(Culture)
    
    async def _get_final_tour(self, query: str, interests: list, duration: float, research_results: dict):
        self.printer.update_item(&quot;Final Tour&quot;, &quot;Creating your personalized tour...&quot;)
        
        # Build content sections based on selected interests
        content_sections = []
        for interest in interests:
            if interest.lower() in research_results:
                content_sections.append(research_results[interest.lower()].output)
        
        # Calculate total words based on duration
        # Assuming average speaking rate of 150 words per minute
        words_per_minute = 150
        total_words = int(duration) * words_per_minute
        
        # Create the prompt with proper string formatting
        prompt = (
            &quot;Query: {}\n&quot;
            &quot;Selected Interests: {}\n&quot;
            &quot;Total Tour Duration (in minutes): {}\n&quot;
            &quot;Target Word Count: {}\n\n&quot;
            &quot;Content Sections:\n{}\n\n&quot;
            &quot;Instructions: Create a natural, conversational audio tour that focuses only on the selected interests. &quot;
            &quot;Make it feel like a friendly guide walking alongside the visitor, sharing interesting stories and insights. &quot;
            &quot;Use natural transitions between topics and maintain an engaging but relaxed pace. &quot;
            &quot;Include specific locations and landmarks where possible. &quot;
            &quot;Add natural pauses and transitions as if walking between locations. &quot;
            &quot;Use phrases like &#x27;as we walk&#x27;, &#x27;look to your left&#x27;, &#x27;notice how&#x27;, etc. &quot;
            &quot;Make it interactive and engaging, as if the guide is actually there with the visitor. &quot;
            &quot;Start with a warm welcome and end with a natural closing thought. &quot;
            &quot;The total content should be approximately {} words when spoken at a natural pace of 150 words per minute. &quot;
            &quot;This will ensure the tour lasts approximately {} minutes.&quot;
        ).format(
            query,
            &#x27;, &#x27;.join(interests),
            duration,
            total_words,
            &#x27;\n\n&#x27;.join(content_sections),
            total_words,
            duration
        )
        
        result = await Runner.run(
            orchestrator_agent,
            prompt
        )
        
        self.printer.update_item(
            &quot;Final Tour&quot;,
            &quot;Completed Final Tour Guide Creation&quot;,
            is_done=True,
        )
        return result.final_output_as(FinalTour)
```

```
from typing import Any

from rich.console import Console, Group
from rich.live import Live
from rich.spinner import Spinner


class Printer:
    &quot;&quot;&quot;
    Simple wrapper to stream status updates. Used by the financial bot
    manager as it orchestrates planning, search and writing.
    &quot;&quot;&quot;

    def __init__(self, console: Console) -&gt; None:
        self.live = Live(console=console)
        self.items: dict[str, tuple[str, bool]] = {}
        self.hide_done_ids: set[str] = set()
        self.live.start()

    def end(self) -&gt; None:
        self.live.stop()

    def hide_done_checkmark(self, item_id: str) -&gt; None:
        self.hide_done_ids.add(item_id)

    def update_item(
        self, item_id: str, content: str, is_done: bool = False, hide_checkmark: bool = False
    ) -&gt; None:
        self.items[item_id] = (content, is_done)
        if hide_checkmark:
            self.hide_done_ids.add(item_id)
        self.flush()

    def mark_item_done(self, item_id: str) -&gt; None:
        self.items[item_id] = (self.items[item_id][0], True)
        self.flush()

    def flush(self) -&gt; None:
        renderables: list[Any] = []
        for item_id, (content, is_done) in self.items.items():
            if is_done:
                prefix = &quot;✅ &quot; if item_id not in self.hide_done_ids else &quot;&quot;
                renderables.append(prefix + content)
            else:
                renderables.append(Spinner(&quot;dots&quot;, text=content))
        self.live.update(Group(*renderables))
```