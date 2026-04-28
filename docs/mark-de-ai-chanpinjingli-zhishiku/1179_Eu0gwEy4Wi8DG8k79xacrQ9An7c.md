# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/JBiPdBj4HoBXulxVmAxcN4GpnYg](https://qqs7y1hozd1.feishu.cn/docx/JBiPdBj4HoBXulxVmAxcN4GpnYg)

```
# Import the required libraries
import streamlit as st
from scrapegraphai.graphs import SmartScraperGraph

# Set up the Streamlit app
st.title(&quot;Web Scrapping AI Agent 🕵️‍♂️&quot;)
st.caption(&quot;This app allows you to scrape a website using OpenAI API&quot;)

# Get OpenAI API key from user
openai_access_token = st.text_input(&quot;OpenAI API Key&quot;, type=&quot;password&quot;)

if openai_access_token:
    model = st.radio(
        &quot;Select the model&quot;,
        [&quot;gpt-3.5-turbo&quot;, &quot;gpt-4&quot;],
        index=0,
    )    
    graph_config = {
        &quot;llm&quot;: {
            &quot;api_key&quot;: openai_access_token,
            &quot;model&quot;: model,
        },
    }
    # Get the URL of the website to scrape
    url = st.text_input(&quot;Enter the URL of the website you want to scrape&quot;)
    # Get the user prompt
    user_prompt = st.text_input(&quot;What you want the AI agent to scrae from the website?&quot;)
    
    # Create a SmartScraperGraph object
    smart_scraper_graph = SmartScraperGraph(
        prompt=user_prompt,
        source=url,
        config=graph_config
    )
    # Scrape the website
    if st.button(&quot;Scrape&quot;):
        result = smart_scraper_graph.run()
        st.write(result)
```

```
# Import the required libraries
import streamlit as st
from scrapegraphai.graphs import SmartScraperGraph

# Set up the Streamlit app
st.title(&quot;Web Scrapping AI Agent 🕵️‍♂️&quot;)
st.caption(&quot;This app allows you to scrape a website using Llama 3.2&quot;)

# Set up the configuration for the SmartScraperGraph
graph_config = {
    &quot;llm&quot;: {
        &quot;model&quot;: &quot;ollama/llama3.2&quot;,
        &quot;temperature&quot;: 0,
        &quot;format&quot;: &quot;json&quot;,  # Ollama needs the format to be specified explicitly
        &quot;base_url&quot;: &quot;http://localhost:11434&quot;,  # set Ollama URL
    },
    &quot;embeddings&quot;: {
        &quot;model&quot;: &quot;ollama/nomic-embed-text&quot;,
        &quot;base_url&quot;: &quot;http://localhost:11434&quot;,  # set Ollama URL
    },
    &quot;verbose&quot;: True,
}
# Get the URL of the website to scrape
url = st.text_input(&quot;Enter the URL of the website you want to scrape&quot;)
# Get the user prompt
user_prompt = st.text_input(&quot;What you want the AI agent to scrape from the website?&quot;)

# Create a SmartScraperGraph object
smart_scraper_graph = SmartScraperGraph(
    prompt=user_prompt,
    source=url,
    config=graph_config
)
# Scrape the website
if st.button(&quot;Scrape&quot;):
    result = smart_scraper_graph.run()
    st.write(result)
```