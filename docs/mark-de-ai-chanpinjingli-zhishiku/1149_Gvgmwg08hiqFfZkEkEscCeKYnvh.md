# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/JwD1dB5qzo3twaxIPQtcfYP3nEW](https://qqs7y1hozd1.feishu.cn/docx/JwD1dB5qzo3twaxIPQtcfYP3nEW)

```
import os
import json
import re
import sys
import io
import contextlib
import warnings
from typing import Optional, List, Any, Tuple
from PIL import Image
import streamlit as st
import pandas as pd
import base64
from io import BytesIO
from together import Together
from e2b_code_interpreter import Sandbox

warnings.filterwarnings(&quot;ignore&quot;, category=UserWarning, module=&quot;pydantic&quot;)

pattern = re.compile(r&quot;```python\n(.*?)\n```&quot;, re.DOTALL)

def code_interpret(e2b_code_interpreter: Sandbox, code: str) -&gt; Optional[List[Any]]:
    with st.spinner(&#x27;Executing code in E2B sandbox...&#x27;):
        stdout_capture = io.StringIO()
        stderr_capture = io.StringIO()

        with contextlib.redirect_stdout(stdout_capture), contextlib.redirect_stderr(stderr_capture):
            with warnings.catch_warnings():
                warnings.simplefilter(&quot;ignore&quot;)
                exec = e2b_code_interpreter.run_code(code)

        if stderr_capture.getvalue():
            print(&quot;[Code Interpreter Warnings/Errors]&quot;, file=sys.stderr)
            print(stderr_capture.getvalue(), file=sys.stderr)

        if stdout_capture.getvalue():
            print(&quot;[Code Interpreter Output]&quot;, file=sys.stdout)
            print(stdout_capture.getvalue(), file=sys.stdout)

        if exec.error:
            print(f&quot;[Code Interpreter ERROR] {exec.error}&quot;, file=sys.stderr)
            return None
        return exec.results

def match_code_blocks(llm_response: str) -&gt; str:
    match = pattern.search(llm_response)
    if match:
        code = match.group(1)
        return code
    return &quot;&quot;

def chat_with_llm(e2b_code_interpreter: Sandbox, user_message: str, dataset_path: str) -&gt; Tuple[Optional[List[Any]], str]:
    # Update system prompt to include dataset path information
    system_prompt = f&quot;&quot;&quot;You&#x27;re a Python data scientist and data visualization expert. You are given a dataset at path &#x27;{dataset_path}&#x27; and also the user&#x27;s query.
You need to analyze the dataset and answer the user&#x27;s query with a response and you run Python code to solve them.
IMPORTANT: Always use the dataset path variable &#x27;{dataset_path}&#x27; in your code when reading the CSV file.&quot;&quot;&quot;

    messages = [
        {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: system_prompt},
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: user_message},
    ]

    with st.spinner(&#x27;Getting response from Together AI LLM model...&#x27;):
        client = Together(api_key=st.session_state.together_api_key)
        response = client.chat.completions.create(
            model=st.session_state.model_name,
            messages=messages,
        )

        response_message = response.choices[0].message
        python_code = match_code_blocks(response_message.content)
        
        if python_code:
            code_interpreter_results = code_interpret(e2b_code_interpreter, python_code)
            return code_interpreter_results, response_message.content
        else:
            st.warning(f&quot;Failed to match any Python code in model&#x27;s response&quot;)
            return None, response_message.content

def upload_dataset(code_interpreter: Sandbox, uploaded_file) -&gt; str:
    dataset_path = f&quot;./{uploaded_file.name}&quot;
    
    try:
        code_interpreter.files.write(dataset_path, uploaded_file)
        return dataset_path
    except Exception as error:
        st.error(f&quot;Error during file upload: {error}&quot;)
        raise error


def main():
    &quot;&quot;&quot;Main Streamlit application.&quot;&quot;&quot;
    st.title(&quot;📊 AI Data Visualization Agent&quot;)
    st.write(&quot;Upload your dataset and ask questions about it!&quot;)

    # Initialize session state variables
    if &#x27;together_api_key&#x27; not in st.session_state:
        st.session_state.together_api_key = &#x27;&#x27;
    if &#x27;e2b_api_key&#x27; not in st.session_state:
        st.session_state.e2b_api_key = &#x27;&#x27;
    if &#x27;model_name&#x27; not in st.session_state:
        st.session_state.model_name = &#x27;&#x27;

    with st.sidebar:
        st.header(&quot;API Keys and Model Configuration&quot;)
        st.session_state.together_api_key = st.sidebar.text_input(&quot;Together AI API Key&quot;, type=&quot;password&quot;)
        st.sidebar.info(&quot;💡 Everyone gets a free $1 credit by Together AI - AI Acceleration Cloud platform&quot;)
        st.sidebar.markdown(&quot;[Get Together AI API Key](https://api.together.ai/signin)&quot;)
        
        st.session_state.e2b_api_key = st.sidebar.text_input(&quot;Enter E2B API Key&quot;, type=&quot;password&quot;)
        st.sidebar.markdown(&quot;[Get E2B API Key](https://e2b.dev/docs/legacy/getting-started/api-key)&quot;)
        
        # Add model selection dropdown
        model_options = {
            &quot;Meta-Llama 3.1 405B&quot;: &quot;meta-llama/Meta-Llama-3.1-405B-Instruct-Turbo&quot;,
            &quot;DeepSeek V3&quot;: &quot;deepseek-ai/DeepSeek-V3&quot;,
            &quot;Qwen 2.5 7B&quot;: &quot;Qwen/Qwen2.5-7B-Instruct-Turbo&quot;,
            &quot;Meta-Llama 3.3 70B&quot;: &quot;meta-llama/Llama-3.3-70B-Instruct-Turbo&quot;
        }
        st.session_state.model_name = st.selectbox(
            &quot;Select Model&quot;,
            options=list(model_options.keys()),
            index=0  # Default to first option
        )
        st.session_state.model_name = model_options[st.session_state.model_name]

    uploaded_file = st.file_uploader(&quot;Choose a CSV file&quot;, type=&quot;csv&quot;)
    
    if uploaded_file is not None:
        # Display dataset with toggle
        df = pd.read_csv(uploaded_file)
        st.write(&quot;Dataset:&quot;)
        show_full = st.checkbox(&quot;Show full dataset&quot;)
        if show_full:
            st.dataframe(df)
        else:
            st.write(&quot;Preview (first 5 rows):&quot;)
            st.dataframe(df.head())
        # Query input
        query = st.text_area(&quot;What would you like to know about your data?&quot;,
                            &quot;Can you compare the average cost for two people between different categories?&quot;)
        
        if st.button(&quot;Analyze&quot;):
            if not st.session_state.together_api_key or not st.session_state.e2b_api_key:
                st.error(&quot;Please enter both API keys in the sidebar.&quot;)
            else:
                with Sandbox(api_key=st.session_state.e2b_api_key) as code_interpreter:
                    # Upload the dataset
                    dataset_path = upload_dataset(code_interpreter, uploaded_file)
                    
                    # Pass dataset_path to chat_with_llm
                    code_results, llm_response = chat_with_llm(code_interpreter, query, dataset_path)
                    
                    # Display LLM&#x27;s text response
                    st.write(&quot;AI Response:&quot;)
                    st.write(llm_response)
                    
                    # Display results/visualizations
                    if code_results:
                        for result in code_results:
                            if hasattr(result, &#x27;png&#x27;) and result.png:  # Check if PNG data is available
                                # Decode the base64-encoded PNG data
                                png_data = base64.b64decode(result.png)
                                
                                # Convert PNG data to an image and display it
                                image = Image.open(BytesIO(png_data))
                                st.image(image, caption=&quot;Generated Visualization&quot;, use_container_width=False)
                            elif hasattr(result, &#x27;figure&#x27;):  # For matplotlib figures
                                fig = result.figure  # Extract the matplotlib figure
                                st.pyplot(fig)  # Display using st.pyplot
                            elif hasattr(result, &#x27;show&#x27;):  # For plotly figures
                                st.plotly_chart(result)
                            elif isinstance(result, (pd.DataFrame, pd.Series)):
                                st.dataframe(result)
                            else:
                                st.write(result)  

if __name__ == &quot;__main__&quot;:
    main()
```