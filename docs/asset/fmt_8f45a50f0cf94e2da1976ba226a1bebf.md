
+ 🔗 Cross-Modal Relationship Mapping: Establishes semantic connections and dependencies between textual entities and multimodal components. This is achieved through automated relationship inference algorithms.

+ 🏗️ Hierarchical Structure Preservation: Maintains original document organization through `belongs_to` relationship chains. These chains preserve logical content hierarchy and sectional dependencies.

+ ⚖️ Weighted Relationship Scoring: Assigns quantitative relevance scores to relationship types. Scoring is based on semantic proximity and contextual significance within the document structure.

### Modality-Aware Retrieval

The hybrid retrieval system combines vector similarity search with graph traversal algorithms for comprehensive content retrieval. It implements modality-aware ranking mechanisms and maintains relational coherence between retrieved elements to ensure contextually integrated information delivery.

Retrieval Mechanisms:

+ 🔀 Vector-Graph Fusion: Integrates vector similarity search with graph traversal algorithms. This approach leverages both semantic embeddings and structural relationships for comprehensive content retrieval.

+ 📊 Modality-Aware Ranking: Implements adaptive scoring mechanisms that weight retrieval results based on content type relevance. The system adjusts rankings according to query-specific modality preferences.

+ 🔗 Relational Coherence Maintenance: Maintains semantic and structural relationships between retrieved elements. This ensures coherent information delivery and contextual integrity.

---

## 🚀 Quick Start

Initialize Your AI Journey

![](img/1e0e8f33dc0efeb5f99ef879473fef99.png)

### Installation

#### Option 1: Install from PyPI (Recommended)

Basic installation

`pip install raganything`

With optional dependencies for extended format support:

`pip install 'raganything[all]'  # All optional features`

`pip install 'raganything[image]'  # Image format conversion (BMP, TIFF, GIF, WebP)`

`pip install 'raganything[text]'  # Text file processing (TXT, MD)`

`pip install 'raganything[image,text]'  # Multiple features`

#### Option 2: Install from Source

Install uv (if not already installed)

`curl -LsSf https://astral.sh/uv/install.sh | sh`

Clone and setup the project with uv

`git clone https://github.com/HKUDS/RAG-Anything.git`

`cd RAG-Anything`

Install the package and dependencies in a virtual environment

`uv sync`

If you encounter network timeouts (especially for opencv packages):

`UV_HTTP_TIMEOUT=120 uv sync`

Run commands directly with uv (recommended approach)

`uv run python examples/raganything_example.py --help`

Install with optional dependencies

`uv sync --extra image --extra text  # Specific extras`

`uv sync --all-extras  # All optional features`

#### Optional Dependencies

+ `image` - Enables processing of BMP, TIFF, GIF, WebP image formats (requires Pillow)

+ `text` - Enables processing of TXT and MD files (requires ReportLab)

+ `all` - Includes all Python optional dependencies

> ⚠️ Office Document Processing Requirements:

+ Office documents (`.doc`, `.docx`, `.ppt`, `.pptx`, `.xls`, `.xlsx`) require LibreOffice installation

+ Download from LibreOffice official website

+ Windows: Download installer from official website

+ macOS: `brew install --cask libreoffice`

+ Ubuntu/Debian: `sudo apt-get install libreoffice`

+ CentOS/RHEL: `sudo yum install libreoffice`

Check MinerU installation:

Verify installation:

```
mineru --version
```

Check if properly configured:

```
python -c "from raganything import RAGAnything; rag = RAGAnything(); print('✅ MinerU installed properly' if rag.check_parser_installation() else '❌ MinerU installation issue')"
```

Models are downloaded automatically on first use. For manual download, refer to MinerU Model Source Configuration.

### Usage Examples

#### End-to-End Document Processing

