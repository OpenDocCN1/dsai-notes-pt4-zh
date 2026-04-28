# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/KbYjdSYTKos7eexYJRJcQ7XYnJf](https://qqs7y1hozd1.feishu.cn/docx/KbYjdSYTKos7eexYJRJcQ7XYnJf)

```
import streamlit as st
from duckduckgo_search import DDGS
from swarm import Swarm, Agent
from datetime import datetime
from dotenv import load_dotenv

load_dotenv()
MODEL = &quot;llama3.2:latest&quot;
client = Swarm()

st.set_page_config(page_title=&quot;AI News Processor&quot;, page_icon=&quot;📰&quot;)
st.title(&quot;📰 News Inshorts Agent&quot;)

def search_news(topic):
    &quot;&quot;&quot;Search for news articles using DuckDuckGo&quot;&quot;&quot;
    with DDGS() as ddg:
        results = ddg.text(f&quot;{topic} news {datetime.now().strftime(&#x27;%Y-%m&#x27;)}&quot;, max_results=3)
        if results:
            news_results = &quot;\n\n&quot;.join([
                f&quot;Title: {result[&#x27;title&#x27;]}\nURL: {result[&#x27;href&#x27;]}\nSummary: {result[&#x27;body&#x27;]}&quot; 
                for result in results
            ])
            return news_results
        return f&quot;No news found for {topic}.&quot;

# Create specialized agents
search_agent = Agent(
    name=&quot;News Searcher&quot;,
    instructions=&quot;&quot;&quot;
    You are a news search specialist. Your task is to:
    1. Search for the most relevant and recent news on the given topic
    2. Ensure the results are from reputable sources
    3. Return the raw search results in a structured format
    &quot;&quot;&quot;,
    functions=[search_news],
    model=MODEL
)

synthesis_agent = Agent(
    name=&quot;News Synthesizer&quot;,
    instructions=&quot;&quot;&quot;
    You are a news synthesis expert. Your task is to:
    1. Analyze the raw news articles provided
    2. Identify the key themes and important information
    3. Combine information from multiple sources
    4. Create a comprehensive but concise synthesis
    5. Focus on facts and maintain journalistic objectivity
    6. Write in a clear, professional style
    Provide a 2-3 paragraph synthesis of the main points.
    &quot;&quot;&quot;,
    model=MODEL
)

summary_agent = Agent(
    name=&quot;News Summarizer&quot;,
    instructions=&quot;&quot;&quot;
    You are an expert news summarizer combining AP and Reuters style clarity with digital-age brevity.

    Your task:
    1. Core Information:
       - Lead with the most newsworthy development
       - Include key stakeholders and their actions
       - Add critical numbers/data if relevant
       - Explain why this matters now
       - Mention immediate implications

    2. Style Guidelines:
       - Use strong, active verbs
       - Be specific, not general
       - Maintain journalistic objectivity
       - Make every word count
       - Explain technical terms if necessary

    Format: Create a single paragraph of 250-400 words that informs and engages.
    Pattern: [Major News] + [Key Details/Data] + [Why It Matters/What&#x27;s Next]

    Focus on answering: What happened? Why is it significant? What&#x27;s the impact?

    IMPORTANT: Provide ONLY the summary paragraph. Do not include any introductory phrases, 
    labels, or meta-text like &quot;Here&#x27;s a summary&quot; or &quot;In AP/Reuters style.&quot;
    Start directly with the news content.
    &quot;&quot;&quot;,
    model=MODEL
)

def process_news(topic):
    &quot;&quot;&quot;Run the news processing workflow&quot;&quot;&quot;
    with st.status(&quot;Processing news...&quot;, expanded=True) as status:
        # Search
        status.write(&quot;🔍 Searching for news...&quot;)
        search_response = client.run(
            agent=search_agent,
            messages=[{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: f&quot;Find recent news about {topic}&quot;}]
        )
        raw_news = search_response.messages[-1][&quot;content&quot;]
        
        # Synthesize
        status.write(&quot;🔄 Synthesizing information...&quot;)
        synthesis_response = client.run(
            agent=synthesis_agent,
            messages=[{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: f&quot;Synthesize these news articles:\n{raw_news}&quot;}]
        )
        synthesized_news = synthesis_response.messages[-1][&quot;content&quot;]
        
        # Summarize
        status.write(&quot;📝 Creating summary...&quot;)
        summary_response = client.run(
            agent=summary_agent,
            messages=[{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: f&quot;Summarize this synthesis:\n{synthesized_news}&quot;}]
        )
        return raw_news, synthesized_news, summary_response.messages[-1][&quot;content&quot;]

# User Interface
topic = st.text_input(&quot;Enter news topic:&quot;, value=&quot;artificial intelligence&quot;)
if st.button(&quot;Process News&quot;, type=&quot;primary&quot;):
    if topic:
        try:
            raw_news, synthesized_news, final_summary = process_news(topic)
            st.header(f&quot;📝 News Summary: {topic}&quot;)
            st.markdown(final_summary)
        except Exception as e:
            st.error(f&quot;An error occurred: {str(e)}&quot;)
    else:
        st.error(&quot;Please enter a topic!&quot;)
```