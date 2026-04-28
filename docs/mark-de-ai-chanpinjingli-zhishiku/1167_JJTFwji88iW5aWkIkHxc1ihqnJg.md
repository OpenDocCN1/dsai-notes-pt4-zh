# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/O3SKdtj3YoaaGTxKbyTcowk7nfb](https://qqs7y1hozd1.feishu.cn/docx/O3SKdtj3YoaaGTxKbyTcowk7nfb)

```
from textwrap import dedent
from agno.agent import Agent
from agno.tools.serpapi import SerpApiTools
import streamlit as st
import re
from agno.models.ollama import Ollama
from icalendar import Calendar, Event
from datetime import datetime, timedelta


def generate_ics_content(plan_text:str, start_date: datetime = None) -&gt; bytes:
    &quot;&quot;&quot;
        Generate an ICS calendar file from a travel itinerary text.

        Args:
            plan_text: The travel itinerary text
            start_date: Optional start date for the itinerary (defaults to today)

        Returns:
            bytes: The ICS file content as bytes
        &quot;&quot;&quot;
    cal = Calendar()
    cal.add(&#x27;prodid&#x27;,&#x27;-//AI Travel Planner//github.com//&#x27; )
    cal.add(&#x27;version&#x27;, &#x27;2.0&#x27;)

    if start_date is None:
        start_date = datetime.today()

    # Split the plan into days
    day_pattern = re.compile(r&#x27;Day (\d+)[:\s]+(.*?)(?=Day \d+|$)&#x27;, re.DOTALL)
    days = day_pattern.findall(plan_text)

    if not days: # If no day pattern found, create a single all-day event with the entire content
        event = Event()
        event.add(&#x27;summary&#x27;, &quot;Travel Itinerary&quot;)
        event.add(&#x27;description&#x27;, plan_text)
        event.add(&#x27;dtstart&#x27;, start_date.date())
        event.add(&#x27;dtend&#x27;, start_date.date())
        event.add(&quot;dtstamp&quot;, datetime.now())
        cal.add_component(event)  
    else:
        # Process each day
        for day_num, day_content in days:
            day_num = int(day_num)
            current_date = start_date + timedelta(days=day_num - 1)
            
            # Create a single event for the entire day
            event = Event()
            event.add(&#x27;summary&#x27;, f&quot;Day {day_num} Itinerary&quot;)
            event.add(&#x27;description&#x27;, day_content.strip())
            
            # Make it an all-day event
            event.add(&#x27;dtstart&#x27;, current_date.date())
            event.add(&#x27;dtend&#x27;, current_date.date())
            event.add(&quot;dtstamp&quot;, datetime.now())
            cal.add_component(event)

    return cal.to_ical()


# Set up the Streamlit app
st.title(&quot;AI Travel Planner using Llama-3.2 &quot;)
st.caption(&quot;Plan your next adventure with AI Travel Planner by researching and planning a personalized itinerary on autopilot using local Llama-3&quot;)

# Initialize session state to store the generated itinerary
if &#x27;itinerary&#x27; not in st.session_state:
    st.session_state.itinerary = None

# Get SerpAPI key from the user
serp_api_key = st.text_input(&quot;Enter Serp API Key for Search functionality&quot;, type=&quot;password&quot;)

if serp_api_key:
    researcher = Agent(
        name=&quot;Researcher&quot;,
        role=&quot;Searches for travel destinations, activities, and accommodations based on user preferences&quot;,
        model=Ollama(id=&quot;llama3.2&quot;),
        description=dedent(
            &quot;&quot;&quot;\
        You are a world-class travel researcher. Given a travel destination and the number of days the user wants to travel for,
        generate a list of search terms for finding relevant travel activities and accommodations.
        Then search the web for each term, analyze the results, and return the 10 most relevant results.
        &quot;&quot;&quot;
        ),
        instructions=[
            &quot;Given a travel destination and the number of days the user wants to travel for, first generate a list of 3 search terms related to that destination and the number of days.&quot;,
            &quot;For each search term, `search_google` and analyze the results.&quot;
            &quot;From the results of all searches, return the 10 most relevant results to the user&#x27;s preferences.&quot;,
            &quot;Remember: the quality of the results is important.&quot;,
        ],
        tools=[SerpApiTools(api_key=serp_api_key)],
        add_datetime_to_instructions=True,
    )
    planner = Agent(
        name=&quot;Planner&quot;,
        role=&quot;Generates a draft itinerary based on user preferences and research results&quot;,
        model=Ollama(id=&quot;llama3.2&quot;),
        description=dedent(
            &quot;&quot;&quot;\
        You are a senior travel planner. Given a travel destination, the number of days the user wants to travel for, and a list of research results,
        your goal is to generate a draft itinerary that meets the user&#x27;s needs and preferences.
        &quot;&quot;&quot;
        ),
        instructions=[
            &quot;Given a travel destination, the number of days the user wants to travel for, and a list of research results, generate a draft itinerary that includes suggested activities and accommodations.&quot;,
            &quot;Ensure the itinerary is well-structured, informative, and engaging.&quot;,
            &quot;Ensure you provide a nuanced and balanced itinerary, quoting facts where possible.&quot;,
            &quot;Remember: the quality of the itinerary is important.&quot;,
            &quot;Focus on clarity, coherence, and overall quality.&quot;,
            &quot;Never make up facts or plagiarize. Always provide proper attribution.&quot;,
        ],
        add_datetime_to_instructions=True,
    )

    # Input fields for the user&#x27;s destination and the number of days they want to travel for
    destination = st.text_input(&quot;Where do you want to go?&quot;)
    num_days = st.number_input(&quot;How many days do you want to travel for?&quot;, min_value=1, max_value=30, value=7)

    col1, col2 = st.columns(2)
    
    with col1:
        if st.button(&quot;Generate Itinerary&quot;):
            with st.spinner(&quot;Processing...&quot;):
                # Get the response from the assistant
                response = planner.run(f&quot;{destination} for {num_days} days&quot;, stream=False)
                # Store the response in session state
                st.session_state.itinerary = response.content
                st.write(response.content)
    
    # Only show download button if there&#x27;s an itinerary
    with col2:
        if st.session_state.itinerary:
            # Generate the ICS file
            ics_content = generate_ics_content(st.session_state.itinerary)
            
            # Provide the file for download
            st.download_button(
                label=&quot;Download Itinerary as Calendar (.ics)&quot;,
                data=ics_content,
                file_name=&quot;travel_itinerary.ics&quot;,
                mime=&quot;text/calendar&quot;
            )
```

