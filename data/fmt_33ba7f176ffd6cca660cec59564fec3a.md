
```python
&quot;&quot;&quot;

def read_json(json_file_path):
    &quot;&quot;&quot;读取json文件&quot;&quot;&quot;
    with open(json_file_path, &#x27;r&#x27;) as f:
        data = json.load(f)
    return data

def write_json(json_file_path, data):
    &quot;&quot;&quot;写入json文件&quot;&quot;&quot;
    with open(json_file_path, &#x27;w&#x27;) as f:
        json.dump(data, f, ensure_ascii=False, indent=4)

def get_completions(text):
    messages = [ChatMessage(
        role=&quot;user&quot;,
        content=text
    )]
    spark = ChatSparkLLM(
        spark_api_url=SPARKAI_URL,
        spark_app_id=SPARKAI_APP_ID,
        spark_api_key=SPARKAI_API_KEY,
        spark_api_secret=SPARKAI_API_SECRET,
        spark_llm_domain=SPARKAI_DOMAIN,
        streaming=False,
    )
    handler = ChunkPrintHandler()
    a = spark.generate([messages], callbacks=[handler])
    return a.generations[0][0].text

def convert_all_json_in_text_to_dict(text):
    &quot;&quot;&quot;提取LLM输出文本中的json字符串&quot;&quot;&quot;
    dicts, stack = [], []
    for i in range(len(text)):
        if text[i] == &#x27;{&#x27;:
            stack.append(i)
        elif text[i] == &#x27;}&#x27;:
            begin = stack.pop()
            if not stack:
                dicts.append(json.loads(text[begin:i+1]))
    return dicts

class JsonFormatError(Exception):
    def __init__(self, message):
        self.message = message
        super().__init__(self.message)

def check_and_complete_json_format(data):
    required_keys = {
        &quot;基本信息-姓名&quot;: str,
        &quot;基本信息-手机号码&quot;: str,
        &quot;基本信息-邮箱&quot;: str,
        &quot;基本信息-地区&quot;: str,
        &quot;基本信息-详细地址&quot;: str,
        &quot;基本信息-性别&quot;: str,
        &quot;基本信息-年龄&quot;: str,
        &quot;基本信息-生日&quot;: str,
        &quot;咨询类型&quot;: list,
        &quot;意向产品&quot;: list,
        &quot;购买异议点&quot;: list,
        &quot;客户预算-预算是否充足&quot;: str,
        &quot;客户预算-总体预算金额&quot;: str,
        &quot;客户预算-预算明细&quot;: str,
        &quot;竞品信息&quot;: str,
        &quot;客户是否有意向&quot;: str,
        &quot;客户是否有卡点&quot;: str,
        &quot;客户购买阶段&quot;: str,
        &quot;下一步跟进计划-参与人&quot;: list,
        &quot;下一步跟进计划-时间点&quot;: str,
        &quot;下一步跟进计划-具体事项&quot;: str
    }

    if not isinstance(data, list):
        raise JsonFormatError(&quot;Data is not a list&quot;)

    for item in data:
        if not isinstance(item, dict):
            raise JsonFormatError(&quot;Item is not a dictionary&quot;)
        for key, value_type in required_keys.items():
            if key not in item:
                item[key] = [] if value_type == list else &quot;&quot;
            if not isinstance(item[key], value_type):
                raise JsonFormatError(f&quot;Key &#x27;{key}&#x27; is not of type {value_type.__name__}&quot;)
            if value_type == list and not all(isinstance(i, str) for i in item[key]):
                raise JsonFormatError(f&quot;Key &#x27;{key}&#x27; does not contain all strings in the list&quot;)

if __name__ == &quot;__main__&quot;:
    retry_count = 5 # 重试次数
    result = []
    error_data = []

    # 读取数据
    train_data = read_json(&quot;dataset/train.json&quot;)
    test_data = read_json(&quot;dataset/test_data.json&quot;)

    for index, data in tqdm(enumerate(test_data)):
        index += 1
        is_success = False
        for i in range(retry_count):
            try:
                res = get_completions(PROMPT_EXTRACT.format(content=data[&quot;chat_text&quot;]))
                infos = convert_all_json_in_text_to_dict(res)
                infos = check_and_complete_json_format(infos)
                result.append({
                    &quot;infos&quot;: infos,
                    &quot;index&quot;: index
                })
                is_success = True
                break
            except Exception as e:
                print(&quot;index:&quot;, index, &quot;, error:&quot;, e)
                continue
        if not is_success:
            data[&quot;index&quot;] = index
            error_data.append(data)
    write_json(&quot;output.json&quot;, result)
```

> 本教程主要由 、 写作贡献，经过多位助教内测和建议修改，期待你的点赞和评论～