```python
import asyncio
from raganything import RAGAnything, RAGAnythingConfig
from lightrag.llm.openai import openai_complete_if_cache, openai_embed
from lightrag.utils import EmbeddingFunc

async def main():
    # Set up API configuration
    api_key = "your-api-key"
    base_url = "your-base-url"  # Optional
    # Create RAGAnything configuration
    config = RAGAnythingConfig(
        working_dir="./rag_storage",
        parser="mineru",  # Parser selection: mineru or docling
        parse_method="auto",  # Parse method: auto, ocr, or txt
        enable_image_processing=True,
        enable_table_processing=True,
        enable_equation_processing=True,
    )

    # Define LLM model function
    def llm_model_func(prompt, system_prompt=None, history_messages=[], **kwargs):
        return openai_complete_if_cache(
            "gpt-4o-mini",
            prompt,
            system_prompt=system_prompt,
            history_messages=history_messages,
            api_key=api_key,
            base_url=base_url,
            **kwargs,
        )

    # Define vision model function for image processing
    def vision_model_func(
        prompt, system_prompt=None, history_messages=[], image_data=None, messages=None, **kwargs
    ):
        # If messages format is provided (for multimodal VLM enhanced query), use it directly
        if messages:
            return openai_complete_if_cache(
                "gpt-4o",
                "",
                system_prompt=None,
                history_messages=[],
                messages=messages,
                api_key=api_key,
                base_url=base_url,
                **kwargs,
            )
        # Traditional single image format
        elif image_data:
            return openai_complete_if_cache(
                "gpt-4o",
                "",
                system_prompt=None,
                history_messages=[],
                messages=[
                    {"role": "system", "content": system_prompt}
                    if system_prompt
                    else None,
                    {
                        "role": "user",
                        "content": [
                            {"type": "text", "text": prompt},
                            {
                                "type": "image_url",
                                "image_url": {
                                    "url": f"data:image/jpeg;base64,{image_data}"
                                },
                            },
                        ],
                    }
                    if image_data
                    else {"role": "user", "content": prompt},
                ],
                api_key=api_key,
                base_url=base_url,
                **kwargs,
            )
        # Pure text format
        else:
            return llm_model_func(prompt, system_prompt, history_messages, **kwargs)

    # Define embedding function
    embedding_func = EmbeddingFunc(
        embedding_dim=3072,
        max_token_size=8192,
        func=lambda texts: openai_embed(
            texts,
            model="text-embedding-3-large",
            api_key=api_key,
            base_url=base_url,
        ),
    )

    # Initialize RAGAnything
    rag = RAGAnything(
        config=config,
        llm_model_func=llm_model_func,
        vision_model_func=vision_model_func,
        embedding_func=embedding_func,
    )

    # Process a document
    await rag.process_document_complete(
        file_path="path/to/your/document.pdf",
        output_dir="./output",
        parse_method="auto"
    )

    # Query the processed content
    # Pure text query - for basic knowledge base search
    text_result = await rag.aquery(
        "What are the main findings shown in the figures and tables?",
        mode="hybrid"
    )
    print("Text query result:", text_result)

    # Multimodal query with specific multimodal content
    multimodal_result = await rag.aquery_with_multimodal(
        "Explain this formula and its relevance to the document content",
        multimodal_content=[{
            "type": "equation",
            "latex": "P(d|q) = \\frac{P(q|d) \\cdot P(d)}{P(q)}",
            "equation_caption": "Document relevance probability"
        }],
        mode="hybrid"
    )
    print("Multimodal query result:", multimodal_result)

if __name__ == "__main__":
    asyncio.run(main())
```

#### Direct Multimodal Content Processing

```python
import asyncio
from lightrag import LightRAG
from lightrag.llm.openai import openai_complete_if_cache, openai_embed
from lightrag.utils import EmbeddingFunc
from raganything.modalprocessors import ImageModalProcessor, TableModalProcessor

async def process_multimodal_content():
    # Set up API configuration
    api_key = "your-api-key"
    base_url = "your-base-url"  # Optional
    # Initialize LightRAG
    rag = LightRAG(
        working_dir="./rag_storage",
        llm_model_func=lambda prompt, system_prompt=None, history_messages=[], **kwargs: openai_complete_if_cache(
            "gpt-4o-mini",
            prompt,
            system_prompt=system_prompt,
            history_messages=history_messages,
            api_key=api_key,
            base_url=base_url,
            **kwargs,
        ),
        embedding_func=EmbeddingFunc(
            embedding_dim=3072,
            max_token_size=8192,
            func=lambda texts: openai_embed(
                texts,
                model="text-embedding-3-large",
                api_key=api_key,
                base_url=base_url,
            ),
        ),
    )

    await rag.initialize_storages()
```