```
from textwrap import dedent
from agno.agent import Agent
from agno.tools.serpapi import SerpApiTools
import streamlit as st
import re
from agno.models.openai import OpenAIChat
from icalendar import Calendar, Event
from datetime import datetime, timedelta


def generate_ics_content(plan_text:str, start_date: datetime = None) -&gt; bytes:
    &quot;&quot;&quot;
        Generate an ICS calendar file from a travel itinerary text.

        Args:
            plan_text: The travel itinerary text
            start_date: Optional start date for the itinerary (defaults to today)

        Returns:
            bytes: The ICS file content as bytes
        &quot;&quot;&quot;
    cal = Calendar()
    cal.add(&#x27;prodid&#x27;,&#x27;-//AI Travel Planner//github.com//&#x27; )
    cal.add(&#x27;version&#x27;, &#x27;2.0&#x27;)

    if start_date is None:
        start_date = datetime.today()

    # Split the plan into days
    day_pattern = re.compile(r&#x27;Day (\d+)[:\s]+(.*?)(?=Day \d+|$)&#x27;, re.DOTALL)
    days = day_pattern.findall(plan_text)

    if not days: # If no day pattern found, create a single all-day event with the entire content
        event = Event()
        event.add(&#x27;summary&#x27;, &quot;Travel Itinerary&quot;)
        event.add(&#x27;description&#x27;, plan_text)
        event.add(&#x27;dtstart&#x27;, start_date.date())
        event.add(&#x27;dtend&#x27;, start_date.date())
        event.add(&quot;dtstamp&quot;, datetime.now())
        cal.add_component(event)  
    else:
        # Process each day
        for day_num, day_content in days:
            day_num = int(day_num)
            current_date = start_date + timedelta(days=day_num - 1)
            
            # Create a single event for the entire day
            event = Event()
            event.add(&#x27;summary&#x27;, f&quot;Day {day_num} Itinerary&quot;)
            event.add(&#x27;description&#x27;, day_content.strip())
            
            # Make it an all-day event
            event.add(&#x27;dtstart&#x27;, current_date.date())
            event.add(&#x27;dtend&#x27;, current_date.date())
            event.add(&quot;dtstamp&quot;, datetime.now())
            cal.add_component(event)

    return cal.to_ical()

# Set up the Streamlit app
st.title(&quot;AI Travel Planner &quot;)
st.caption(&quot;Plan your next adventure with AI Travel Planner by researching and planning a personalized itinerary on autopilot using GPT-4o&quot;)

# Initialize session state to store the generated itinerary
if &#x27;itinerary&#x27; not in st.session_state:
    st.session_state.itinerary = None

# Get OpenAI API key from user
openai_api_key = st.text_input(&quot;Enter OpenAI API Key to access GPT-4o&quot;, type=&quot;password&quot;)

# Get SerpAPI key from the user
serp_api_key = st.text_input(&quot;Enter Serp API Key for Search functionality&quot;, type=&quot;password&quot;)

if openai_api_key and serp_api_key:
    researcher = Agent(
        name=&quot;Researcher&quot;,
        role=&quot;Searches for travel destinations, activities, and accommodations based on user preferences&quot;,
        model=OpenAIChat(id=&quot;gpt-4o&quot;, api_key=openai_api_key),
        description=dedent(
            &quot;&quot;&quot;\
        You are a world-class travel researcher. Given a travel destination and the number of days the user wants to travel for,
        generate a list of search terms for finding relevant travel activities and accommodations.
        Then search the web for each term, analyze the results, and return the 10 most relevant results.
        &quot;&quot;&quot;
        ),
        instructions=[
            &quot;Given a travel destination and the number of days the user wants to travel for, first generate a list of 3 search terms related to that destination and the number of days.&quot;,
            &quot;For each search term, `search_google` and analyze the results.&quot;
            &quot;From the results of all searches, return the 10 most relevant results to the user&#x27;s preferences.&quot;,
            &quot;Remember: the quality of the results is important.&quot;,
        ],
        tools=[SerpApiTools(api_key=serp_api_key)],
        add_datetime_to_instructions=True,
    )
    planner = Agent(
        name=&quot;Planner&quot;,
        role=&quot;Generates a draft itinerary based on user preferences and research results&quot;,
        model=OpenAIChat(id=&quot;gpt-4o&quot;, api_key=openai_api_key),
        description=dedent(
            &quot;&quot;&quot;\
        You are a senior travel planner. Given a travel destination, the number of days the user wants to travel for, and a list of research results,
        your goal is to generate a draft itinerary that meets the user&#x27;s needs and preferences.
        &quot;&quot;&quot;
        ),
        instructions=[
            &quot;Given a travel destination, the number of days the user wants to travel for, and a list of research results, generate a draft itinerary that includes suggested activities and accommodations.&quot;,
            &quot;Ensure the itinerary is well-structured, informative, and engaging.&quot;,
            &quot;Ensure you provide a nuanced and balanced itinerary, quoting facts where possible.&quot;,
            &quot;Remember: the quality of the itinerary is important.&quot;,
            &quot;Focus on clarity, coherence, and overall quality.&quot;,
            &quot;Never make up facts or plagiarize. Always provide proper attribution.&quot;,
        ],
        add_datetime_to_instructions=True,
    )

    # Input fields for the user&#x27;s destination and the number of days they want to travel for
    destination = st.text_input(&quot;Where do you want to go?&quot;)
    num_days = st.number_input(&quot;How many days do you want to travel for?&quot;, min_value=1, max_value=30, value=7)

    col1, col2 = st.columns(2)

    with col1:
        if st.button(&quot;Generate Itinerary&quot;):
            with st.spinner(&quot;Researching your destination...&quot;):
                # First get research results
                research_results = researcher.run(f&quot;Research {destination} for a {num_days} day trip&quot;, stream=False)

                # Show research progress
                st.write(&quot; Research completed&quot;)
                
            with st.spinner(&quot;Creating your personalized itinerary...&quot;):
                # Pass research results to planner
                prompt = f&quot;&quot;&quot;
                Destination: {destination}
                Duration: {num_days} days
                Research Results: {research_results.content}
                
                Please create a detailed itinerary based on this research.
                &quot;&quot;&quot;
                response = planner.run(prompt, stream=False)
                # Store the response in session state
                st.session_state.itinerary = response.content
                st.write(response.content)
    
    # Only show download button if there&#x27;s an itinerary
    with col2:
        if st.session_state.itinerary:
            # Generate the ICS file
            ics_content = generate_ics_content(st.session_state.itinerary)
            
            # Provide the file for download
            st.download_button(
                label=&quot;Download Itinerary as Calendar (.ics)&quot;,
                data=ics_content,
                file_name=&quot;travel_itinerary.ics&quot;,
                mime=&quot;text/calendar&quot;
            )
```