
# let's set up a dummy product catalog:
sample_product_catalog = """
Sleep Haven product 1: Luxury Cloud-Comfort Memory Foam Mattress
Experience the epitome of opulence with our Luxury Cloud-Comfort Memory Foam Mattress. Designed with an innovative, temperature-sensitive memory foam layer, this mattress embraces your body shape, offering personalized support and unparalleled comfort. The mattress is completed with a high-density foam base that ensures longevity, maintaining its form and resilience for years. With the incorporation of cooling gel-infused particles, it regulates your body temperature throughout the night, providing a perfect cool slumbering environment. The breathable, hypoallergenic cover, exquisitely embroidered with silver threads, not only adds a touch of elegance to your bedroom but also keeps allergens at bay. For a restful night and a refreshed morning, invest in the Luxury Cloud-Comfort Memory Foam Mattress.
Price: $999
Sizes available for this product: Twin, Queen, King
Sleep Haven product 2: Classic Harmony Spring Mattress
A perfect blend of traditional craftsmanship and modern comfort, the Classic Harmony Spring Mattress is designed to give you restful, uninterrupted sleep. It features a robust inner spring construction, complemented by layers of plush padding that offers the perfect balance of support and comfort. The quilted top layer is soft to the touch, adding an extra level of luxury to your sleeping experience. Reinforced edges prevent sagging, ensuring durability and a consistent sleeping surface, while the natural cotton cover wicks away moisture, keeping you dry and comfortable throughout the night. The Classic Harmony Spring Mattress is a timeless choice for those who appreciate the perfect fusion of support and plush comfort.
Price: $1,299
Sizes available for this product: Queen, King
Sleep Haven product 3: EcoGreen Hybrid Latex Mattress
The EcoGreen Hybrid Latex Mattress is a testament to sustainable luxury. Made from 100% natural latex harvested from eco-friendly plantations, this mattress offers a responsive, bouncy feel combined with the benefits of pressure relief. It is layered over a core of individually pocketed coils, ensuring minimal motion transfer, perfect for those sharing their bed. The mattress is wrapped in a certified organic cotton cover, offering a soft, breathable surface that enhances your comfort. Furthermore, the natural antimicrobial and hypoallergenic properties of latex make this mattress a great choice for allergy sufferers. Embrace a green lifestyle without compromising on comfort with the EcoGreen Hybrid Latex Mattress.
Price: $1,599
Sizes available for this product: Twin, Full
Sleep Haven product 4: Plush Serenity Bamboo Mattress
The Plush Serenity Bamboo Mattress takes the concept of sleep to new heights of comfort and environmental responsibility. The mattress features a layer of plush, adaptive foam that molds to your body's unique shape, providing tailored support for each sleeper. Underneath, a base of high-resilience support foam adds longevity and prevents sagging. The crowning glory of this mattress is its bamboo-infused top layer - this sustainable material is not only gentle on the planet, but also creates a remarkably soft, cool sleeping surface. Bamboo's natural breathability and moisture-wicking properties make it excellent for temperature regulation, helping to keep you cool and dry all night long. Encased in a silky, removable bamboo cover that's easy to clean and maintain, the Plush Serenity Bamboo Mattress offers a luxurious and eco-friendly sleeping experience.
Price: $2,599
Sizes available for this product: King
"""
with open("sample_product_catalog.txt", "w") as f:
    f.write(sample_product_catalog)

product_catalog = "sample_product_catalog.txt"

## 创建知识库

```
# Set up a knowledge base
def setup_knowledge_base(product_catalog: str = None):
    """
    We assume that the product knowledge base is simply a text file.
    """
    # load product catalog
    with open(product_catalog, "r") as f:
        product_catalog = f.read()

    text_splitter = CharacterTextSplitter(chunk_size=10, chunk_overlap=0)
    texts = text_splitter.split_text(product_catalog)

    llm = ChatOpenAI(temperature=0)
    embeddings = OpenAIEmbeddings()
    docsearch = Chroma.from_texts(
        texts, embeddings, collection_name="product-knowledge-base"
    )

    knowledge_base = RetrievalQA.from_chain_type(
        llm=llm, chain_type="stuff", retriever=docsearch.as_retriever()
    )
    return knowledge_base

knowledge_base = setup_knowledge_base("sample_product_catalog.txt")
knowledge_base.run("What products do you have available?")
```

## 支付网关

为了设置AI agent使用支付网关为用户生成付款链接，需要两件事情：

1. 注册`Stripe`账户并获取`STRIPE` API密钥。

2. 在`Stripe`用户界面中创建想要销售的产品。然后按照`example_product_price_id_mapping.json`的示例，将产品名称映射到价格ID，从而生成付款链接。

```
import json

from litellm import completion

# set GPT model env variable
os.environ["GPT_MODEL"] = "gpt-4-turbo-preview"

product_price_id_mapping = {
    "ai-consulting-services": "price_1Ow8ofB795AYY8p1goWGZi6m",
    "Luxury Cloud-Comfort Memory Foam Mattress": "price_1Owv99B795AYY8p1mjtbKyxP",
    "Classic Harmony Spring Mattress": "price_1Owv9qB795AYY8p1tPcxCM6T",
    "EcoGreen Hybrid Latex Mattress": "price_1OwvLDB795AYY8p1YBAMBcbi",
    "Plush Serenity Bamboo Mattress": "price_1OwvMQB795AYY8p1hJN2uS3S",
}
with open("example_product_price_id_mapping.json", "w") as f:
    json.dump(product_price_id_mapping, f)


def get_product_id_from_query(query, product_price_id_mapping_path):
    # Load product_price_id_mapping from a JSON file
    with open(product_price_id_mapping_path, "r") as f:
        product_price_id_mapping = json.load(f)

    # Serialize the product_price_id_mapping to a JSON string for inclusion in the prompt
    product_price_id_mapping_json_str = json.dumps(product_price_id_mapping)

    # Dynamically create the enum list from product_price_id_mapping keys
    enum_list = list(product_price_id_mapping.values()) + [
        "No relevant product id found"
    ]
    enum_list_str = json.dumps(enum_list)

    prompt = f"""
    You are an expert data scientist and you are working on a project to recommend products to customers based on their needs.
    Given the following query:
    {query}
    and the following product price id mapping:
    {product_price_id_mapping_json_str}
    return the price id that is most relevant to the query.
    ONLY return the price id, no other text. If no relevant price id is found, return 'No relevant price id found'.
    Your output will follow this schema:
    {{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "title": "Price ID Response",
    "type": "object",
    "properties": {{
        "price_id": {{
        "type": "string",
        "enum": {enum_list_str}
        }}
    }},
    "required": ["price_id"]
    }}
    Return a valid directly parsable json, dont return in it within a code snippet or add any kind of explanation!!
    """
    prompt += "{"
    response = completion(
        model=os.getenv("GPT_MODEL", "gpt-3.5-turbo-1106"),
        messages=[{"content": prompt, "role": "user"}],
        max_tokens=1000,
        temperature=0,
    )

    product_id = response.choices[0].message.content.strip()
    return product_id
```

## 生成支付链接
