# AI-3003 在 Azure 中开发自然语言解决方案

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/TWvhdyKVeou7hQxdODycxOBln2c](https://zsyhjtnsa5.feishu.cn/docx/TWvhdyKVeou7hQxdODycxOBln2c)

实验 Repo：https://github.com/microsoftlearning/mslearn-ai-language

# 使用 Azure AI 语言分析文本

https://learn.microsoft.com/zh-cn/training/modules/analyze-text-ai-language/

### 什么是 Azure AI 语言服务？

Azure AI 语言服务旨在帮助用户从文本中提取信息，提供多种功能来分析和理解文本内容。

### Azure AI 语言服务提供哪些主要功能？

Azure AI 语言服务提供以下主要功能：

+   语言检测：识别文本所使用的语言。

+   关键短语提取：从文档中识别出重要的词语和短语，以捕捉文本的要点。

+   情绪分析：量化文本的积极或消极程度。

+   命名实体识别 （NER）：识别文本中提到的实体，并将其分类，例如人物、地点、日期时间、组织、地址、电子邮件和网址等。

+   实体链接：通过引用知识库中的文章（如维基百科）来消除同名实体的歧义，从而识别特定的实体。

### 如何使用 Azure AI 语言服务？

要使用 Azure AI 语言服务，用户需要在 Azure 订阅中预配相应的资源。预配完成后，可以通过其“终结点”和“密钥”之一从代码中调用 Azure AI 语言 API。这可以通过将 JSON 格式的请求提交到 REST 接口或使用任何可用的特定编程语言 SDK 来实现。

### “实体识别”和“实体链接”有什么区别？

实体识别主要关注于识别文本中提及的实体，并将其归类到预定义的类别（如人物、地点、组织等）。例如，识别出“Joe”是一个“人物”，“London”是一个“地点”。

实体链接在此基础上更进一步，它解决的是同名实体的歧义问题。例如，当文本中出现“金星”一词时，实体链接会根据上下文将其与知识库中的特定条目（如行星“金星”或女神“维纳斯”）进行关联，提供一个明确的链接（如维基百科链接）。

### 语言检测功能如何处理多语言文本或无法识别的文本？

当文档包含多种语言内容时，语言检测服务会优先显示内容中出现频率最高的语言，但会以较低的置信度评分来反映评估的边际效力。如果分析器无法分析文本内容（例如，由于字符编码问题），则语言名称和 ISO 代码将显示为“（Unknown）”，并且置信度分数为 0。

### 关键短语提取功能适用于哪些类型的文本？

关键短语提取最适合分析大型文档，最大可分析大小为 5,120 个字符。它通过评估文本来识别围绕文档上下文的要点。

### 使用 Azure AI 语言 API 时，是否有关于文档大小或集合数量的限制？

对于语言检测功能，每个文档的大小必须低于 5,120 个字符。此外，每个集合（即每次 API 请求中提交的文档批次）的文档数量限制为 1,000 个项目（ID）。

### 练习：Analyze Text

https://github.com/MicrosoftLearning/mslearn-ai-language/blob/main/Instructions/Labs/01-analyze-text.md

文本分析

```
from dotenv import load_dotenv
import os

# Import namespaces
from azure.core.credentials import AzureKeyCredential
from azure.ai.textanalytics import TextAnalyticsClient

def main():
    try:
        # Get Configuration Settings
        load_dotenv()
        ai_endpoint = os.getenv(&#x27;AI_SERVICE_ENDPOINT&#x27;)
        ai_key = os.getenv(&#x27;AI_SERVICE_KEY&#x27;)

        # Create client using endpoint and key
        credential = AzureKeyCredential(ai_key)
        ai_client = TextAnalyticsClient(endpoint=ai_endpoint, credential=credential)

        # Analyze each text file in the reviews folder
        reviews_folder = &#x27;reviews&#x27;
        for file_name in os.listdir(reviews_folder):
            # Read the file contents
            print(&#x27;\n-------------\n&#x27; + file_name)
            text = open(os.path.join(reviews_folder, file_name), encoding=&#x27;utf8&#x27;).read()
            print(&#x27;\n&#x27; + text)

            # Get language
            detectedLanguage = ai_client.detect_language(documents=[text])[0]
            print(&#x27;\nLanguage: {}&#x27;.format(detectedLanguage.primary_language.name))

            # Get sentiment
            sentimentAnalysis = ai_client.analyze_sentiment(documents=[text])[0]
            print(&quot;\nSentiment: {}&quot;.format(sentimentAnalysis.sentiment))

            # Get key phrases
            phrases = ai_client.extract_key_phrases(documents=[text])[0].key_phrases
            if len(phrases) &gt; 0:
                print(&quot;\nKey Phrases:&quot;)
                for phrase in phrases:
                    print(&#x27;\t{}&#x27;.format(phrase))

            # Get entities
            entities = ai_client.recognize_entities(documents=[text])[0].entities
            if len(entities) &gt; 0:
                print(&quot;\nEntities&quot;)
                for entity in entities:
                    print(&#x27;\t{} ({})&#x27;.format(entity.text, entity.category))

            # Get linked entities
            entities = ai_client.recognize_linked_entities(documents=[text])[0].entities
            if len(entities) &gt; 0:
                print(&quot;\nLinks&quot;)
                for linked_entity in entities:
                    print(&#x27;\t{} ({})&#x27;.format(linked_entity.name, linked_entity.url))

    except Exception as ex:
        print(ex)


if __name__ == &quot;__main__&quot;:
    main()
```

> 这些代码片段的目的是逐步启用 Azure AI 语言服务的不同文本分析功能。

1.  导入命名空间 （Import namespaces）

```
      # import namespacesfrom azure.core.credentials import AzureKeyCredential
from azure.ai.textanalytics import TextAnalyticsClient
```

+   from azure.core.credentials import AzureKeyCredential:

+   **作用**： 这个类用于处理 Azure 服务的身份验证。具体来说，它允许你使用你的 Azure AI 语言服务密钥（API Key）来授权你的应用程序访问该服务。

+   **为什么需要**： Azure 服务需要知道是谁在发出请求，以及该请求是否被授权。AzureKeyCredential 对象会安全地包装你的服务密钥。

+   from azure.ai.textanalytics import TextAnalyticsClient:

+   **作用**： 这是 Azure AI 语言文本分析 SDK 的核心客户端类。你需要创建这个类的一个实例，才能调用文本分析的各种功能，如语言检测、情感分析等。

+   **为什么需要**： 它是与 Azure AI 语言服务进行所有交互的入口点。

1.  **创建客户端 （Create client usi**ng endpoint and key）

```
      # Create client using endpoint and key
credential = AzureKeyCredential(ai_key)
ai_client = TextAnalyticsClient(endpoint=ai_endpoint, credential=credential)
```

+   credential = AzureKeyCredential(ai_key):

+   **作用**： 创建一个 AzureKeyCredential 类的实例。它接收从 .env 文件中加载的 ai_key （你的 Azure AI 语言服务密钥） 作为参数。

+   **为什么需要**： 这个 credential 对象将被传递给 TextAnalyticsClient，用于向 Azure 服务证明你的应用程序有权访问。

+   ai_client = TextAnalyticsClient(endpoint=ai_endpoint, credential=credential):

+   **作用**： 创建 TextAnalyticsClient 类的实例，这个实例就是我们之后用来调用所有文本分析 API 的对象。

+   **参数**：

+   endpoint=ai_endpoint： 指定你的 Azure AI 语言服务的终结点 URL （从 .env 文件加载）。SDK 需要知道向哪个服务器地址发送请求。

+   credential=credential： 传入上一步创建的 credential 对象，用于身份验证。

+   **为什么需要**： 这个 ai_client 对象是实际执行文本分析操作的工具。

1.  获取语言 （Get language）

```
      # Get language
detectedLanguage = ai_client.detect_language(documents=[text])[0]
print(&#x27;\nLanguage: {}&#x27;.format(detectedLanguage.primary_language.name))
```

+   detectedLanguage = ai_client.detect_language(documents=[text])[0]:

+   ai_client.detect_language(...)： 调用 TextAnalyticsClient 对象的 detect_language 方法来检测输入文本的语言。

+   documents=[text]： 这个方法期望接收一个文档列表作为输入。即使我们只分析一个文档 （text变量的内容），也需要将其包装在一个列表中。

+   [0]： detect_language 方法（以及其他许多文本分析方法）返回一个结果列表，每个结果对应输入列表中的一个文档。由于我们只发送了一个文档，所以我们通过索引 [0] 来获取这第一个（也是唯一一个）文档的检测结果。

+   detectedLanguage.primary_language.name:

+   detectedLanguage 是单个文档的语言检测结果对象。

+   它有一个 primary_language 属性，表示最可能的主要语言。

+   primary_language 对象有一个 name 属性，它包含语言的名称 （例如 "English"， "Chinese_Simplified"， "Spanish"）。

+   **作用**： 识别给定文本是用什么语言编写的。

1.  获取情感 （Get sentiment）

```
      # Get sentiment
sentimentAnalysis = ai_client.analyze_sentiment(documents=[text])[0]
print(&quot;\nSentiment: {}&quot;.format(sentimentAnalysis.sentiment))
```

+   sentimentAnalysis = ai_client.analyze_sentiment(documents=[text])[0]:

+   ai_client.analyze_sentiment(...)： 调用情感分析功能。

+   documents=[text] 和 [0]： 与语言检测的逻辑相同，处理单个文档并获取其结果。

+   sentimentAnalysis.sentiment:

+   sentimentAnalysis 是单个文档的情感分析结果对象。

+   它有一个 sentiment 属性，表示文本的整体情感，通常是 "positive" （积极）， "negative" （消极）， "neutral" （中性）， 或 "mixed" （混合）。

+   **作用**： 判断文本所表达的情感是积极的、消极的还是中性的。

1.  获取关键短语 （Get key phrases）

```
      # Get key phrases
phrases = ai_client.extract_key_phrases(documents=[text])[0].key_phrases
if len(phrases) &gt; 0:
    print(&quot;\nKey Phrases:&quot;)for phrase in phrases:
        print(&#x27;\t{}&#x27;.format(phrase))
```

+   phrases = ai_client.extract_key_phrases(documents=[text])[0].key_phrases:

+   ai_client.extract_key_phrases(...)： 调用关键短语提取功能。

+   documents=[text] 和 [0]： 逻辑同上。

+   .key_phrases： 单个文档的关键短语提取结果对象有一个 key_phrases 属性，这是一个包含字符串的列表，每个字符串都是从文本中提取出的一个关键短语。

+   if len(phrases) > 0: ... for phrase in phrases: ...： 检查是否提取到了任何关键短语，如果有，则遍历并打印它们。

+   **作用**： 从文本中识别出主要的谈论点或核心概念。

1.  获取实体 （Get entities）

```
      # Get entities
entities = ai_client.recognize_entities(documents=[text])[0].entities
if len(entities) &gt; 0:
    print(&quot;\nEntities&quot;)for entity in entities:
        print(&#x27;\t{} ({})&#x27;.format(entity.text, entity.category))
```

+   entities = ai_client.recognize_entities(documents=[text])[0].entities:

+   ai_client.recognize_entities(...)： 调用命名实体识别 （NER） 功能。

+   documents=[text] 和 [0]： 逻辑同上。

+   .entities： 单个文档的实体识别结果对象有一个 entities 属性，这是一个 CategorizedEntity 对象列表。

+   entity.text： CategorizedEntity 对象包含被识别出的实体文本 （例如 "Microsoft"， "London"）。

+   entity.category： CategorizedEntity 对象还包含该实体的类别 （例如 "Organization"， "Location"， "Person"， "DateTime"）。

+   **作用**： 识别并分类文本中提到的已知实体，如人名、地名、组织机构名、日期、数量等。

1.  获取链接实体 （Get linked entities）

```
      # Get linked entities
entities = ai_client.recognize_linked_entities(documents=[text])[0].entities
if len(entities) &gt; 0:
    print(&quot;\nLinks&quot;)for linked_entity in entities:
        print(&#x27;\t{} ({})&#x27;.format(linked_entity.name, linked_entity.url))
```

+   entities = ai_client.recognize_linked_entities(documents=[text])[0].entities： （注意：这里变量名 entities 被重用了，在实际项目中为了清晰，可能会使用不同的名称，如 linked_entities_result）

+   ai_client.recognize_linked_entities(...)： 调用链接实体识别功能。

+   documents=[text] 和 [0]： 逻辑同上。

+   .entities： 单个文档的链接实体识别结果对象有一个 entities 属性，这是一个 LinkedEntity 对象列表。

+   linked_entity.name： LinkedEntity 对象包含实体在知识库中的规范名称 （例如 "Microsoft Corporation"）。

+   linked_entity.url： LinkedEntity 对象包含一个指向该实体在知识库 （通常是维基百科） 中的 URL 链接。

+   **作用**： 识别文本中的实体，并将其链接到知名知识库（如维基百科）中的对应条目，有助于消除歧义并提供更丰富的上下文信息。

> 总结来说，这些添加的代码块逐步构建了一个完整的文本分析流程：首先配置并创建与 Azure AI 语言服务的连接，然后针对每个评论文件，依次调用服务的不同功能来提取语言、情感、关键短语、命名实体和链接实体信息，并将结果打印出来。每个功能都通过 ai_client 对象的方法调用，并遵循相似的模式（传入文档列表，获取结果列表中的第一个元素）。

输出

```
-------------
review1.txt

Good Hotel and staff
The Royal Hotel, London, UK
3/2/2018
Clean rooms, good service, great location near Buckingham Palace and Westminster Abbey, and so on. We thoroughly enjoyed our stay. The courtyard is very peaceful and we went to a restaurant which is part of the same group and is Indian ( West coast so plenty of fish) with a Michelin Star. We had the taster menu which was fabulous. The rooms were very well appointed with a kitchen, lounge, bedroom and enormous bathroom. Thoroughly recommended.

Language: English

Sentiment: positive

Key Phrases:
        The Royal Hotel
        Good Hotel
        good service
        great location
        Buckingham Palace
        Westminster Abbey
        same group
        West coast
        Michelin Star
        taster menu
        enormous bathroom
        Clean rooms
        staff
        London
        UK
        stay
        courtyard
        restaurant
        part
        plenty
        fish
        kitchen
        lounge
        bedroom

Entities
        Hotel (Location)
        staff (PersonType)
        Royal Hotel, London, UK (Address)
        3/2/2018 (DateTime)
        Buckingham Palace (Location)
        Westminster Abbey (Location)
        courtyard (Location)
        restaurant (Location)
        Indian (PersonType)
        West coast (Location)
        fish (Product)
        taster (Product)
        rooms (Location)
        kitchen (Location)
        lounge (Location)
        bedroom (Location)
        bathroom (Location)

Links
        GOOD Music (https://en.wikipedia.org/wiki/GOOD_Music)
        Hotel (https://en.wikipedia.org/wiki/Hotel)
        The Royal Hotel (https://en.wikipedia.org/wiki/The_Royal_Hotel)
        London (https://en.wikipedia.org/wiki/London)
        Buckingham Palace (https://en.wikipedia.org/wiki/Buckingham_Palace)
        Westminster Abbey (https://en.wikipedia.org/wiki/Westminster_Abbey)
        India (https://en.wikipedia.org/wiki/India)
        West Coast Main Line (https://en.wikipedia.org/wiki/West_Coast_Main_Line)
        Michelin Guide (https://en.wikipedia.org/wiki/Michelin_Guide)

-------------
review2.txt

Tired hotel with poor service
The Royal Hotel, London, United Kingdom
5/6/2018
This is a old hotel (has been around since 1950&#x27;s) and the room furnishings are average - becoming a bit old now and require changing. The internet didn&#x27;t work and had to come to one of their office rooms to check in for my flight home. The website says it&#x27;s close to the British Museum, but it&#x27;s too far to walk.

Language: English

Sentiment: negative

Key Phrases:
        The Royal Hotel
        Tired hotel
        old hotel
        poor service
        United Kingdom
        room furnishings
        office rooms
        flight home
        British Museum
        London
        changing
        internet
        website
        1950

Entities
        hotel (Location)
        The Royal Hotel (Organization)
        London (Location)
        United Kingdom (Location)
        5/6/2018 (DateTime)
        hotel (Location)
        since 1950&#x27;s (DateTime)
        furnishings (Product)
        now (DateTime)
        internet (Skill)
        one (Quantity)
        office rooms (Location)
        flight (Event)
        home (Location)
        British Museum (Location)

Links
        The Royal Hotel (https://en.wikipedia.org/wiki/The_Royal_Hotel)
        London (https://en.wikipedia.org/wiki/London)
        British Museum (https://en.wikipedia.org/wiki/British_Museum)

-------------
review3.txt

Good location and helpful staff, but on a busy road.
The Lombard Hotel, San Francisco, USA
8/16/2018
We stayed here in August after reading reviews. We were very pleased with location, just behind Chestnut Street, a cosmopolitan and trendy area with plenty of restaurants to choose from. The
Marina district was lovely to wander through, very interesting houses. Make sure to walk to the San Francisco Museum of Fine Arts and the Marina to get a good view of Golden Gate bridge and the city. On a bus route and easy to get into centre. Rooms were clean with plenty of room and staff were friendly and helpful. The only down side was the noise from Lombard Street so ask to have a room furthest away from traffic noise.

Language: English

Sentiment: mixed

Key Phrases:
        Golden Gate bridge
        The Lombard Hotel
        The Marina district
        San Francisco Museum
        Lombard Street
        busy road
        Chestnut Street
        trendy area
        interesting houses
        Fine Arts
        good view
        bus route
        down side
        Good location
        helpful staff
        traffic noise
        USA
        We
        August
        reviews
        cosmopolitan
        plenty
        restaurants
        city
        centre
        Rooms

Entities
        staff (PersonType)
        Hotel (Location)
        San Francisco (Location)
        USA (Location)
        8/16/2018 (DateTime)
        August (DateTime)
        Chestnut Street (Address)
        restaurants (Location)
        Marina district (Location)
        houses (Location)
        San Francisco Museum of Fine Arts (Location)
        Marina (Location)
        Golden Gate bridge (Location)
        city (Location)
        centre (Location)
        Rooms (Location)
        staff (PersonType)
        Lombard Street (Address)
        room (Location)

Links
        Lombardy (https://en.wikipedia.org/wiki/Lombardy)
        Hotel (https://en.wikipedia.org/wiki/Hotel)
        San Francisco (https://en.wikipedia.org/wiki/San_Francisco)
        Chestnut Street (Philadelphia) (https://en.wikipedia.org/wiki/Chestnut_Street_(Philadelphia))
        Marina District, San Francisco (https://en.wikipedia.org/wiki/Marina_District,_San_Francisco)
        Museum of Fine Arts, Boston (https://en.wikipedia.org/wiki/Museum_of_Fine_Arts,_Boston)
        Golden Gate Bridge (https://en.wikipedia.org/wiki/Golden_Gate_Bridge)
        Room (https://en.wikipedia.org/wiki/Room)
        Lombard Street (San Francisco) (https://en.wikipedia.org/wiki/Lombard_Street_(San_Francisco))

-------------
review4.txt

Very noisy and rooms are tiny
The Lombard Hotel, San Francisco, USA
9/5/2018
Hotel is located on Lombard street which is a very busy SIX lane street directly off the Golden Gate Bridge. Traffic from early morning until late at night especially on weekends. Noise would not be so bad if rooms were better insulated but they are not. Had to put cotton balls in my ears to be able to sleep--was too tired to enjoy the city the next day. Rooms are TINY. I picked the room because it had two queen size beds--but the room barely had space to fit them. With family of four in the room it was tight. With all that said, rooms are clean and they&#x27;ve made an effort to update them. The hotel is in Marina district with lots of good places to eat, within walking distance to Presidio. May be good hotel for young stay-up-late adults on a budget


Language: English

Sentiment: mixed

Key Phrases:
        two queen size beds
        busy SIX lane street
        Golden Gate Bridge
        The Lombard Hotel
        Lombard street
        San Francisco
        early morning
        cotton balls
        Marina district
        good places
        walking distance
        late adults
        good hotel
        rooms
        USA
        Traffic
        night
        weekends
        Noise
        ears
        city
        TINY
        space
        family
        effort
        lots
        Presidio
        young
        budget

Entities
        rooms (Location)
        Hotel (Location)
        San Francisco (Location)
        USA (Location)
        9/5/2018 (DateTime)
        Hotel (Location)
        Lombard street (Address)
        SIX (Quantity)
        Golden Gate Bridge (Location)
        early morning until late at night (DateTime)
        weekends (DateTime)
        rooms (Location)
        cotton balls (Product)
        city (Location)
        the next day (DateTime)
        Rooms (Location)
        room (Location)
        two (Quantity)
        beds (Product)
        room (Location)
        four (Quantity)
        room (Location)
        rooms (Location)
        hotel (Location)
        Marina district (Location)
        Presidio (Location)
        hotel (Location)
        young (PersonType)
        adults (PersonType)

Links
        Lombard, Illinois (https://en.wikipedia.org/wiki/Lombard,_Illinois)
        Hotel (https://en.wikipedia.org/wiki/Hotel)
        San Francisco (https://en.wikipedia.org/wiki/San_Francisco)
        Lombard Street (San Francisco) (https://en.wikipedia.org/wiki/Lombard_Street_(San_Francisco))
        Golden Gate Bridge (https://en.wikipedia.org/wiki/Golden_Gate_Bridge)
        Traffic (https://en.wikipedia.org/wiki/Traffic)
        Noise rock (https://en.wikipedia.org/wiki/Noise_rock)
        Room (https://en.wikipedia.org/wiki/Room)
        Marina District, San Francisco (https://en.wikipedia.org/wiki/Marina_District,_San_Francisco)
        Presidio of San Francisco (https://en.wikipedia.org/wiki/Presidio_of_San_Francisco)
        May (https://en.wikipedia.org/wiki/May)

-------------
review5.txt

Un hôtel agréable
L&#x27;Hotel Buckingham, Londres, UK
J’adore cet hôtel. Le personnel est très amical et les chambres sont confortables.

Language: French

Sentiment: positive

Key Phrases:
        hôtel agréable
        L&#x27;Hotel Buckingham
        Londres
        UK
        personnel
        chambres

Entities
        hôtel (Location)
        Buckingham (Organization)
        Londres (Location)
        UK (Location)
        hôtel (Location)
        personnel (PersonType)
        chambres (Location)

Links
        United Nations (https://en.wikipedia.org/wiki/United_Nations)
        L&#x27;Hôtel (https://en.wikipedia.org/wiki/L&#x27;Hôtel)
        Buckingham (https://en.wikipedia.org/wiki/Buckingham)
        London (https://en.wikipedia.org/wiki/London)
        United Kingdom (https://en.wikipedia.org/wiki/United_Kingdom)
```

# 使用 Azure AI 语言创建问题解答解决方案

https://learn.microsoft.com/zh-cn/training/modules/create-question-answer-solution-ai-language/

### 什么是 Azure AI 语言中的问答功能？

Azure AI 语言中的问答功能允许用户创建和管理“知识库”，其中包含问答对。这些知识库旨在通过自然语言查询进行访问，并能为客户端应用程序（如聊天机器人）提供答案。它能够从多种现有来源构建知识库，包括常见问题解答 （FAQ） 网站、结构化文本文件（如手册或用户指南），以及预定义的闲聊问答对。此功能是 QnA Maker 服务的较新版本。

### 如何创建和管理知识库？

创建知识库通常通过 Language Studio Web 界面完成，尽管也可以使用 REST API 或 SDK 编写代码。创建过程包括登录 Azure 门户，搜索并选择 Azure AI 服务，然后创建语言服务资源，同时启用问答功能并选择一个 Azure AI 搜索资源来托管知识库索引。在 Language Studio 中，用户可以创建自定义问答项目，并添加数据源（如 FAQ 网页 URL、结构化文本文件或预定义的闲聊数据集）来填充知识库。之后，可以在门户中编辑问答对。

### 如何使用已发布的知识库？

一旦知识库发布，客户端应用程序可以通过 REST 接口使用它。一个最小的请求正文包含一个问题，并可以指定其他参数，如返回的最大答案数 （top）、答案的置信度分数阈值 （scoreThreshold），以及用于限制答案的严格过滤器 （strictFilters）。响应会包括知识库中找到的最接近的问题匹配、相关的答案、置信度分数以及其他元数据。

### 什么是多轮对话，如何实现？

多轮对话是指在提供明确答案之前需要询问后续问题以从用户那里获取更多信息的情况。例如，当一个初始问题“如何取消预订？”出现时，由于预订可能指酒店或航班，系统会通过后续提示来澄清细节。多轮对话可以通过在导入现有网页或文档时启用，或者通过为现有问答对显式定义后续提示和响应来实现。在定义后续提示时，可以链接到知识库中的现有答案，或专门为后续定义新答案，并且可以限制链接答案仅在由原始问题发起的多轮对话上下文中显示。

### 如何提高问答性能？

知识库的性能可以通过“主动学习”和定义“同义词”来改进。主动学习有助于持续改进，通过审查和接受或拒绝用户提出的不同表达方式但含义相同的问题的替代短语来提高准确性。默认情况下，主动学习是启用的，并且可以在 Language Studio 的“审阅建议”窗格中管理。同义词功能允许用户定义表示相同内容的多个不同词语（例如，“预留”和“预订”），从而帮助问答服务在用户使用不同术语时也能找到相应的答案。同义词通过 REST API 以 JSON 格式提交。

### 如何测试和部署知识库？

定义知识库后，用户可以在 Language Studio 中对其自然语言模型进行训练和交互式测试。这包括提交问题并查看返回的答案，同时检查置信度分数和其他潜在答案。一旦对知识库的性能满意，就可以直接从 Language Studio 将其部署到 REST 终结点，供客户端应用程序提交问题和接收答案。

### 问答功能和 Azure AI 语言理解有什么区别和联系？

问答功能和 Azure AI 语言理解（Language Understanding）都允许定义可以使用自然语言表达式查询的语言模型，但它们旨在解决的用例不同。问答功能主要用于用户提交问题并需要静态答案的场景，它通过自然语言理解将问题与知识库中的答案进行匹配。而语言理解则用于用户提交语句并需要适当的响应或操作的场景，它解释语句，将其与意图匹配，并识别实体。虽然它们用途不同，但它们是互补的，可以结合使用以构建全面的自然语言解决方案。

### 问答知识库可以从哪些现有源创建？

问答知识库可以从多种现有源创建，以便快速填充内容并构建实用的问答系统。这些来源包括：

+   包含常见问题解答 （FAQ） 文档的网站：可以直接从现有的 FAQ 页面导入问答对。

+   包含结构化文本的文件：例如手册、用户指南或其他包含结构化信息（从中可以提取问答对）的文档。

+   内置的闲聊问题和答案对：这些是预定义的对话交流，用于处理常见的、非特定于领域的闲聊问题，使机器人能够进行更自然的对话。

### 实验：Create a Question Answering Solution

https://github.com/MicrosoftLearning/mslearn-ai-language/blob/main/Instructions/Labs/02-qna.md

# 生成对话语言理解模型

https://learn.microsoft.com/zh-cn/training/modules/build-language-understanding-model/

### 什么是 Azure AI 语言服务？

Azure AI 语言服务是一项功能强大的服务，旨在理解人类语言，提供各种功能以改善用户沟通、理解传入信息，并深入分析用户的言语、意图和询问。它分为两大类：预配置功能和学习功能，后者需要用户构建和训练模型。

### Azure AI 语言服务有哪些预配置功能？

Azure AI 语言服务提供了多种无需训练模型即可使用的预配置功能。这些功能包括：

+   摘要：总结文档或对话的关键信息。

+   命名实体识别（NER）：识别和提取文本中的实体，如人名、地点或公司。

+   个人身份信息（PII）检测：识别、分类和编辑敏感信息，如电子邮件地址或家庭住址。

+   关键短语提取：快速从文本中识别主要概念。

+   情感分析：判断文本或文档的情感是正面还是负面。

+   语言检测：识别所提供文档的语言。

### Azure AI 语言服务中的“学习功能”指的是什么？

“学习功能”是指那些需要用户标记数据、训练和部署模型才能在应用程序中使用的功能。这些功能允许用户自定义预测或提取的信息。学习模型的准确性高度依赖于数据质量、标记方式和训练数据的多样性。

### 学习功能包含哪些具体服务？

学习功能包括：

+   对话语言理解（CLU）：Azure AI 语言的核心自定义功能，帮助用户构建自定义自然语言理解模型，以预测整体意图并从输入语言中提取关键信息。

+   自定义命名实体识别：根据自定义标记数据从非结构化文本中提取特定实体。

+   自定义文本分类：允许用户将文本或文档分类到自定义组中。

+   问答：主要是一种预配置功能，但需要基于 FAQ 或手册等文档创建问答对，然后进行部署以回答用户问题。

### 如何构建和部署一个学习模型（例如对话语言理解模型）？

构建和部署学习模型是一个迭代过程，通常涉及以下步骤：

1.  创建 Azure AI 语言资源：在 Azure 门户中创建语言服务资源，获取密钥和终结点。

1.  构建模型：

+   使用 Language Studio：这是一个更直观的工具，可以在其中创建项目并进行模型的构建、训练和部署。

+   使用 REST API：通过向相应的 URI 提交请求来创建项目、导入数据、训练、部署和查询模型。这些任务是异步进行的。

1.  训练模型：模型从示例言语中学习意图和实体。

1.  测试模型：通过交互方式或使用带有已知标签的测试数据集来测试模型。

1.  部署模型：将训练好的模型部署到公共终结点，以便客户端应用程序可以使用它。

1.  审查预测：持续审查预测并优化言语以提高模型的准确性。

每次调用 Azure AI 语言资源都需要使用资源密钥进行身份验证。

### 在构建语言理解模型时，“意图”、“言语”和“实体”分别代表什么？

+   言语（Utterances）：用户在与使用语言模型的应用程序交互时可能输入的短语。

+   意图（Intents）：表示用户希望执行的任务或操作，或者更简单地表示言语的含义。模型通过定义意图并将其与一个或多个言语关联来创建。例如，“打开灯”和“打开风扇”都可能属于“TurnOnDevice”意图。

+   实体（Entities）：用于为意图添加特定的上下文信息。例如，在“打开 灯”中，“灯”就是一个实体（设备）。

### 如何通过模式识别来优化模型的理解，并使用预构建实体？

+   模式识别：当多个意图的言语语法相似时，可以使用言语模式来消除歧义，同时最大限度地减少示例言语的数量。例如，“打开厨房灯”、“厨房开着灯吗？”和“关闭厨房灯”虽然语法相似，但代表不同的意图（TurnOnDevice， GetDeviceStatus， TurnOffDevice），通过定义模式，模型可以根据格式和标点正确分类意图。

+   预构建实体组件：Azure AI 语言服务提供了预构建的实体组件，可以自动检测常见的实体类型，例如数字、电子邮件、URL 或特定选项。使用这些组件可以显著缩短开发时间，因为它们无需用户提供示例来训练模型。一个实体最多可以包含 5 个预构建组件。

### 如何查询部署的模型？

查询部署的模型可以通过以下两种方式实现：

+   使用 SDK：可以通过 C# 或 Python 中的 SDK 创建客户端，然后使用它来调用相应的终结点。不同的语言功能可能需要不同的请求构建和发送方式。

+   使用 REST API：向指定的 URL 发送带有相应正文的 POST 请求。对于内置功能（如语言检测或情感分析），需要查询 analyze-text 终结点，并在请求正文中指定 kind 参数。对于自定义的对话语言理解模型，请求将发送到 analyze-conversations 终结点。

无论哪种方法，所有请求都需要在请求头中包含 Ocp-Apim-Subscription-Key 作为身份验证凭据。查询响应通常以 JSON 格式返回，包含模型预测的结果。

### 实验：Create a language understanding model with the Language service

https://github.com/MicrosoftLearning/mslearn-ai-language/blob/main/Instructions/Labs/03-language-understanding.md

# 创建自定义文本分类解决方案

https://learn.microsoft.com/zh-cn/training/modules/custom-text-classification/

### 什么是自定义文本分类？

自定义文本分类是 Azure AI 语言服务中的一个功能，它允许开发人员为文本文件分配自定义定义的标签（或类别）。例如，电子游戏摘要可以根据其内容被归类为“冒险”、“策略”、“动作”或“体育”。

### 单标签分类和多标签分类之间有什么区别？

自定义文本分类项目分为两类：

+   单标签分类：每个文件只能分配一个类别。例如，一个游戏摘要只能被归类为“冒险”或“策略”，不能同时是两者。

+   多标签分类：每个文件可以分配多个类别。例如，一个游戏摘要可以同时被归类为“冒险”和“策略”。

主要区别在于数据标记、模型改进的复杂性以及 API 有效负载的结构。多标签项目由于需要处理标签的组合排列，因此在数据标记和模型改进方面更具挑战性。

### 为什么正确标记数据很重要，尤其是在多标签项目中？

正确标记数据与模型的性能直接相关。数据集的质量越高、越明确、越多样化，模型就会越准确。对于多标签项目，复杂性增加意味着数据必须保持清晰，并为模型学习提供良好分布的可能输入。不正确的标记会导致模型预测错误，影响模型的召回率、精准率和 F1 分数。

### 如何评估和改进文本分类模型的性能？

评估模型性能不仅要看预测的正确性。还会关注两种类型的错误：

+   假正：模型预测了某个标签，但文件实际上没有该标签。

+   假负：模型没有预测到某个标签，但文件实际上有该标签。

这些错误会转换为三个关键度量值：

+   召回率：所有实际标签中被正确识别的标签数量。

+   精准率：预测的标签中有多少是正确的。

+   F1 分数：召回率和精准率的综合函数，提供平衡的评估分数。

为了改进模型，尤其是在多标签项目中，需要识别哪些类别（或类别组合）表现不佳，然后找到更多高质量、多样化的数据来重新训练模型，以帮助它更好地学习区分这些类别。

### Azure AI 语言项目生命周期包含哪些步骤？

Azure AI 语言项目的生命周期包括以下步骤：

1.  定义标签：确定要分类的数据以及可能的标签。

1.  标记数据：为现有数据分配相应的标签，这是模型学习的基础。

1.  训练模型：使用标记的数据来训练模型。

1.  查看模型：评估训练后的模型结果，包括精确率和召回率。

1.  改进模型：根据评估结果，通过添加更多数据或调整数据分布来提升模型性能。

1.  部署模型：将表现良好的模型部署为可用的 API 服务。

1.  对文本进行分类：使用部署的模型对新的文本进行分类。

### 在训练模型时，如何拆分数据集？

在标记数据时，可以指定每个文件属于哪个数据集：

+   训练数据集：用于实际训练模型，将数据和标签输入到机器学习算法中。建议占标记数据的约 80%。

+   测试数据集：用于在模型训练后验证模型。Azure 会将测试数据集中的数据提交给模型，并将其输出与实际标记进行比较，以评估模型的性能。

在训练模型时，有两种拆分数据集的选项：

+   自动拆分：Azure 随机将所有数据按指定百分比分为训练集和测试集。适用于大型、数据一致或类别分布广泛的数据集。

+   手动拆分：用户手动指定每个文件应属于训练集还是测试集。适用于小型数据集，以确保正确的类别分布和数据多样性。

### Azure AI 语言的 REST API 如何用于文本分类任务？

Azure AI 语言服务提供了 REST API，允许通过编程方式（例如命令行界面）管理和使用文本分类项目，与 Language Studio 的 GUI 功能相同。使用 API 的模式通常是异步的：

1.  提交初始请求：发送 POST 请求到相应的终结点（例如训练模型或提交分类文本），并包含所需的 JSON 正文和认证密钥。

1.  获取状态/结果：从初始请求的响应头中获取操作位置 URL，然后定期发送 GET 请求到该 URL，直到操作状态显示为“succeeded”以获取最终结果。

这种异步模式允许在后台处理长时间运行的任务，而客户端可以轮询状态。

### Azure AI 语言项目有哪些部署选项，以及部署模型的优势是什么？

Azure AI 语言允许每个项目同时创建多个模型和多个部署，每个部署都有其唯一的名称。部署模型的优势包括：

+   并排测试模型：可以同时部署和比较两个或更多模型的性能。

+   比较数据集拆分的影响：评估不同的数据集拆分策略如何影响模型性能。

+   部署多个版本的模型：管理和测试模型的不同迭代或版本。

每个项目最多可以有 10 个部署名称。在提交分类任务时，需要指定要使用的部署名称。

### 实验：Custom text classification

https://github.com/MicrosoftLearning/mslearn-ai-language/blob/main/Instructions/Labs/04-text-classification.md

# 自定义命名实体识别

https://learn.microsoft.com/zh-cn/training/modules/custom-name-entity-recognition/

### 什么是自定义命名实体识别 （NER）？

自定义命名实体识别 （NER） 是一种 Azure AI 服务，能够查看文档并识别、提取用户定义的特定实体。与内置 NER 服务识别预定义实体（如人物、地点、组织等）不同，自定义 NER 允许用户根据特定需求定义和提取独特的实体，例如银行对账单中的特定条款或审计政策中的文本。它属于 Azure AI 服务中的 Azure AI 语言的一部分。

### 自定义 NER 与内置 NER 有何不同？

Azure AI 语言提供了两种 NER 功能：内置 NER 和自定义 NER。内置 NER 能够识别和提取通用的预定义实体，如人员、位置、组织或 URL，配置简单。而自定义 NER 适用于内置服务无法满足需求，或者用户只想提取特定实体的情况。用户可以根据需要构建简单或复杂的自定义 NER 模型，例如处理特定法律或银行数据、增强目录搜索或在审计策略中查找特定文本。

### 构建自定义 NER 模型的主要生命周期步骤有哪些？

构建自定义 NER 模型通常遵循七个关键步骤：

1.  定义实体：明确需要识别和提取的数据和实体。

1.  标记数据：在现有数据集中标注文本，将其与相应的实体关联。这是模型准确性的关键。

1.  训练模型：使用标记好的数据训练模型，使其学习如何识别定义的实体。

1.  查看模型：评估训练结果，通过精度和召回率分数了解模型的性能，识别需要改进的实体。

1.  改进模型：分析未被识别或错误提取的实体，添加更多数据或调整策略以提高模型性能。

1.  部署模型：当模型性能达到要求时，将其部署为可通过 API 访问的服务。

1.  提取实体：通过 API 调用已部署的模型来实际提取实体。

### 在选择数据和优化实体时有哪些重要考虑因素？

为了获得最佳模型性能，需要使用高质量的数据和明确定义的实体类型：

+   数据质量：

+   多样性：数据集应尽可能多样化，代表实际数据的预期分布，包含来自不同来源和格式的示例。

+   分布：使用适当的文档类型分布，避免模型学习不正确的关系。

+   准确性：使用尽可能接近真实数据的数据，尽管可以使用假数据启动训练过程，但真实数据更关键。

+   实体优化：

+   明确性：实体应尽可能明确定义，避免模糊不清的情况，例如银行账单上相邻的两个名称。如果不可避免，需提供更多示例供模型学习区分。

+   独特性：保持实体的独特性将显著提高模型性能。例如，将“联系人信息”分解为更具体的实体，如“电话”、“电子邮件”和“社交媒体”。

### 如何在自定义 NER 中提交实体提取任务？

要提交实体提取任务，需要向 API 发送一个 JSON 请求体。对于自定义 NER，JSON 有效负载中的任务类型为 CustomEntityRecognition。请求中需要指定 projectName（项目名称）和 deploymentName（部署名称）。API 将根据这些参数和提供的文档内容执行实体提取操作。

### 如何评估自定义 NER 模型的性能？

模型训练后，可以通过“视图模型详细信息”页面评估其性能。评估主要基于三个指标：

+   精准率：成功识别实体次数与尝试的所有识别次数的比率。高精准率表示只要识别出实体，其标签就是正确的。

+   召回率：成功实体识别与文档中实际实体数的比率。高召回率表示模型能够很好地识别出存在的实体，无论标签是否正确。

+   F1 分数：精准率和召回率的组合分数，提供单一的整体性能指标。

这些分数可以针对每个实体以及整个模型进行查看。此外，“混淆矩阵”视图提供了一个可视化表格，显示所有实体的性能，帮助识别需要改进的地方。

### 如果模型的精准率高但召回率低，或者精准率低但召回率高，这意味着什么？

+   精准率高但召回率低：这表示模型并不总是能识别出文档中所有的实体（低召回率），但一旦它识别并提取了实体，所分配的标签通常是正确的（高精准率）。这种情况下，需要改进的是模型识别实体的能力，确保它能捕获更多的目标实体。

+   精准率低但召回率高：这表示模型能够很好地识别出文档中存在的实体（高召回率），但它可能无法准确地将这些实体标记为正确的类型（低精准率）。这种情况下，需要改进的是模型为识别到的实体分配正确标签的能力。

理想情况下，我们希望模型在精准率和召回率方面都表现良好。

### 自定义 NER 项目有哪些主要限制？

Azure AI 语言服务对自定义 NER 项目施加了以下限制：

+   训练数据：至少需要 10 个文件，但不超过 100,000 个文件。

+   部署：每个项目最多可以有 10 个部署名称。

+   API 创作：用于创建项目、训练和部署模型的 API，限制为每分钟 10 个 POST 请求和 100 个 GET 请求。

+   分析 API：用于实际提取实体的工作，限制为 20 个 GET 或 POST 请求。

+   项目数量：每个项目只能有 1 个存储账户，每个资源最多 500 个项目，每个项目最多 50 个训练模型。

+   实体定义：每个实体名称最多 500 个字符，最多可以定义 200 种实体类型。

这些限制有助于管理资源和确保服务稳定性。

### 实验：Extract custom entities

https://github.com/MicrosoftLearning/mslearn-ai-language/blob/main/Instructions/Labs/05-extract-custom-entities.md

# 使用 Azure AI 翻译服务来翻译文本

https://learn.microsoft.com/zh-cn/training/modules/translate-text-with-translator-service/

### 什么是 Azure AI Translator，它提供哪些核心功能？

Azure AI Translator 是一种多语言文本翻译 API，它提供语言检测、一对多翻译和脚本音译（将文本从其原生脚本转换为替代脚本）等功能。

### 如何使用 Azure AI Translator 进行语言检测？

要检测文本所用的语言，可以使用 REST API 的 Detect 函数。您需要向 https://api.cognitive.microsofttranslator.com/detect?api-version=3.0 终结点提交包含待检测文本的 JSON 对象。响应将指示文本的语言及其置信度分数。

### 如何使用 Azure AI Translator 进行文本翻译？

要将文本从一种语言翻译到另一种语言，请使用 Translate 函数。在您的请求中，您需要指定源语言（通过 from 参数）和一个或多个目标语言（通过 to 参数）。例如，您可以将日语文本翻译成英语和法语。

### 音译与翻译有何不同，Azure AI Translator 如何处理音译？

翻译是将文本从一种语言转换为另一种语言，而音译是将文本从其原生脚本转换为不同的脚本，但保留其原始语言。例如，您可以将日文（使用平假名脚本）音译为拉丁脚本，使其以类似于英文文本的方式呈现。这通过 Transliterate 函数实现，需要指定 fromScript 和 toScript 参数。

### 如何创建和使用自定义翻译模型？

如果 Azure AI Translator 的默认翻译模型无法满足特定行业或企业的术语需求，您可以创建自定义模型。这涉及在自定义翻译门户中创建一个工作区、项目，上传训练数据并训练模型，然后进行测试和发布。创建自定义模型后，会分配一个唯一的“类别 ID”，您可以在调用 Azure AI Translator API 时使用 category 参数来指定它，从而使其使用您的自定义模型进行翻译。

### Azure AI Translator 的 Translate 函数提供了哪些额外的翻译选项？

Translate 函数支持多种参数来影响输出：

+   文本对齐方式 （includeAlignment=true）：此选项在源文本和翻译文本之间提供字符对齐信息，有助于理解字符映射关系。

+   句子长度 （includeSentenceLength=true）：提供源句子和翻译句子的长度信息，有助于在用户界面中更好地展示翻译结果。

+   不雅内容筛选 （profanityAction）：允许您控制如何处理不雅内容，选项包括：NoAction（不雅内容照常翻译）、Deleted（翻译中省略不雅内容）和 Marked（使用星号或 XML 标签标记不雅内容）。

### 在使用 Azure AI Translator 服务之前需要进行哪些准备？

在使用 Azure AI Translator 服务之前，您必须在 Azure 订阅中为其预配资源。这可以是单服务 Azure AI Translator 资源，也可以是多服务 Azure AI 服务资源中的文本翻译 API。预配资源后，您可以使用部署资源的“位置”和“订阅密钥”从代码中调用 API。

### 如何向 Azure AI Translator API 发送请求？

您可以通过将 JSON 格式的请求提交到 REST 接口或使用任何可用的特定于编程语言的 SDK 来调用 Azure AI Translator API。请求通常是 POST 请求，包含 api-version、to（目标语言）和可选的 category（用于自定义模型）等参数，以及 Ocp-Apim-Subscription-Key 和 Content-Type 等必需的请求头。请求正文是一个 JSON 对象数组，其中包含要翻译的文本。

### 实验：Translate Text

https://github.com/MicrosoftLearning/mslearn-ai-language/blob/main/Instructions/Labs/06-translate-text.md

# 使用 Azure AI 服务创建启用语音的应用

https://learn.microsoft.com/zh-cn/training/modules/create-speech-enabled-apps/

### 使用 Azure AI 语音服务需要预置哪些资源？

在使用 Azure AI 语音服务之前，您需要在 Azure 订阅中创建 Azure AI 语音资源。这可以是专用的 Azure AI 语音资源，也可以是多服务 Azure AI 服务资源。创建资源后，您需要资源的部署位置（例如 eastus）和分配给资源的密钥之一。这些信息可以在 Azure 门户中资源的“密钥和终结点”页面上找到。

### Azure AI 语音服务在语音识别方面提供哪些核心功能？

Azure AI 语音服务通过以下功能支持语音识别：

+   实时听录：使用实时音频输入的中间结果进行即时听录。

+   快速听录：对于具有可预测延迟的情况，同步输出最快。

+   批量听录：高效处理大量预录制的音频。

+   自定义语音：针对特定领域和条件的具有增强准确性的模型。

### 如何使用 Azure AI 语音 SDK 执行语音转文本操作？

使用 Azure AI 语音 SDK 执行语音转文本操作通常遵循以下模式：

1.  使用 SpeechConfig 对象封装连接到 Azure AI 语音资源所需的信息，特别是资源的位置和密钥。

1.  （可选）使用 AudioConfig 定义要转录的音频输入源。默认情况下是系统麦克风，但也可以指定音频文件。

1.  使用 SpeechConfig 和 AudioConfig 创建 SpeechRecognizer 对象，作为语音转文本 API 的代理客户端。

1.  使用 SpeechRecognizer 对象的方法调用基础 API 函数，例如 RecognizeOnceAsync（） 方法用于异步转录单个口述话语。

1.  处理来自 Azure AI 语音服务的响应。成功时，SpeechRecognitionResult 对象的 Reason 属性为 RecognizedSpeech，Text 属性包含转录结果。

### 除了语音转文本，Azure AI 语音服务还支持哪些语音合成相关的 API？

与语音转文本 API 类似，Azure AI 语音服务提供用于语音合成的其他 REST API：

+   文本转语音 API：这是执行语音合成的主要方法。

+   批量合成 API：旨在支持将大量文本转换为音频的批处理操作，例如，从源文本生成有声读物。

实际上，大多数支持语音的交互式应用程序都是通过特定于语言的 SDK 使用 Azure AI 语音服务来实现的。

### 使用 Azure AI 语音 SDK 执行文本转语音操作的流程是怎样的？

使用 Azure AI 语音 SDK 实现语音合成的模式类似于语音识别：

1.  使用 SpeechConfig 对象封装连接到 Azure AI 语音资源所需的信息，特别是资源的位置和密钥。

1.  （可选）使用 AudioConfig 定义要合成语音的输出设备。默认情况下是系统扬声器，但也可以指定音频文件，或者通过将此值显式设置为 null 值直接处理返回的音频流对象。

1.  使用 SpeechConfig 和 AudioConfig 创建 SpeechSynthesizer 对象，作为文本转语音 API 的代理客户端。

1.  使用 SpeechSynthesizer 对象的方法调用基础 API 函数，例如 SpeakTextAsync（） 方法用于将文本转换为语音音频。

1.  处理来自 Azure AI 语音服务的响应。成功合成语音后，SpeechSynthesisResult 对象的 Reason 属性设置为 SynthesizingAudioCompleted，AudioData 属性包含音频流。

### 什么是语音合成标记语言（SSML），它提供了哪些优势？

语音合成标记语言（SSML）是一种基于 XML 的语法，用于描述要生成的语音的特征。虽然 Azure AI 语音 SDK 允许提交纯文本以合成语音，但 SSML 提供了对语音输出声音方式的更好控制，使您能够：

+   使用神经语音时指定说话风格（例如“兴奋”或“快乐”）。

+   插入停顿或沉默。

+   指定音素（拼音发音）。

+   调整声音的韵律（影响音调、音色和说话速率）。

+   使用常见的“say-as”规则，例如将字符串表示为日期、时间或电话号码。

+   插入录制的语音或音频，例如包括标准录制的消息或模拟背景噪音。

要将 SSML 描述提交到语音服务，可以使用 speechSynthesizer.SpeakSsmlAsync（ssml_string） 方法。

### 如何配置合成语音的音频格式？

在合成语音时，您可以使用 SpeechConfig 对象自定义 Azure AI 语音服务返回的音频流的输出格式。Azure AI 语音服务支持多种输出格式，您可以根据音频文件类型、采样率和位深度等需求进行选择。通过使用 SpeechSynthesisOutputFormat 枚举（例如 SpeechSynthesisOutputFormat.Riff24Khz16BitMonoPcm）并在 SpeechConfig 对象上使用 SetSpeechSynthesisOutputFormat 方法来指定所需的输出格式。

### Azure AI 语音服务提供哪些类型的语音，以及如何指定它们？

Azure AI 语音服务提供两种类型的语音，可用于个性化您的语音应用程序：

+   标准语音：从音频示例创建的合成语音。

+   神经语音：使用深度神经网络创建的更自然的声音。

语音通过表示区域设置和人员姓名的名称进行标识，例如 en-GB-George。要在 SpeechConfig 中指定语音合成的语音，您只需将其 SpeechSynthesisVoiceName 属性设置为您要使用的语音，例如 speechConfig.SpeechSynthesisVoiceName = "en-GB-George"；.

### 实验：Recognize and synthesize speech

https://github.com/MicrosoftLearning/mslearn-ai-language/blob/main/Instructions/Labs/07-speech.md

```
 pip install azure-cognitiveservices-speech==1.30.0
  pip install playsound==1.2.2
```

```
from dotenv import load_dotenv
from datetime import datetime
import os

# Import namespaces
import azure.cognitiveservices.speech as speech_sdk
from playsound import playsound


def main():
    try:
        global speech_config

        # Get Configuration Settings
        load_dotenv()
        ai_key = os.getenv(&#x27;AI_SERVICE_KEY&#x27;)
        ai_region = os.getenv(&#x27;SPEECH_REGION&#x27;)

        # Configure speech service
        speech_config = speech_sdk.SpeechConfig(ai_key, ai_region)
        print(&#x27;Ready to use speech service in:&#x27;, speech_config.region)

        # Get spoken input
        command = TranscribeCommand()
        if command.lower() == &#x27;what time is it?&#x27;:
            TellTime()

    except Exception as ex:
        print(ex)


def TranscribeCommand():
    command = &#x27;&#x27;

    # Configure speech recognition
    current_dir = os.getcwd()
    audioFile = current_dir + &#x27;\\time.wav&#x27;
    playsound(audioFile)
    audio_config = speech_sdk.AudioConfig(filename=audioFile)
    speech_recognizer = speech_sdk.SpeechRecognizer(speech_config, audio_config)

    # Process speech input
    speech = speech_recognizer.recognize_once_async().get()
    if speech.reason == speech_sdk.ResultReason.RecognizedSpeech:
        command = speech.text
        print(command)
    else:
        print(speech.reason)
        if speech.reason == speech_sdk.ResultReason.Canceled:
            cancellation = speech.cancellation_details
            print(cancellation.reason)
            print(cancellation.error_details)

    # Return the command
    return command


def TellTime():
    now = datetime.now()
    response_text = &#x27;The time is {}:{:02d}&#x27;.format(now.hour, now.minute)

    # Configure speech synthesis
    speech_config.speech_synthesis_voice_name = &quot;en-GB-RyanNeural&quot;
    speech_synthesizer = speech_sdk.SpeechSynthesizer(speech_config)

    # Synthesize spoken output
    speak = speech_synthesizer.speak_text_async(response_text).get()
    if speak.reason != speech_sdk.ResultReason.SynthesizingAudioCompleted:
        print(speak.reason)

    # Print the response
    print(response_text)


if __name__ == &quot;__main__&quot;:
    main()
```

> **代码功能总览**

> 这段代码实现了一个简单的语音助手程序。它的核心功能是：

1.  **识别一个音频文件中的语音**：程序会读取一个名为 time.wav 的音频文件，并将其中的英文语音转换成文字。

1.  **理解命令**：程序会检查转换后的文字是不是“what time is it？”.

1.  **语音回答**：如果命令正确，程序会获取当前系统时间，并用语音合成技术将当前时间（例如 "The time is 14:30"）播报出来。

> 简单来说，这是一个“语音提问、语音回答”的演示程序，但它的“提问”部分是来自一个固定的音频文件，而不是实时麦克风。

---

> **1. ****main()**** 函数 （主流程控制）**

> 这是程序的入口和总指挥。

+   **加载配置**：

+   load_dotenv()： 从项目根目录下的 .env 文件中加载环境变量。

+   os.getenv(...)： 读取 Azure AI 服务的密钥 （AI_SERVICE_KEY） 和区域 （SPEECH_REGION）。这样做是为了不把敏感信息直接写在代码里。

+   **配置语音服务**：

+   speech_sdk.SpeechConfig(...)： 使用获取到的密钥和区域信息，初始化 Azure 语音服务的配置。这是后续所有语音功能的“通行证”。

+   **执行命令**：

+   TranscribeCommand()： 调用这个函数来识别音频文件中的命令。

+   if command.lower() == 'what time is it?'： 判断识别出的命令（转换成小写后）是否是“what time is it？”.

+   TellTime()： 如果命令匹配，就调用这个函数来播报时间。

> **2. ****TranscribeC**ommand() 函数 （语音转文本）

> 这个函数扮演了程序的“耳朵”角色。

+   **播放音频**：

+   playsound(audioFile)： 首先，它会播放 time.wav 这个文件。这样做的目的是让你能听到程序正在“处理”的语音是什么。

+   **配置音频源**：

+   audio_config = speech_sdk.AudioConfig(filename=audioFile)： 告诉 Azure SDK，语音的来源不是麦克风，而是 time.wav 这个文件。

+   **识别语音**：

+   speech_recognizer = speech_sdk.SpeechRecognizer(...)： 创建一个语音识别器。

+   speech_recognizer.recognize_once_async().get()： 启动一次性的语音识别。程序会把音频文件发送到 Azure 云端进行处理，然后等待返回结果。

+   **处理结果**：

+   if speech.reason == speech_sdk.ResultReason.RecognizedSpeech： 如果成功识别出语音，就将识别到的文本 （speech.text） 打印出来并返回。

+   else： 如果失败（比如音频质量差、网络问题等），则打印出失败的原因。

> **3. ****TellTime()**** 函数 （文本转语音）**

> 这个函数扮演了程序的“嘴巴”角色。

+   **获取当前时间**：

+   now = datetime.now()： 使用 datetime 库获取当前的日期和时间。

+   response_text = 'The time is {}:{:02d}'.format(...)： 将时间格式化成一个友好的字符串，例如 "The time is 10:05"。

+   **配置语音合成**：

+   speech_config.speech_synthesis_voice_name = "en-GB-RyanNeural"： 指定用来朗读的语音角色。这里选择了一个名为 "Ryan" 的英式男性神经语音。

+   speech_synthesizer = speech_sdk.SpeechSynthesizer(...)： 创建一个语音合成器。

+   **合成并播放语音**：

+   speech_synthesizer.speak_text_async(response_text).get()： 将文本字符串发送给 Azure 服务，服务会将其转换成音频数据并直接通过你的扬声器播放出来。

```
Ready to use speech service in: eastus
What time is it?
The time is 11:38
```

# 通过 Azure AI 语音服务翻译语音

https://learn.microsoft.com/zh-cn/training/paths/develop-language-solutions-azure-ai/

### 什么是 Azure AI 语音服务？

Azure AI 语音服务是一种由 Microsoft Azure 提供的强大机器学习和人工智能驱动的服务，旨在为开发人员提供实时的端到端语音翻译功能，以便他们可以将其集成到应用程序或服务中。

### 在使用 Azure AI 语音服务进行语音翻译之前需要准备什么？

在使用 Azure AI 语音服务进行语音翻译之前，您需要在 Azure 订阅中创建一个 Azure AI 语音资源。创建资源后，您需要获取部署该资源的位置（例如 eastus）和分配给资源的密钥之一。这些信息可以在 Azure 门户中资源的“密钥和终结点”页面上找到。

### 如何将语音翻译成文本？

将语音翻译成文本的过程与语音识别类似，但需要额外指定源语言和目标语言信息。这通过以下步骤实现：

1.  使用 SpeechTranslationConfig 对象封装连接到 Azure AI 语音资源所需的信息，包括其位置和密钥。此对象还用于指定语音识别语言（输入语音的语言）和目标翻译语言。

1.  （可选）使用 AudioConfig 定义要转录的音频输入源。默认情况下，这是系统默认麦克风，但您也可以指定音频文件。

1.  使用 SpeechTranslationConfig 和 AudioConfig 创建 TranslationRecognizer 对象，该对象充当 Azure AI 语音翻译 API 的代理客户端。

1.  使用 TranslationRecognizer 对象的方法调用底层 API 函数，例如 RecognizeOnceAsync（） 方法异步翻译单个口述内容。

1.  处理来自 Azure AI 语音的响应。结果是一个 SpeechRecognitionResult 对象，其中包含原始语言的转录（在 Text 属性中）和翻译字典（在 Translation 属性中，使用双字符 ISO 语言代码作为键）。

### 如何将翻译的文本合成为语音（语音转语音翻译）？

您可以将翻译的文本合成为语音，以创建完整的语音转语音翻译解决方案。有两种主要方式实现这一点：基于事件的合成和手动合成。

### 什么是基于事件的合成？

基于事件的合成适用于一对一翻译（从一种源语言翻译成单一目标语言）的情况。您可以使用这种方法将翻译捕获为音频流。具体步骤包括：

1.  在 TranslationConfig 中指定所需的语音。

1.  为 TranslationRecognizer 对象的 合成 事件创建事件处理程序。

1.  在事件处理程序中，使用 Result 参数的 GetAudio（） 方法检索已翻译音频的字节流。实现事件处理程序的特定代码会因所使用的编程语言而异，可以参考语音 SDK 文档中的 C# 和 Python 示例。

### 什么是手动合成？

手动合成是基于事件的合成的替代方法，它不需要实现事件处理程序。它允许您为一个或多个目标语言生成音频翻译。手动合成本质上是两个独立操作的组合：

1.  使用 TranslationRecognizer 将口语输入翻译成一个或多个目标语言的文本转录。

1.  在翻译操作的结果中遍历 翻译 字典，并使用 SpeechSynthesizer 合成每种语言的音频流。

### TranslationRecognizer 对象在语音翻译中扮演什么角色？

TranslationRecognizer 对象是 Azure AI 语音翻译 API 的代理客户端。它负责将口语输入翻译成一个或多个目标语言的文本转录，并且是实现语音转文本和语音转语音翻译解决方案的关键组件。它还能够触发基于事件的合成，以流式传输翻译的音频。

### SpeechTranslationConfig 对象有什么作用？

SpeechTranslationConfig 对象用于封装连接到 Azure AI 语音资源所需的信息，包括其位置和密钥。此外，它还用于指定语音识别语言（即输入语音的原始语言）以及应将语音翻译到的一个或多个目标语言。这是配置语音翻译过程的起点。

### 实验：Translate Speech

https://github.com/MicrosoftLearning/mslearn-ai-language/blob/main/Instructions/Labs/08-translate-speech.md

# 开发支持音频的生成 AI 应用程序

https://learn.microsoft.com/zh-cn/training/modules/develop-generative-ai-audio-apps/

### 如何开发基于音频的聊天客户端应用？

开发基于音频的聊天客户端应用与开发基于文本的聊天应用基本技术相同。您需要连接到部署模型的终结点，并使用该终结点将包含消息的提示提交给模型并处理响应。主要区别在于，基于音频的聊天提示包含多部分用户消息，其中包括文本内容项和音频内容项。

### 基于音频的聊天提示中的音频内容如何表示？

音频内容项可以是网站中音频文件的 URL，也可以是二进制音频数据。如果使用二进制数据提交本地音频文件，audio_url 内容将采用数据 URL 格式的 base64 编码值。例如，JSON 格式可能如下所示：{ "type": "audio_url"， "audio_url": { "url": "data:audio/mp3；base64,<binary_audio_data>" } }.

### 提交基于音频的提示可以使用哪些 API 和 SDK？

根据模型类型及其部署位置，您可以使用 Microsoft Azure AI 模型推理或 OpenAI API 提交基于音频的提示。这些库还提供特定于语言的 SDK，用于抽象基础 REST API。在后续练习中，您可以使用适用于 Azure AI 模型推理 API 的 Python 或 。NET SDK 和 OpenAI API 开发支持音频的聊天应用程序。

### 用于处理包含音频的提示需要部署哪种类型的模型？

若要处理包含音频的提示，您需要部署多模式生成 AI 模型。这意味着该模型不仅支持基于文本的输入，还支持基于音频的输入。

### Azure AI Foundry 中有哪些多模式模型可用？

Azure AI Foundry 中提供的多模式模型包括 Microsoft Phi-4-multimodal-instruct、OpenAI gpt-4o 和 OpenAI gpt-4o-mini 等。您可以查阅 Azure AI Foundry 文档中的模型目录和集合，以了解更多关于 Azure AI Foundry 中可用模型的信息。

### 部署多模式模型后，如何在 Azure AI Foundry 门户中对其进行测试？

部署多模式模型后，您可以在 Azure AI Foundry 门户的聊天场中对其进行测试。某些模型允许您在测试环境中通过上传文件或录制消息来包含音频附件。

### 在聊天场中测试多模式模型时，我可以上传哪些类型的内容？

在聊天场中，您可以上传本地音频文件，并将文本添加到消息中，以从多模式模型获取响应。

### 多模式模型处理的提示的 JSON 结构是怎样的？

包含多部分用户消息的提示的 JSON 表示形式如下所示：

```
{
  &quot;messages&quot;: [
    {
      &quot;role&quot;: &quot;system&quot;,
      &quot;content&quot;: &quot;You are a helpful assistant.&quot;
    },
    {
      &quot;role&quot;: &quot;user&quot;,
      &quot;content&quot;: [
        {
          &quot;type&quot;: &quot;text&quot;,
          &quot;text&quot;: &quot;Transcribe this audio:&quot;
        },
        {
          &quot;type&quot;: &quot;audio_url&quot;,
          &quot;audio_url&quot;: {
            &quot;url&quot;: &quot;https://.....&quot;
          }
        }
      ]
    }
  ]
}
```

### 实验：Develop an audio-enabled chat app

https://github.com/MicrosoftLearning/mslearn-ai-language/blob/main/Instructions/Labs/09-audio-chat.md