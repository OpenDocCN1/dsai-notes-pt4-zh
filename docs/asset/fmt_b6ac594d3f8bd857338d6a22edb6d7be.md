
```
import re

# 示例文本
text = second_row_option_content

def get_questions(text):
    # 数据清洗，将所有换行改为两个空格方便统一处理
    text = text.replace('\n', '  ')+'  '
    # print(text)
    # 正则表达式模式
    # 通过匹配以数字开头然后带一个点，为题干
    # 然后抽取选项A  以A开头 后面带一个点 最后以两个空格结尾
    # 为什么是两个空格？部分数据换行时为换行符，我们已经换成了两个空格，有些是以多个空格分割，我们默认为两个空格
    # 接着匹配B C D选项内容
    # 最后有一个
    pattern = re.compile(r'(\d+\..*?)(A\..*?\s{2})([B-D]\..*?\s{2})([B-D]\..*?\s{2})(D\..*?\s{2})', re.DOTALL)

    # 查找所有匹配项
    matches = pattern.findall(text)

    # 存储结果的字典列表
    questions_dict_list = []

    # 打印结果
    for match in matches:
        question, option1, option2, option3, option4 = match
        pattern_question = re.compile(r'(\d+)\.(.*)')
        # 第一个为选择题的题目 提前存到question_text 
        question_text = pattern_question.findall(question.strip())[0][1]
        
        # 提取选项字母和内容
        options = {option1[0]: option1, option2[0]: option2, option3[0]: option3, option4[0]: option4}
        
        question_dict = {
            'question': question_text,
            # 这一步就是防止ACBD这种乱序，我们进行重新匹配，将可能是ACBD的数据以首字母按位置排好号
            'options': {
                'A': options.get('A', '').strip(),
                'B': options.get('B', '').strip(),
                'C': options.get('C', '').strip(),
                'D': options.get('D', '').strip()
            }
        }
        
        questions_dict_list.append(question_dict)
    # 最后获得
    return questions_dict_list

# 调用函数并打印结果
questions = get_questions(text)
for q in questions:
    print(q)
```

### 3.3.3 抽取问题的结果

```
# 首先做数据清洗，将空格、换行符及点都删除
def remove_whitespace_and_newlines(input_string):
    # 使用str.replace()方法删除空格和换行符
    result = input_string.replace(" ", "").replace("\n", "").replace(".", "")
    return result
```

```
import re

# 示例文本
text = """
32. B. The underlying logic of the effect.                                                   33.D. estimates were not fully independent.
34.C. The discussion process.            35.D. Approving.
"""
def get_answers(text):
    text = remove_whitespace_and_newlines(text)
    # 正则表达式模式
    # 这里是一个数字加一个A-D的大写字母表示为答案区域，因为有些答案中有解释，这样的匹配规则可以尽可能匹配到答案
    pattern = re.compile(r'(\d)\s*([A-D])')

    # 查找所有匹配项
    matches = pattern.findall(text)
    res = []
    # 打印结果
    for match in matches:
        number_dot, first_letter = match
        res.append(first_letter)
    return res
```

### 3.3.4 `prompt`设计

```
def get_prompt_en(text):
    prompt = f'''
    你是⼀个⾼考选择题出题专家，你出的题有⼀定深度，你将根据阅读文本，出4道单项选择题，包含题目选项，以及对应的答案，注意：不⽤给出原文，每道题由1个问题和4个选项组成，仅存在1个正确答案，请严格按照要求执行。
The reading text is mainly in English. The questions and answers you raised need to be completed in English for at least the following points:
    
    ### 回答要求
    (1)Understanding the main idea of the main idea.
    (2)Understand the specific information in the text.
    (3)infering the meaning of words and phrases from the context
    
    
    ### 阅读文本
    {text}
    '''
    
    return prompt   
```

可以看到代码里面首先对大模型做了声明，声明如下：

你是⼀个⾼考选择题出题专家，你出的题有⼀定深度，你将根据阅读文本，出4道单项选择题，包含题目选项，以及对应的答案，注意：不⽤给出原文，每道题由1个问题和4个选项组成，仅存在1个正确答案，请严格按照要求执行。

The reading text is mainly in English. The questions and answers you raised need to be completed in English for at least the following points:

这里是官方的声明，不建议修改~

接下来是要求部分：（这里建议大家根据出题要求做修改尝试~）

### 回答要求

(1)Understanding the main idea of the main idea.

(2)Understand the specific information in the text.

(3)infering the meaning of words and phrases from the context

最后是阅读材料，这里其实是我们传入的阅读材料参数。

### 阅读文本

`{text}`

### 3.3.5 英文数据处理主函数

这里大家可以参考中文部分对比学习，相比中文简单很多~

```
def process_en(df): 
    res_input = []
    res_output = []
    for id in range(len(df)):
        data_options = df.loc[id, '选项']
        data_answers = df.loc[id,'答案']
        data_prompt = df.loc[id,'阅读文本']
        data_options = get_questions(data_options)
        data_answers = get_answers(data_answers)
        data_prompt = get_prompt_en(data_prompt)
        # print(data_options)
        # print(data_answers)

        if(len(data_answers)==len(data_options)):
            res = ''
            for id,question in enumerate(data_options):
                res += f'''
                {id+1}.{question['question']}
                {question['options']['A']}
                {question['options']['B']}
                {question['options']['C']}
                {question['options']['D']}
                answer:{data_answers[id]}
                '''+'\n'
            res_output.append(res)
            res_input.append(data_prompt)
    return res_input,res_output
    # break
```

# 3.4 数据合并

因为微调需要150条数据，数据处理后得到有效数据为102，从中文抽取30条，英文抽取20条组成152条数据作为微调数据。

```
# 将两个列表转换为DataFrame

df_new = pd.DataFrame({'input': cn_input+cn_input[:30]+en_input+en_input[:20], 'output': cn_output+cn_output[:30]+en_output+en_output[:20]})
```

# 额外补充知识

## `python`知识

如果你对`baseline1`的解读仍然充满困惑，那么推荐你从学习`python`开始，打好基础再来尝试也不迟~

你可以学习《聪明办法学`python`第二版》打扎实深度学习相关的`python`基础知识~

## `Pandas`相关知识

![](img/fdf3861a7e74bbfba1625f219a68ac74.png)

https://github.com/datawhalechina/joyful-pandas

## 大语言模型知识

大模型基础: 一文了解大模型基础知识

https://github.com/datawhalechina/so-large-lm

如果你想从0手写代码，构建大语言模型，本项目很适合你。

https://github.com/datawhalechina/llms-from-scratch-cn

吴恩达系列课程

https://github.com/datawhalechina/llm-cookbook

> [ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/AWAQwWgPkignkUkU43zcbdfQnrh)！

> [ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/AWAQwWgPkignkUkU43zcbdfQnrh)任何想法都可以在学习群里交流或在  进行填写哟~

>

> 本教程主要由   写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

## 另外别忘了写笔记打卡呀！！~

## 及时思考、沉淀和产出能让你的学习事半功倍！

>

> 注：本文采用 署名—非商业性使用—相同方式共享 4.0 协议国际版许可协议进行许可。
