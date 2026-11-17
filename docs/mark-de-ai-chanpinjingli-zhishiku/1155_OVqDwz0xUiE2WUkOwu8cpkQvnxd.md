# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/QjbPdcgbFoE1Kpx8J4wcyQv9nVc](https://qqs7y1hozd1.feishu.cn/docx/QjbPdcgbFoE1Kpx8J4wcyQv9nVc)

        return f&quot;https://i.imgflip.com/{meme_id}.jpg&quot;

```
import asyncio
import streamlit as st
from browser_use import Agent, SystemPrompt
from langchain_openai import ChatOpenAI
from langchain_anthropic import ChatAnthropic
from langchain_core.messages import HumanMessage
import re

async def generate_meme(query: str, model_choice: str, api_key: str) -&gt; None:
    # Initialize the appropriate LLM based on user selection
    if model_choice == &quot;Claude&quot;:
        llm = ChatAnthropic(
            model=&quot;claude-3-5-sonnet-20241022&quot;,
            api_key=api_key
        )
    elif model_choice == &quot;Deepseek&quot;:
        llm = ChatOpenAI(
            base_url=&#x27;https://api.deepseek.com/v1&#x27;,
            model=&#x27;deepseek-chat&#x27;,
            api_key=api_key,
            temperature=0.3
        )
    else:  # OpenAI
        llm = ChatOpenAI(
            model=&quot;gpt-4o&quot;,
            api_key=api_key,
            temperature=0.0
        )

    task_description = (
        &quot;You are a meme generator expert. You are given a query and you need to generate a meme for it.\n&quot;
        &quot;1. Go to https://imgflip.com/memetemplates \n&quot;
        &quot;2. Click on the Search bar in the middle and search for ONLY ONE MAIN ACTION VERB (like &#x27;bully&#x27;, &#x27;laugh&#x27;, &#x27;cry&#x27;) in this query: &#x27;{0}&#x27;\n&quot;
        &quot;3. Choose any meme template that metaphorically fits the meme topic: &#x27;{0}&#x27;\n&quot;
        &quot;   by clicking on the &#x27;Add Caption&#x27; button below it\n&quot;
        &quot;4. Write a Top Text (setup/context) and Bottom Text (punchline/outcome) related to &#x27;{0}&#x27;.\n&quot; 
        &quot;5. Check the preview making sure it is funny and a meaningful meme. Adjust text directly if needed. \n&quot;
        &quot;6. Look at the meme and text on it, if it doesnt make sense, PLEASE retry by filling the text boxes with different text. \n&quot;
        &quot;7. Click on the Generate meme button to generate the meme\n&quot;
        &quot;8. Copy the image link and give it as the output\n&quot;
    ).format(query)

    agent = Agent(
        task=task_description,
        llm=llm,
        max_actions_per_step=5,
        max_failures=25,
        use_vision=(model_choice != &quot;Deepseek&quot;)
    )

    history = await agent.run()
    
    # Extract final result from agent history
    final_result = history.final_result()
    
    # Use regex to find the meme URL in the result
    url_match = re.search(r&#x27;https://imgflip\.com/i/(\w+)&#x27;, final_result)
    if url_match:
        meme_id = url_match.group(1)
    return None

def main():
    # Custom CSS styling

    st.title(&quot;🥸 AI Meme Generator Agent - Browser Use&quot;)
    st.info(&quot;This AI browser agent does browser automation to generate memes based on your input with browser use. Please enter your API key and describe the meme you want to generate.&quot;)
    
    # Sidebar configuration
    with st.sidebar:
        st.markdown(&#x27;&lt;p class=&quot;sidebar-header&quot;&gt;⚙️ Model Configuration&lt;/p&gt;&#x27;, unsafe_allow_html=True)
        
        # Model selection
        model_choice = st.selectbox(
            &quot;Select AI Model&quot;,
            [&quot;Claude&quot;, &quot;Deepseek&quot;, &quot;OpenAI&quot;],
            index=0,
            help=&quot;Choose which LLM to use for meme generation&quot;
        )
        
        # API key input based on model selection
        api_key = &quot;&quot;
        if model_choice == &quot;Claude&quot;:
            api_key = st.text_input(&quot;Claude API Key&quot;, type=&quot;password&quot;, 
                                  help=&quot;Get your API key from https://console.anthropic.com&quot;)
        elif model_choice == &quot;Deepseek&quot;:
            api_key = st.text_input(&quot;Deepseek API Key&quot;, type=&quot;password&quot;,
                                  help=&quot;Get your API key from https://platform.deepseek.com&quot;)
        else:
            api_key = st.text_input(&quot;OpenAI API Key&quot;, type=&quot;password&quot;,
                                  help=&quot;Get your API key from https://platform.openai.com&quot;)

    # Main content area
    st.markdown(&#x27;&lt;p class=&quot;header-text&quot;&gt;🎨 Describe Your Meme Concept&lt;/p&gt;&#x27;, unsafe_allow_html=True)
    
    query = st.text_input(
        &quot;Meme Idea Input&quot;,
        placeholder=&quot;Example: &#x27;Ilya&#x27;s SSI quietly looking at the OpenAI vs Deepseek debate while diligently working on ASI&#x27;&quot;,
        label_visibility=&quot;collapsed&quot;
    )

    if st.button(&quot;Generate Meme 🚀&quot;):
        if not api_key:
            st.warning(f&quot;Please provide the {model_choice} API key&quot;)
            st.stop()
        if not query:
            st.warning(&quot;Please enter a meme idea&quot;)
            st.stop()

        with st.spinner(f&quot;🧠 {model_choice} is generating your meme...&quot;):
            try:
                meme_url = asyncio.run(generate_meme(query, model_choice, api_key))
                
                if meme_url:
                    st.success(&quot;✅ Meme Generated Successfully!&quot;)
                    st.image(meme_url, caption=&quot;Generated Meme Preview&quot;, use_container_width=True)
                    st.markdown(f&quot;&quot;&quot;
                        **Direct Link:** [Open in ImgFlip]({meme_url})  
                        **Embed URL:** `{meme_url}`
                    &quot;&quot;&quot;)
                else:
                    st.error(&quot;❌ Failed to generate meme. Please try again with a different prompt.&quot;)
                    
            except Exception as e:
                st.error(f&quot;Error: {str(e)}&quot;)
                st.info(&quot;💡 If using OpenAI, ensure your account has GPT-4o access&quot;)

if __name__ == &#x27;__main__&#x27;:
    main()
```