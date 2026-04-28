# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/PqrNdK1yooXVMXxphMMc2nAKn4c](https://qqs7y1hozd1.feishu.cn/docx/PqrNdK1yooXVMXxphMMc2nAKn4c)

```
import re
import asyncio
from textwrap import dedent
from agno.agent import Agent
from agno.tools.mcp import MultiMCPTools
from agno.tools.googlesearch import GoogleSearchTools
from agno.models.openai import OpenAIChat
from icalendar import Calendar, Event
from datetime import datetime, timedelta
import streamlit as st
from datetime import date
import os

def generate_ics_content(plan_text: str, start_date: datetime = None) -&gt; bytes:
    &quot;&quot;&quot;
    Generate an ICS calendar file from a travel itinerary text.

    Args:
        plan_text: The travel itinerary text
        start_date: Optional start date for the itinerary (defaults to today)

    Returns:
        bytes: The ICS file content as bytes
    &quot;&quot;&quot;
    cal = Calendar()
    cal.add(&#x27;prodid&#x27;,&#x27;-//AI Travel Planner//github.com//&#x27;)
    cal.add(&#x27;version&#x27;, &#x27;2.0&#x27;)

    if start_date is None:
        start_date = datetime.today()

    # Split the plan into days
    day_pattern = re.compile(r&#x27;Day (\d+)[:\s]+(.*?)(?=Day \d+|$)&#x27;, re.DOTALL)
    days = day_pattern.findall(plan_text)

    if not days:  # If no day pattern found, create a single all-day event with the entire content
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

async def run_mcp_travel_planner(destination: str, num_days: int, preferences: str, budget: int, openai_key: str, google_maps_key: str):
    &quot;&quot;&quot;Run the MCP-based travel planner agent with real-time data access.&quot;&quot;&quot;

    try:
        # Set Google Maps API key environment variable
        os.environ[&quot;GOOGLE_MAPS_API_KEY&quot;] = google_maps_key

        # Initialize MCPTools with Airbnb MCP
        mcp_tools = MultiMCPTools(
            [
            &quot;npx -y @openbnb/mcp-server-airbnb --ignore-robots-txt&quot;,
            &quot;npx @gongrzhe/server-travelplanner-mcp&quot;,
            ],      
            env={
                &quot;GOOGLE_MAPS_API_KEY&quot;: google_maps_key,
            },
            timeout_seconds=60,
        )   

        # Connect to Airbnb MCP server
        await mcp_tools.connect()


        travel_planner = Agent(
            name=&quot;Travel Planner&quot;,
            role=&quot;Creates travel itineraries using Airbnb, Google Maps, and Google Search&quot;,
            model=OpenAIChat(id=&quot;gpt-4o&quot;, api_key=openai_key),
            description=dedent(
                &quot;&quot;&quot;\
                You are a professional travel consultant AI that creates highly detailed travel itineraries directly without asking questions.

                You have access to:
                🏨 Airbnb listings with real availability and current pricing
                🗺️ Google Maps MCP for location services, directions, distance calculations, and local navigation
                🔍 Web search capabilities for current information, reviews, and travel updates

                ALWAYS create a complete, detailed itinerary immediately without asking for clarification or additional information.
                Use Google Maps MCP extensively to calculate distances between all locations and provide precise travel times.
                If information is missing, use your best judgment and available tools to fill in the gaps.
                &quot;&quot;&quot;
            ),
            instructions=[
                &quot;IMPORTANT: Never ask questions or request clarification - always generate a complete itinerary&quot;,
                &quot;Research the destination thoroughly using all available tools to gather comprehensive current information&quot;,
                &quot;Find suitable accommodation options within the budget using Airbnb MCP with real prices and availability&quot;,
                &quot;Create an extremely detailed day-by-day itinerary with specific activities, locations, exact timing, and distances&quot;,
                &quot;Use Google Maps MCP extensively to calculate distances between ALL locations and provide travel times&quot;,
                &quot;Include detailed transportation options and turn-by-turn navigation tips using Google Maps MCP&quot;,
                &quot;Research dining options with specific restaurant names, addresses, price ranges, and distance from accommodation&quot;,
                &quot;Check current weather conditions, seasonal factors, and provide detailed packing recommendations&quot;,
                &quot;Calculate precise estimated costs for EVERY aspect of the trip and ensure recommendations fit within budget&quot;,
                &quot;Include detailed information about each attraction: opening hours, ticket prices, best visiting times, and distance from accommodation&quot;,
                &quot;Add practical information including local transportation costs, currency exchange, safety tips, and cultural norms&quot;,
                &quot;Structure the itinerary with clear sections, detailed timing for each activity, and include buffer time between activities&quot;,
                &quot;Use all available tools proactively without asking for permission&quot;,
                &quot;Generate the complete, detailed itinerary in one response without follow-up questions&quot;
            ],
            tools=[mcp_tools, GoogleSearchTools()],
            add_datetime_to_instructions=True,
            markdown=True,
            show_tool_calls=False,
        )

        # Create the planning prompt
        prompt = f&quot;&quot;&quot;
        IMMEDIATELY create an extremely detailed and comprehensive travel itinerary for:

        **Destination:** {destination}
        **Duration:** {num_days} days
        **Budget:** ${budget} USD total
        **Preferences:** {preferences}

        DO NOT ask any questions. Generate a complete, highly detailed itinerary now using all available tools.

        **CRITICAL REQUIREMENTS:**
        - Use Google Maps MCP to calculate distances and travel times between ALL locations
        - Include specific addresses for every location, restaurant, and attraction
        - Provide detailed timing for each activity with buffer time between locations
        - Calculate precise costs for transportation between each location
        - Include opening hours, ticket prices, and best visiting times for all attractions
        - Provide detailed weather information and specific packing recommendations

        **REQUIRED OUTPUT FORMAT:**
        1. **Trip Overview** - Summary, total estimated cost breakdown, detailed weather forecast
        2. **Accommodation** - 3 specific Airbnb options with real prices, addresses, amenities, and distance from city center
        3. **Transportation Overview** - Detailed transportation options, costs, and recommendations
        4. **Day-by-Day Itinerary** - Extremely detailed schedule with:
           - Specific start/end times for each activity
           - Exact distances and travel times between locations (use Google Maps MCP)
           - Detailed descriptions of each location with addresses
           - Opening hours, ticket prices, and best visiting times
           - Estimated costs for each activity and transportation
           - Buffer time between activities for unexpected delays
        5. **Dining Plan** - Specific restaurants with addresses, price ranges, cuisine types, and distance from accommodation
        6. **Detailed Practical Information**:
           - Weather forecast with clothing recommendations
           - Currency exchange rates and costs
           - Local transportation options and costs
           - Safety information and emergency contacts
           - Cultural norms and etiquette tips
           - Communication options (SIM cards, WiFi, etc.)
           - Health and medical considerations
           - Shopping and souvenir recommendations

        Use Airbnb MCP for real accommodation data, Google Maps MCP for ALL distance calculations and location services, and web search for current information.
        Make reasonable assumptions and fill in any gaps with your knowledge.
        Generate the complete, highly detailed itinerary in one response without asking for clarification.
        &quot;&quot;&quot;

        response = await travel_planner.arun(prompt)
        return response.content

    finally:
        await mcp_tools.close()

def run_travel_planner(destination: str, num_days: int, preferences: str, budget: int, openai_key: str, google_maps_key: str):
    &quot;&quot;&quot;Synchronous wrapper for the async MCP travel planner.&quot;&quot;&quot;
    return asyncio.run(run_mcp_travel_planner(destination, num_days, preferences, budget, openai_key, google_maps_key))
    
# -------------------- Streamlit App --------------------
    
# Configure the page
st.set_page_config(
    page_title=&quot;MCP AI Travel Planner&quot;,
    page_icon=&quot;✈️&quot;,
    layout=&quot;wide&quot;
)

# Initialize session state
if &#x27;itinerary&#x27; not in st.session_state:
    st.session_state.itinerary = None

# Title and description
st.title(&quot;✈️ MCP AI Travel Planner&quot;)
st.caption(&quot;Plan your next adventure with AI Travel Planner using MCP servers for real-time data access&quot;)

# Sidebar for API keys
with st.sidebar:
    st.header(&quot;🔑 API Keys Configuration&quot;)
    st.warning(&quot;⚠️ These services require API keys:&quot;)

    openai_api_key = st.text_input(&quot;OpenAI API Key&quot;, type=&quot;password&quot;, help=&quot;Required for AI planning&quot;)
    google_maps_key = st.text_input(&quot;Google Maps API Key&quot;, type=&quot;password&quot;, help=&quot;Required for location services&quot;)

    # Check if API keys are provided (both OpenAI and Google Maps are required)
    api_keys_provided = openai_api_key and google_maps_key

    if api_keys_provided:
        st.success(&quot;✅ All API keys configured!&quot;)
    else:
        st.warning(&quot;⚠️ Please enter both API keys to use the travel planner.&quot;)
        st.info(&quot;&quot;&quot;
        **Required API Keys:**
        - **OpenAI API Key**: https://platform.openai.com/api-keys
        - **Google Maps API Key**: https://console.cloud.google.com/apis/credentials (for location services)
        &quot;&quot;&quot;)

# Main content (only shown if API keys are provided)
if api_keys_provided:
    # Main input section
    st.header(&quot;🌍 Trip Details&quot;)

    col1, col2 = st.columns(2)

    with col1:
        destination = st.text_input(&quot;Destination&quot;, placeholder=&quot;e.g., Paris, Tokyo, New York&quot;)
        num_days = st.number_input(&quot;Number of Days&quot;, min_value=1, max_value=30, value=7)

    with col2:
        budget = st.number_input(&quot;Budget (USD)&quot;, min_value=100, max_value=10000, step=100, value=2000)
        start_date = st.date_input(&quot;Start Date&quot;, min_value=date.today(), value=date.today())

    # Preferences section
    st.subheader(&quot;🎯 Travel Preferences&quot;)
    preferences_input = st.text_area(
        &quot;Describe your travel preferences&quot;,
        placeholder=&quot;e.g., adventure activities, cultural sites, food, relaxation, nightlife...&quot;,
        height=100
    )

    # Quick preference buttons
    quick_prefs = st.multiselect(
        &quot;Quick Preferences (optional)&quot;,
        [&quot;Adventure&quot;, &quot;Relaxation&quot;, &quot;Sightseeing&quot;, &quot;Cultural Experiences&quot;,
         &quot;Beach&quot;, &quot;Mountain&quot;, &quot;Luxury&quot;, &quot;Budget-Friendly&quot;, &quot;Food &amp; Dining&quot;,
         &quot;Shopping&quot;, &quot;Nightlife&quot;, &quot;Family-Friendly&quot;],
        help=&quot;Select multiple preferences or describe in detail above&quot;
    )

    # Combine preferences
    all_preferences = []
    if preferences_input:
        all_preferences.append(preferences_input)
    if quick_prefs:
        all_preferences.extend(quick_prefs)

    preferences = &quot;, &quot;.join(all_preferences) if all_preferences else &quot;General sightseeing&quot;

    # Generate button
    col1, col2 = st.columns([1, 1])

    with col1:
        if st.button(&quot;🎯 Generate Itinerary&quot;, type=&quot;primary&quot;):
            if not destination:
                st.error(&quot;Please enter a destination.&quot;)
            elif not preferences:
                st.warning(&quot;Please describe your preferences or select quick preferences.&quot;)
            else:
                tools_message = &quot;🏨 Connecting to Airbnb MCP&quot;
                if google_maps_key:
                    tools_message += &quot; and Google Maps MCP&quot;
                tools_message += &quot;, creating itinerary...&quot;

                with st.spinner(tools_message):
                    try:
                        # Calculate number of days from start date
                        response = run_travel_planner(
                            destination=destination,
                            num_days=num_days,
                            preferences=preferences,
                            budget=budget,
                            openai_key=openai_api_key,
                            google_maps_key=google_maps_key or &quot;&quot;
                        )

                        # Store the response in session state
                        st.session_state.itinerary = response

                        # Show MCP connection status
                        if &quot;Airbnb&quot; in response and (&quot;listing&quot; in response.lower() or &quot;accommodation&quot; in response.lower()):
                            st.success(&quot;✅ Your travel itinerary is ready with Airbnb data!&quot;)
                            st.info(&quot;🏨 Used real Airbnb listings for accommodation recommendations&quot;)
                        else:
                            st.success(&quot;✅ Your travel itinerary is ready!&quot;)
                            st.info(&quot;📝 Used general knowledge for accommodation suggestions (Airbnb MCP may have failed to connect)&quot;)

                    except Exception as e:
                        st.error(f&quot;Error: {str(e)}&quot;)
                        st.info(&quot;Please try again or check your internet connection.&quot;)

    with col2:
        if st.session_state.itinerary:
            # Generate the ICS file
            ics_content = generate_ics_content(st.session_state.itinerary, datetime.combine(start_date, datetime.min.time()))

            # Provide the file for download
            st.download_button(
                label=&quot;📅 Download as Calendar&quot;,
                data=ics_content,
                file_name=&quot;travel_itinerary.ics&quot;,
                mime=&quot;text/calendar&quot;
            )

    # Display itinerary
    if st.session_state.itinerary:
        st.header(&quot;📋 Your Travel Itinerary&quot;)
        st.markdown(st.session_state.itinerary)
```

```
streamlit
agno
openai
icalendar
google-search-results
```