# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/IFyAdMa8yofTNPxA8UxcJXaGnOf](https://qqs7y1hozd1.feishu.cn/docx/IFyAdMa8yofTNPxA8UxcJXaGnOf)

```
import os
from PIL import Image as PILImage
from agno.agent import Agent
from agno.models.google import Gemini
import streamlit as st
from agno.tools.duckduckgo import DuckDuckGoTools
from agno.media import Image as AgnoImage

if &quot;GOOGLE_API_KEY&quot; not in st.session_state:
    st.session_state.GOOGLE_API_KEY = None

with st.sidebar:
    st.title(&quot;ℹ️ Configuration&quot;)
    
    if not st.session_state.GOOGLE_API_KEY:
        api_key = st.text_input(
            &quot;Enter your Google API Key:&quot;,
            type=&quot;password&quot;
        )
        st.caption(
            &quot;Get your API key from [Google AI Studio]&quot;
            &quot;(https://aistudio.google.com/apikey) 🔑&quot;
        )
        if api_key:
            st.session_state.GOOGLE_API_KEY = api_key
            st.success(&quot;API Key saved!&quot;)
            st.rerun()
    else:
        st.success(&quot;API Key is configured&quot;)
        if st.button(&quot;🔄 Reset API Key&quot;):
            st.session_state.GOOGLE_API_KEY = None
            st.rerun()
    
    st.info(
        &quot;This tool provides AI-powered analysis of medical imaging data using &quot;
        &quot;advanced computer vision and radiological expertise.&quot;
    )
    st.warning(
        &quot;⚠DISCLAIMER: This tool is for educational and informational purposes only. &quot;
        &quot;All analyses should be reviewed by qualified healthcare professionals. &quot;
        &quot;Do not make medical decisions based solely on this analysis.&quot;
    )

medical_agent = Agent(
    model=Gemini(
        id=&quot;gemini-2.0-flash&quot;,
        api_key=st.session_state.GOOGLE_API_KEY
    ),
    tools=[DuckDuckGoTools()],
    markdown=True
) if st.session_state.GOOGLE_API_KEY else None

if not medical_agent:
    st.warning(&quot;Please configure your API key in the sidebar to continue&quot;)

# Medical Analysis Query
query = &quot;&quot;&quot;
You are a highly skilled medical imaging expert with extensive knowledge in radiology and diagnostic imaging. Analyze the patient&#x27;s medical image and structure your response as follows:

### 1. Image Type &amp; Region
- Specify imaging modality (X-ray/MRI/CT/Ultrasound/etc.)
- Identify the patient&#x27;s anatomical region and positioning
- Comment on image quality and technical adequacy

### 2. Key Findings
- List primary observations systematically
- Note any abnormalities in the patient&#x27;s imaging with precise descriptions
- Include measurements and densities where relevant
- Describe location, size, shape, and characteristics
- Rate severity: Normal/Mild/Moderate/Severe

### 3. Diagnostic Assessment
- Provide primary diagnosis with confidence level
- List differential diagnoses in order of likelihood
- Support each diagnosis with observed evidence from the patient&#x27;s imaging
- Note any critical or urgent findings

### 4. Patient-Friendly Explanation
- Explain the findings in simple, clear language that the patient can understand
- Avoid medical jargon or provide clear definitions
- Include visual analogies if helpful
- Address common patient concerns related to these findings

### 5. Research Context
IMPORTANT: Use the DuckDuckGo search tool to:
- Find recent medical literature about similar cases
- Search for standard treatment protocols
- Provide a list of relevant medical links of them too
- Research any relevant technological advances
- Include 2-3 key references to support your analysis

Format your response using clear markdown headers and bullet points. Be concise yet thorough.
&quot;&quot;&quot;

st.title(&quot;🏥 Medical Imaging Diagnosis Agent&quot;)
st.write(&quot;Upload a medical image for professional analysis&quot;)

# Create containers for better organization
upload_container = st.container()
image_container = st.container()
analysis_container = st.container()

with upload_container:
    uploaded_file = st.file_uploader(
        &quot;Upload Medical Image&quot;,
        type=[&quot;jpg&quot;, &quot;jpeg&quot;, &quot;png&quot;, &quot;dicom&quot;],
        help=&quot;Supported formats: JPG, JPEG, PNG, DICOM&quot;
    )

if uploaded_file is not None:
    with image_container:
        col1, col2, col3 = st.columns([1, 2, 1])
        with col2:
            image = PILImage.open(uploaded_file)
            width, height = image.size
            aspect_ratio = width / height
            new_width = 500
            new_height = int(new_width / aspect_ratio)
            resized_image = image.resize((new_width, new_height))
            
            st.image(
                resized_image,
                caption=&quot;Uploaded Medical Image&quot;,
                use_container_width=True
            )
            
            analyze_button = st.button(
                &quot;🔍 Analyze Image&quot;,
                type=&quot;primary&quot;,
                use_container_width=True
            )
    
    with analysis_container:
        if analyze_button:
            with st.spinner(&quot;🔄 Analyzing image... Please wait.&quot;):
                try:
                    temp_path = &quot;temp_resized_image.png&quot;
                    resized_image.save(temp_path)
                    
                    # Create AgnoImage object
                    agno_image = AgnoImage(filepath=temp_path)  # Adjust if constructor differs
                    
                    # Run analysis
                    response = medical_agent.run(query, images=[agno_image])
                    st.markdown(&quot;### 📋 Analysis Results&quot;)
                    st.markdown(&quot;---&quot;)
                    st.markdown(response.content)
                    st.markdown(&quot;---&quot;)
                    st.caption(
                        &quot;Note: This analysis is generated by AI and should be reviewed by &quot;
                        &quot;a qualified healthcare professional.&quot;
                    )
                except Exception as e:
                    st.error(f&quot;Analysis error: {e}&quot;)
else:
    st.info(&quot;👆 Please upload a medical image to begin analysis&quot;)
```