# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/YmXdd9eapodTcgx4zf6csKhpnGc](https://qqs7y1hozd1.feishu.cn/docx/YmXdd9eapodTcgx4zf6csKhpnGc)

```
import json
import tempfile
import csv
import streamlit as st
import pandas as pd
from agno.models.openai import OpenAIChat
from phi.agent.duckdb import DuckDbAgent
from agno.tools.pandas import PandasTools
import re

# Function to preprocess and save the uploaded file
def preprocess_and_save(file):
    try:
        # Read the uploaded file into a DataFrame
        if file.name.endswith(&#x27;.csv&#x27;):
            df = pd.read_csv(file, encoding=&#x27;utf-8&#x27;, na_values=[&#x27;NA&#x27;, &#x27;N/A&#x27;, &#x27;missing&#x27;])
        elif file.name.endswith(&#x27;.xlsx&#x27;):
            df = pd.read_excel(file, na_values=[&#x27;NA&#x27;, &#x27;N/A&#x27;, &#x27;missing&#x27;])
        else:
            st.error(&quot;Unsupported file format. Please upload a CSV or Excel file.&quot;)
            return None, None, None
        
        # Ensure string columns are properly quoted
        for col in df.select_dtypes(include=[&#x27;object&#x27;]):
            df[col] = df[col].astype(str).replace({r&#x27;&quot;&#x27;: &#x27;&quot;&quot;&#x27;}, regex=True)
        
        # Parse dates and numeric columns
        for col in df.columns:
            if &#x27;date&#x27; in col.lower():
                df[col] = pd.to_datetime(df[col], errors=&#x27;coerce&#x27;)
            elif df[col].dtype == &#x27;object&#x27;:
                try:
                    df[col] = pd.to_numeric(df[col])
                except (ValueError, TypeError):
                    # Keep as is if conversion fails
                    pass
        
        # Create a temporary file to save the preprocessed data
        with tempfile.NamedTemporaryFile(delete=False, suffix=&quot;.csv&quot;) as temp_file:
            temp_path = temp_file.name
            # Save the DataFrame to the temporary CSV file with quotes around string fields
            df.to_csv(temp_path, index=False, quoting=csv.QUOTE_ALL)
        
        return temp_path, df.columns.tolist(), df  # Return the DataFrame as well
    except Exception as e:
        st.error(f&quot;Error processing file: {e}&quot;)
        return None, None, None

# Streamlit app
st.title(&quot;📊 Data Analyst Agent&quot;)

# Sidebar for API keys
with st.sidebar:
    st.header(&quot;API Keys&quot;)
    openai_key = st.text_input(&quot;Enter your OpenAI API key:&quot;, type=&quot;password&quot;)
    if openai_key:
        st.session_state.openai_key = openai_key
        st.success(&quot;API key saved!&quot;)
    else:
        st.warning(&quot;Please enter your OpenAI API key to proceed.&quot;)

# File upload widget
uploaded_file = st.file_uploader(&quot;Upload a CSV or Excel file&quot;, type=[&quot;csv&quot;, &quot;xlsx&quot;])

if uploaded_file is not None and &quot;openai_key&quot; in st.session_state:
    # Preprocess and save the uploaded file
    temp_path, columns, df = preprocess_and_save(uploaded_file)
    
    if temp_path and columns and df is not None:
        # Display the uploaded data as a table
        st.write(&quot;Uploaded Data:&quot;)
        st.dataframe(df)  # Use st.dataframe for an interactive table
        
        # Display the columns of the uploaded data
        st.write(&quot;Uploaded columns:&quot;, columns)
        
        # Configure the semantic model with the temporary file path
        semantic_model = {
            &quot;tables&quot;: [
                {
                    &quot;name&quot;: &quot;uploaded_data&quot;,
                    &quot;description&quot;: &quot;Contains the uploaded dataset.&quot;,
                    &quot;path&quot;: temp_path,
                }
            ]
        }
        
        # Initialize the DuckDbAgent for SQL query generation
        duckdb_agent = DuckDbAgent(
            model=OpenAIChat(model=&quot;gpt-4&quot;, api_key=st.session_state.openai_key),
            semantic_model=json.dumps(semantic_model),
            tools=[PandasTools()],
            markdown=True,
            add_history_to_messages=False,  # Disable chat history
            followups=False,  # Disable follow-up queries
            read_tool_call_history=False,  # Disable reading tool call history
            system_prompt=&quot;You are an expert data analyst. Generate SQL queries to solve the user&#x27;s query. Return only the SQL query, enclosed in ```sql ``` and give the final answer.&quot;,
        )
        
        # Initialize code storage in session state
        if &quot;generated_code&quot; not in st.session_state:
            st.session_state.generated_code = None
        
        # Main query input widget
        user_query = st.text_area(&quot;Ask a query about the data:&quot;)
        
        # Add info message about terminal output
        st.info(&quot;💡 Check your terminal for a clearer output of the agent&#x27;s response&quot;)
        
        if st.button(&quot;Submit Query&quot;):
            if user_query.strip() == &quot;&quot;:
                st.warning(&quot;Please enter a query.&quot;)
            else:
                try:
                    # Show loading spinner while processing
                    with st.spinner(&#x27;Processing your query...&#x27;):
                        # Get the response from DuckDbAgent
               
                        response1 = duckdb_agent.run(user_query)

                        # Extract the content from the RunResponse object
                        if hasattr(response1, &#x27;content&#x27;):
                            response_content = response1.content
                        else:
                            response_content = str(response1)
                        response = duckdb_agent.print_response(
                        user_query,
                        stream=True,
                        )

                    # Display the response in Streamlit
                    st.markdown(response_content)
                
                    
                except Exception as e:
                    st.error(f&quot;Error generating response from the DuckDbAgent: {e}&quot;)
                    st.error(&quot;Please try rephrasing your query or check if the data format is correct.&quot;)
```