# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/OVccdpcxCol4tGxvqUFcwnUrnRe](https://qqs7y1hozd1.feishu.cn/docx/OVccdpcxCol4tGxvqUFcwnUrnRe)

        &quot;tesla.png&quot;: &quot;https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbef936e6-3efa-43b3-88d7-7ec620cdb33b_2744x1539.png&quot;,
        &quot;netflix.png&quot;: &quot;https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F23bd84c9-5b62-4526-b467-3088e27e4193_2744x1539.png&quot;,
        &quot;nike.png&quot;: &quot;https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa5cd33ba-ae1a-42a8-a254-d85e690d9870_2741x1541.png&quot;,
        &quot;google.png&quot;: &quot;https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F395dd3b9-b38e-4d1f-91bc-d37b642ee920_2741x1541.png&quot;,
        &quot;accenture.png&quot;: &quot;https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F08b2227c-7dc8-49f7-b3c5-13cab5443ba6_2741x1541.png&quot;,
        &quot;tecent.png&quot;: &quot;https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0ec8448c-c4d1-4aab-a8e9-2ddebe0c95fd_2741x1541.png&quot;

```
import requests
import os
import io
import base64
import PIL
from PIL import Image
import tqdm
import numpy as np
import streamlit as st
import cohere
from google import genai
import fitz # PyMuPDF

# --- Streamlit App Configuration ---
st.set_page_config(layout=&quot;wide&quot;, page_title=&quot;Vision RAG with Cohere Embed-4&quot;)
st.title(&quot;Vision RAG with Cohere Embed-4 🖼️&quot;)

# --- API Key Input ---
with st.sidebar:
    st.header(&quot;🔑 API Keys&quot;)
    cohere_api_key = st.text_input(&quot;Cohere API Key&quot;, type=&quot;password&quot;, key=&quot;cohere_key&quot;)
    google_api_key = st.text_input(&quot;Google API Key (Gemini)&quot;, type=&quot;password&quot;, key=&quot;google_key&quot;)
    &quot;[Get a Cohere API key](https://dashboard.cohere.com/api-keys)&quot;
    &quot;[Get a Google API key](https://aistudio.google.com/app/apikey)&quot;

    st.markdown(&quot;---&quot;)
    if not cohere_api_key:
        st.warning(&quot;Please enter your Cohere API key to proceed.&quot;)
    if not google_api_key:
        st.warning(&quot;Please enter your Google API key to proceed.&quot;)
    st.markdown(&quot;---&quot;)

# --- Initialize API Clients ---
co = None
genai_client = None
# Initialize Session State for embeddings and paths
if &#x27;image_paths&#x27; not in st.session_state:
    st.session_state.image_paths = []
if &#x27;doc_embeddings&#x27; not in st.session_state:
    st.session_state.doc_embeddings = None

if cohere_api_key and google_api_key:
    try:
        co = cohere.ClientV2(api_key=cohere_api_key)
        st.sidebar.success(&quot;Cohere Client Initialized!&quot;)
    except Exception as e:
        st.sidebar.error(f&quot;Cohere Initialization Failed: {e}&quot;)

    try:
        genai_client = genai.Client(api_key=google_api_key)
        st.sidebar.success(&quot;Gemini Client Initialized!&quot;)
    except Exception as e:
        st.sidebar.error(f&quot;Gemini Initialization Failed: {e}&quot;)
else:
    st.info(&quot;Enter your API keys in the sidebar to start.&quot;)

# Information about the models
with st.expander(&quot;ℹ️ About the models used&quot;):
    st.markdown(&quot;&quot;&quot;
    ### Cohere Embed-4
    
    Cohere&#x27;s Embed-4 is a state-of-the-art multimodal embedding model designed for enterprise search and retrieval. 
    It enables:
    
    - **Multimodal search**: Search text and images together seamlessly
    - **High accuracy**: State-of-the-art performance for retrieval tasks
    - **Efficient embedding**: Process complex images like charts, graphs, and infographics
    
    The model processes images without requiring complex OCR pre-processing and maintains the connection between visual elements and text.
    
    ### Google Gemini 2.5 Flash
    
    Gemini 2.5 Flash is Google&#x27;s efficient multimodal model that can process text and image inputs to generate high-quality responses.
    It&#x27;s designed for fast inference while maintaining high accuracy, making it ideal for real-time applications like this RAG system.
    &quot;&quot;&quot;)

# --- Helper functions ---
# Some helper functions to resize images and to convert them to base64 format
max_pixels = 1568*1568  #Max resolution for images

# Resize too large images
def resize_image(pil_image: PIL.Image.Image) -&gt; None:
    &quot;&quot;&quot;Resizes the image in-place if it exceeds max_pixels.&quot;&quot;&quot;
    org_width, org_height = pil_image.size

    # Resize image if too large
    if org_width * org_height &gt; max_pixels:
        scale_factor = (max_pixels / (org_width * org_height)) ** 0.5
        new_width = int(org_width * scale_factor)
        new_height = int(org_height * scale_factor)
        pil_image.thumbnail((new_width, new_height))

# Convert images to a base64 string before sending it to the API
def base64_from_image(img_path: str) -&gt; str:
    &quot;&quot;&quot;Converts an image file to a base64 encoded string.&quot;&quot;&quot;
    pil_image = PIL.Image.open(img_path)
    img_format = pil_image.format if pil_image.format else &quot;PNG&quot;

    resize_image(pil_image)

    with io.BytesIO() as img_buffer:
        pil_image.save(img_buffer, format=img_format)
        img_buffer.seek(0)
        img_data = f&quot;data:image/{img_format.lower()};base64,&quot;+base64.b64encode(img_buffer.read()).decode(&quot;utf-8&quot;)

    return img_data

# Convert PIL image to base64 string
def pil_to_base64(pil_image: PIL.Image.Image) -&gt; str:
    &quot;&quot;&quot;Converts a PIL image to a base64 encoded string.&quot;&quot;&quot;
    if pil_image.format is None:
        img_format = &quot;PNG&quot;
    else:
        img_format = pil_image.format
    
    resize_image(pil_image)

    with io.BytesIO() as img_buffer:
        pil_image.save(img_buffer, format=img_format)
        img_buffer.seek(0)
        img_data = f&quot;data:image/{img_format.lower()};base64,&quot;+base64.b64encode(img_buffer.read()).decode(&quot;utf-8&quot;)

    return img_data

# Compute embedding for an image
@st.cache_data(ttl=3600, show_spinner=False)
def compute_image_embedding(base64_img: str, _cohere_client) -&gt; np.ndarray | None:
    &quot;&quot;&quot;Computes an embedding for an image using Cohere&#x27;s Embed-4 model.&quot;&quot;&quot;
    try:
        api_response = _cohere_client.embed(
            model=&quot;embed-v4.0&quot;,
            input_type=&quot;search_document&quot;,
            embedding_types=[&quot;float&quot;],
            images=[base64_img],
        )
        
        if api_response.embeddings and api_response.embeddings.float:
            return np.asarray(api_response.embeddings.float[0])
        else:
            st.warning(&quot;Could not get embedding. API response might be empty.&quot;)
            return None
    except Exception as e:
        st.error(f&quot;Error computing embedding: {e}&quot;)
        return None

# Process a PDF file: extract pages as images and embed them
# Note: Caching PDF processing might be complex due to potential large file sizes and streams
# We will process it directly for now, but show progress.
def process_pdf_file(pdf_file, cohere_client, base_output_folder=&quot;pdf_pages&quot;) -&gt; tuple[list[str], list[np.ndarray] | None]:
    &quot;&quot;&quot;Extracts pages from a PDF as images, embeds them, and saves them.

    Args:
        pdf_file: UploadedFile object from Streamlit.
        cohere_client: Initialized Cohere client.
        base_output_folder: Directory to save page images.

    Returns:
        A tuple containing: 
          - list of paths to the saved page images.
          - list of numpy array embeddings for each page, or None if embedding fails.
    &quot;&quot;&quot;
    page_image_paths = []
    page_embeddings = []
    pdf_filename = pdf_file.name
    output_folder = os.path.join(base_output_folder, os.path.splitext(pdf_filename)[0])
    os.makedirs(output_folder, exist_ok=True)

    try:
        # Open PDF from stream
        doc = fitz.open(stream=pdf_file.read(), filetype=&quot;pdf&quot;)
        st.write(f&quot;Processing PDF: {pdf_filename} ({len(doc)} pages)&quot;)
        pdf_progress = st.progress(0.0)

        for i, page in enumerate(doc.pages()):
            page_num = i + 1
            page_img_path = os.path.join(output_folder, f&quot;page_{page_num}.png&quot;)
            page_image_paths.append(page_img_path)

            # Render page to pixmap (image)
            pix = page.get_pixmap(dpi=150) # Adjust DPI as needed for quality/performance
            pil_image = Image.frombytes(&quot;RGB&quot;, [pix.width, pix.height], pix.samples)
            
            # Save the page image temporarily
            pil_image.save(page_img_path, &quot;PNG&quot;)
            
            # Convert PIL image to base64
            base64_img = pil_to_base64(pil_image)
            
            # Compute embedding for the page image
            emb = compute_image_embedding(base64_img, _cohere_client=cohere_client)
            if emb is not None:
                page_embeddings.append(emb)
            else:
                st.warning(f&quot;Could not embed page {page_num} from {pdf_filename}. Skipping.&quot;)
                # Add a placeholder to keep lists aligned, will be filtered later
                page_embeddings.append(None)

            # Update progress
            pdf_progress.progress((i + 1) / len(doc))

        doc.close()
        pdf_progress.empty() # Remove progress bar after completion
        
        # Filter out pages where embedding failed
        valid_paths = [path for i, path in enumerate(page_image_paths) if page_embeddings[i] is not None]
        valid_embeddings = [emb for emb in page_embeddings if emb is not None]
        
        if not valid_embeddings:
             st.error(f&quot;Failed to generate any embeddings for {pdf_filename}.&quot;)
             return [], None

        return valid_paths, valid_embeddings

    except Exception as e:
        st.error(f&quot;Error processing PDF {pdf_filename}: {e}&quot;)
        return [], None

# Download and embed sample images
@st.cache_data(ttl=3600, show_spinner=False)
def download_and_embed_sample_images(_cohere_client) -&gt; tuple[list[str], np.ndarray | None]:
    &quot;&quot;&quot;Downloads sample images and computes their embeddings using Cohere&#x27;s Embed-4 model.&quot;&quot;&quot;
    # Several images from https://www.appeconomyinsights.com/
    images = {
    }

    # Prepare folders
    img_folder = &quot;img&quot;
    os.makedirs(img_folder, exist_ok=True)

    img_paths = []
    doc_embeddings = []
    
    # Wrap TQDM with st.spinner for better UI integration
    with st.spinner(&quot;Downloading and embedding sample images...&quot;):
        pbar = tqdm.tqdm(images.items(), desc=&quot;Processing sample images&quot;)
        for name, url in pbar:
            img_path = os.path.join(img_folder, name)
            # Don&#x27;t re-append if already processed (useful if function called multiple times)
            if img_path not in img_paths:
                img_paths.append(img_path)

                # Download the image
                if not os.path.exists(img_path):
                    try:
                        response = requests.get(url)
                        response.raise_for_status()
                        with open(img_path, &quot;wb&quot;) as fOut:
                            fOut.write(response.content)
                    except requests.exceptions.RequestException as e:
                        st.error(f&quot;Failed to download {name}: {e}&quot;)
                        # Optionally remove the path if download failed
                        img_paths.pop()
                        continue # Skip if download fails

            # Get embedding for the image if it exists and we haven&#x27;t computed one yet
            # Find index corresponding to this path
            current_index = -1
            try:
                current_index = img_paths.index(img_path)
            except ValueError:
                continue # Should not happen if append logic is correct

            # Check if embedding already exists for this index
            if current_index &gt;= len(doc_embeddings):
                 try:
                     # Ensure file exists before trying to embed
                     if os.path.exists(img_path):
                         base64_img = base64_from_image(img_path)
                         emb = compute_image_embedding(base64_img, _cohere_client=_cohere_client)
                         if emb is not None:
                             # Placeholder to ensure list length matches paths before vstack
                             while len(doc_embeddings) &lt; current_index:
                                 doc_embeddings.append(None) # Append placeholder if needed
                             doc_embeddings.append(emb)
                     else:
                         # If file doesn&#x27;t exist (maybe failed download), add placeholder
                         while len(doc_embeddings) &lt; current_index:
                                 doc_embeddings.append(None)
                         doc_embeddings.append(None)
                 except Exception as e:
                     st.error(f&quot;Failed to embed {name}: {e}&quot;)
                     # Add placeholder on error
                     while len(doc_embeddings) &lt; current_index:
                             doc_embeddings.append(None)
                     doc_embeddings.append(None)
    
    # Filter out None embeddings and corresponding paths before stacking
    filtered_paths = [path for i, path in enumerate(img_paths) if i &lt; len(doc_embeddings) and doc_embeddings[i] is not None]
    filtered_embeddings = [emb for emb in doc_embeddings if emb is not None]

    if filtered_embeddings:
        doc_embeddings_array = np.vstack(filtered_embeddings)
        return filtered_paths, doc_embeddings_array
        
    return [], None

# Search function
def search(question: str, co_client: cohere.Client, embeddings: np.ndarray, image_paths: list[str], max_img_size: int = 800) -&gt; str | None:
    &quot;&quot;&quot;Finds the most relevant image path for a given question.&quot;&quot;&quot;
    if not co_client or embeddings is None or embeddings.size == 0 or not image_paths:
        st.warning(&quot;Search prerequisites not met (client, embeddings, or paths missing/empty).&quot;)
        return None
    if embeddings.shape[0] != len(image_paths):
         st.error(f&quot;Mismatch between embeddings count ({embeddings.shape[0]}) and image paths count ({len(image_paths)}). Cannot perform search.&quot;)
         return None

    try:
        # Compute the embedding for the query
        api_response = co_client.embed(
            model=&quot;embed-v4.0&quot;,
            input_type=&quot;search_query&quot;,
            embedding_types=[&quot;float&quot;],
            texts=[question],
        )

        if not api_response.embeddings or not api_response.embeddings.float:
            st.error(&quot;Failed to get query embedding.&quot;)
            return None

        query_emb = np.asarray(api_response.embeddings.float[0])

        # Ensure query embedding has the correct shape for dot product
        if query_emb.shape[0] != embeddings.shape[1]:
            st.error(f&quot;Query embedding dimension ({query_emb.shape[0]}) does not match document embedding dimension ({embeddings.shape[1]}).&quot;)
            return None

        # Compute cosine similarities
        cos_sim_scores = np.dot(query_emb, embeddings.T)

        # Get the most relevant image
        top_idx = np.argmax(cos_sim_scores)
        hit_img_path = image_paths[top_idx]
        print(f&quot;Question: {question}&quot;) # Keep for debugging
        print(f&quot;Most relevant image: {hit_img_path}&quot;) # Keep for debugging

        return hit_img_path
    except Exception as e:
        st.error(f&quot;Error during search: {e}&quot;)
        return None

# Answer function
def answer(question: str, img_path: str, gemini_client) -&gt; str:
    &quot;&quot;&quot;Answers the question based on the provided image using Gemini.&quot;&quot;&quot;
    if not gemini_client or not img_path or not os.path.exists(img_path):
        missing = []
        if not gemini_client: missing.append(&quot;Gemini client&quot;)
        if not img_path: missing.append(&quot;Image path&quot;)
        elif not os.path.exists(img_path): missing.append(f&quot;Image file at {img_path}&quot;)
        return f&quot;Answering prerequisites not met ({&#x27;, &#x27;.join(missing)} missing or invalid).&quot;
    try:
        img = PIL.Image.open(img_path)
        prompt = [f&quot;&quot;&quot;Answer the question based on the following image. Be as elaborate as possible giving extra relevant information.
Don&#x27;t use markdown formatting in the response.
Please provide enough context for your answer.

Question: {question}&quot;&quot;&quot;, img]

        response = gemini_client.models.generate_content(
            model=&quot;gemini-2.5-flash-preview-04-17&quot;,
            contents=prompt
        )

        llm_answer = response.text
        print(&quot;LLM Answer:&quot;, llm_answer) # Keep for debugging
        return llm_answer
    except Exception as e:
        st.error(f&quot;Error during answer generation: {e}&quot;)
        return f&quot;Failed to generate answer: {e}&quot;

# --- Main UI Setup ---
st.subheader(&quot;📊 Load Sample Images&quot;)
if cohere_api_key and co:
    # If button clicked, load sample images into session state
    if st.button(&quot;Load Sample Images&quot;, key=&quot;load_sample_button&quot;):
        sample_img_paths, sample_doc_embeddings = download_and_embed_sample_images(_cohere_client=co)
        if sample_img_paths and sample_doc_embeddings is not None:
            # Append sample images to session state (avoid duplicates if clicked again)
            current_paths = set(st.session_state.image_paths)
            new_paths = [p for p in sample_img_paths if p not in current_paths]
            
            if new_paths:
                new_indices = [i for i, p in enumerate(sample_img_paths) if p in new_paths]
                st.session_state.image_paths.extend(new_paths)
                new_embeddings_to_add = sample_doc_embeddings[[idx for idx, p in enumerate(sample_img_paths) if p in new_paths]]
                
                if st.session_state.doc_embeddings is None or st.session_state.doc_embeddings.size == 0:
                    st.session_state.doc_embeddings = new_embeddings_to_add
                else:
                    st.session_state.doc_embeddings = np.vstack((st.session_state.doc_embeddings, new_embeddings_to_add))
                st.success(f&quot;Loaded {len(new_paths)} sample images.&quot;)
            else:
                 st.info(&quot;Sample images already loaded.&quot;)
        else:
             st.error(&quot;Failed to load sample images. Check console for errors.&quot;)
else:
     st.warning(&quot;Enter API keys to enable loading sample images.&quot;)

st.markdown(&quot;--- &quot;)
# --- File Uploader (Main UI) ---
st.subheader(&quot;📤 Upload Your Images&quot;)
st.info(&quot;Or, upload your own images or PDFs. The RAG process will search across all loaded content.&quot;)

# File uploader
uploaded_files = st.file_uploader(&quot;Upload images (PNG, JPG, JPEG) or PDFs&quot;, 
                                type=[&quot;png&quot;, &quot;jpg&quot;, &quot;jpeg&quot;, &quot;pdf&quot;], 
                                accept_multiple_files=True, key=&quot;image_uploader&quot;,
                                label_visibility=&quot;collapsed&quot;)

# Process uploaded images
if uploaded_files and co:
    st.write(f&quot;Processing {len(uploaded_files)} uploaded images...&quot;)
    progress_bar = st.progress(0)
    
    # Create a temporary directory for uploaded images
    upload_folder = &quot;uploaded_img&quot;
    os.makedirs(upload_folder, exist_ok=True)
    
    newly_uploaded_paths = []
    newly_uploaded_embeddings = []

    for i, uploaded_file in enumerate(uploaded_files):
        # Check if already processed this session (simple name check)
        img_path = os.path.join(upload_folder, uploaded_file.name)
        if img_path not in st.session_state.image_paths:
            try:
                # Check file type
                file_type = uploaded_file.type
                if file_type == &quot;application/pdf&quot;:
                    # Process PDF - returns list of paths and list of embeddings
                    pdf_page_paths, pdf_page_embeddings = process_pdf_file(uploaded_file, cohere_client=co)
                    if pdf_page_paths and pdf_page_embeddings:
                         # Add only paths/embeddings not already in session state
                         current_paths_set = set(st.session_state.image_paths)
                         unique_new_paths = [p for p in pdf_page_paths if p not in current_paths_set]
                         if unique_new_paths:
                             indices_to_add = [i for i, p in enumerate(pdf_page_paths) if p in unique_new_paths]
                             newly_uploaded_paths.extend(unique_new_paths)
                             newly_uploaded_embeddings.extend([pdf_page_embeddings[idx] for idx in indices_to_add])
                elif file_type in [&quot;image/png&quot;, &quot;image/jpeg&quot;]:
                    # Process regular image
                    # Save the uploaded file
                    with open(img_path, &quot;wb&quot;) as f:
                        f.write(uploaded_file.getbuffer())
                    
                    # Get embedding
                    base64_img = base64_from_image(img_path)
                    emb = compute_image_embedding(base64_img, _cohere_client=co)
                    
                    if emb is not None:
                        newly_uploaded_paths.append(img_path)
                        newly_uploaded_embeddings.append(emb)
                else:
                     st.warning(f&quot;Unsupported file type skipped: {uploaded_file.name} ({file_type})&quot;)

            except Exception as e:
                st.error(f&quot;Error processing {uploaded_file.name}: {e}&quot;)
        # Update progress regardless of processing status for user feedback
        progress_bar.progress((i + 1) / len(uploaded_files))

    # Add newly processed files to session state
    if newly_uploaded_paths:
        st.session_state.image_paths.extend(newly_uploaded_paths)
        if newly_uploaded_embeddings:
            new_embeddings_array = np.vstack(newly_uploaded_embeddings)
            if st.session_state.doc_embeddings is None or st.session_state.doc_embeddings.size == 0:
                st.session_state.doc_embeddings = new_embeddings_array
            else:
                st.session_state.doc_embeddings = np.vstack((st.session_state.doc_embeddings, new_embeddings_array))
            st.success(f&quot;Successfully processed and added {len(newly_uploaded_paths)} new images.&quot;)
        else:
             st.warning(&quot;Failed to generate embeddings for newly uploaded images.&quot;)
    elif uploaded_files: # If files were selected but none were new
         st.info(&quot;Selected images already seem to be processed.&quot;)

# --- Vision RAG Section (Main UI) ---
st.markdown(&quot;---&quot;)
st.subheader(&quot;❓ Ask a Question&quot;)

if not st.session_state.image_paths:
    st.warning(&quot;Please load sample images or upload your own images first.&quot;)
else:
    st.info(f&quot;Ready to answer questions about {len(st.session_state.image_paths)} images.&quot;)

    # Display thumbnails of all loaded images (optional)
    with st.expander(&quot;View Loaded Images&quot;, expanded=False):
        if st.session_state.image_paths:
            num_images_to_show = len(st.session_state.image_paths)
            cols = st.columns(5) # Show 5 thumbnails per row
            for i in range(num_images_to_show):
                with cols[i % 5]:
                    # Add try-except for missing files during display
                    try:
                         # Display PDF pages differently? For now, just show the image
                         st.image(st.session_state.image_paths[i], width=100, caption=os.path.basename(st.session_state.image_paths[i]))
                    except FileNotFoundError:
                        st.error(f&quot;Missing: {os.path.basename(st.session_state.image_paths[i])}&quot;)
        else:
            st.write(&quot;No images loaded yet.&quot;)

question = st.text_input(&quot;Ask a question about the loaded images:&quot;, 
                          key=&quot;main_question_input&quot;,
                          placeholder=&quot;E.g., What is Nike&#x27;s net profit?&quot;,
                          disabled=not st.session_state.image_paths)

run_button = st.button(&quot;Run Vision RAG&quot;, key=&quot;main_run_button&quot;, 
                      disabled=not (cohere_api_key and google_api_key and question and st.session_state.image_paths and st.session_state.doc_embeddings is not None and st.session_state.doc_embeddings.size &gt; 0))

# Output Area
st.markdown(&quot;### Results&quot;)
retrieved_image_placeholder = st.empty()
answer_placeholder = st.empty()

# Run search and answer logic
if run_button:
    if co and genai_client and st.session_state.doc_embeddings is not None and len(st.session_state.doc_embeddings) &gt; 0:
         with st.spinner(&quot;Finding relevant image...&quot;):
            # Ensure embeddings and paths match before search
             if len(st.session_state.image_paths) != st.session_state.doc_embeddings.shape[0]:
                 st.error(&quot;Error: Mismatch between number of images and embeddings. Cannot proceed.&quot;)
             else:
                top_image_path = search(question, co, st.session_state.doc_embeddings, st.session_state.image_paths)

                if top_image_path:
                    caption = f&quot;Retrieved content for: &#x27;{question}&#x27; (Source: {os.path.basename(top_image_path)})&quot;
                    # Add source PDF name if it&#x27;s a page image
                    if top_image_path.startswith(&quot;pdf_pages/&quot;):
                         parts = top_image_path.split(os.sep)
                         if len(parts) &gt;= 3:
                             pdf_name = parts[1]
                             page_name = parts[-1]
                             caption = f&quot;Retrieved content for: &#x27;{question}&#x27; (Source: {pdf_name}.pdf, {page_name.replace(&#x27;.png&#x27;,&#x27;&#x27;)})&quot;

                    retrieved_image_placeholder.image(top_image_path, caption=caption, use_container_width=True)

                    with st.spinner(&quot;Generating answer...&quot;):
                        final_answer = answer(question, top_image_path, genai_client)
                        answer_placeholder.markdown(f&quot;**Answer:**\n{final_answer}&quot;)
                else:
                    retrieved_image_placeholder.warning(&quot;Could not find a relevant image for your question.&quot;)
                    answer_placeholder.text(&quot;&quot;) # Clear answer placeholder
    else:
        # This case should ideally be prevented by the disabled state of the button
        st.error(&quot;Cannot run RAG. Check API clients and ensure images are loaded with embeddings.&quot;)

# Footer
st.markdown(&quot;---&quot;)
st.caption(&quot;Vision RAG with Cohere Embed-4 | Built with Streamlit, Cohere Embed-4, and Google Gemini 2.5 Flash&quot;)
```