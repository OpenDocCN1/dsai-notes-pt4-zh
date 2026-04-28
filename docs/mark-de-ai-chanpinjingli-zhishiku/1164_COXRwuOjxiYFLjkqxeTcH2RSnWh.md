# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/NJjddOS05oC2kVxFEJ2cYOB5nKe](https://qqs7y1hozd1.feishu.cn/docx/NJjddOS05oC2kVxFEJ2cYOB5nKe)

```
import streamlit as st
from agno.agent import Agent
from agno.tools.duckduckgo import DuckDuckGoTools
from agno.models.anthropic import Claude
from agno.tools.newspaper4k import Newspaper4kTools
from agno.tools import Tool
import logging

logging.basicConfig(level=logging.DEBUG)

# Setting up Streamlit app
st.title(&quot;AI Startup Trend Analysis Agent 📈&quot;)
st.caption(&quot;Get the latest trend analysis and startup opportunities based on your topic of interest in a click!.&quot;)

topic = st.text_input(&quot;Enter the area of interest for your Startup:&quot;)
anthropic_api_key = st.sidebar.text_input(&quot;Enter Anthropic API Key&quot;, type=&quot;password&quot;)

if st.button(&quot;Generate Analysis&quot;):
    if not anthropic_api_key:
        st.warning(&quot;Please enter the required API key.&quot;)
    else:
        with st.spinner(&quot;Processing your request...&quot;):
            try:
                # Initialize Anthropic model
                anthropic_model = Claude(id =&quot;claude-3-5-sonnet-20240620&quot;,api_key=anthropic_api_key)

                # Define News Collector Agent - Duckduckgo_search tool enables an Agent to search the web for information.
                search_tool = DuckDuckGoTools(search=True, news=True, fixed_max_results=5)
                news_collector = Agent(
                    name=&quot;News Collector&quot;,
                    role=&quot;Collects recent news articles on the given topic&quot;,
                    tools=[search_tool],
                    model=anthropic_model,
                    instructions=[&quot;Gather latest articles on the topic&quot;],
                    show_tool_calls=True,
                    markdown=True,
                )

                # Define Summary Writer Agent
                news_tool = Newspaper4kTools(read_article=True, include_summary=True)
                summary_writer = Agent(
                    name=&quot;Summary Writer&quot;,
                    role=&quot;Summarizes collected news articles&quot;,
                    tools=[news_tool],
                    model=anthropic_model,
                    instructions=[&quot;Provide concise summaries of the articles&quot;],
                    show_tool_calls=True,
                    markdown=True,
                )

                # Define Trend Analyzer Agent
                trend_analyzer = Agent(
                    name=&quot;Trend Analyzer&quot;,
                    role=&quot;Analyzes trends from summaries&quot;,
                    model=anthropic_model,
                    instructions=[&quot;Identify emerging trends and startup opportunities&quot;],
                    show_tool_calls=True,
                    markdown=True,
                )

                # The multi agent Team setup of phidata:
                agent_team = Agent(
                    agents=[news_collector, summary_writer, trend_analyzer],
                    instructions=[
                        &quot;First, search DuckDuckGo for recent news articles related to the user&#x27;s specified topic.&quot;,
                        &quot;Then, provide the collected article links to the summary writer.&quot;,
                        &quot;Important: you must ensure that the summary writer receives all the article links to read.&quot;,
                        &quot;Next, the summary writer will read the articles and prepare concise summaries of each.&quot;,
                        &quot;After summarizing, the summaries will be passed to the trend analyzer.&quot;,
                        &quot;Finally, the trend analyzer will identify emerging trends and potential startup opportunities based on the summaries provided in a detailed Report form so that any young entreprenur can get insane value reading this easily&quot;
                    ],
                    show_tool_calls=True,
                    markdown=True,
                )

                # Executing the workflow
                # Step 1: Collect news
                news_response = news_collector.run(f&quot;Collect recent news on {topic}&quot;)
                articles = news_response.content

                # Step 2: Summarize articles
                summary_response = summary_writer.run(f&quot;Summarize the following articles:\n{articles}&quot;)
                summaries = summary_response.content

                # Step 3: Analyze trends
                trend_response = trend_analyzer.run(f&quot;Analyze trends from the following summaries:\n{summaries}&quot;)
                analysis = trend_response.content

                # Display results - if incase you want to use this furthur, you can uncomment the below 2 lines to get the summaries too!
                # st.subheader(&quot;News Summaries&quot;)
                # # st.write(summaries)

                st.subheader(&quot;Trend Analysis and Potential Startup Opportunities&quot;)
                st.write(analysis)

            except Exception as e:
                st.error(f&quot;An error occurred: {e}&quot;)
else:
    st.info(&quot;Enter the topic and API keys, then click &#x27;Generate Analysis&#x27; to start.&quot;)
```