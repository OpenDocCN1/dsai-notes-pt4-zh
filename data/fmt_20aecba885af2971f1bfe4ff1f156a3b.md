
# Python MySQL 优惠数据操作与智能客服集成

## 创建并填充优惠表

```
import pymysql

def create_and_populate_discounts():
    # 连接到 MySQL 服务器（未指定数据库）
    conn_mysql = pymysql.connect(
        host='localhost',         # 这里替换成你自己的 MySQL 主机地址
        user='root',              # 这里替换成你自己的用户名
        password='snowball2019',  # 这里替换成你自己的密码
        charset='utf8mb4'         # 字符集
    )
    cursor_mysql = conn_mysql.cursor()

    # 切换到 sports_db 数据库
    cursor_mysql.execute('USE sports_db;')

    # 创建优惠信息表（如果不存在）
    create_discount_table_query = '''
    CREATE TABLE IF NOT EXISTS discounts (
        product_id VARCHAR(100),  -- 产品名称作为唯一标识
        discount_percentage FLOAT,
        discount_amount FLOAT,
        start_date DATE,
        end_date DATE,
        PRIMARY KEY (product_id)  -- 使用产品名称作为主键
    ) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci
    '''
    cursor_mysql.execute(create_discount_table_query)

    # 插入每个产品的优惠信息
    discounts = [
        ('足球', 10.0, 12.0, '2024-12-19', '2024-12-19'),
        ('羽毛球拍', 15.0, 45.0, '2024-12-19', '2024-12-19'),
        ('篮球', 20.0, 40.0, '2024-12-19', '2024-12-19'),
        ('跑步鞋', 5.0, 25.0, '2024-12-19', '2024-12-19'),
        ('瑜伽垫', 8.0, 12.0, '2024-12-19', '2024-12-19'),
        ('速干运动衫', 12.0, 21.6, '2024-12-19', '2024-12-19'),
        ('电子计步器', 10.0, 25.0, '2024-12-19', '2024-12-19'),
        ('乒乓球拍套装', 7.0, 11.2, '2024-12-19', '2024-12-19'),
        ('健身手套', 10.0, 12.0, '2024-12-19', '2024-12-19'),
        ('膝盖护具', 15.0, 33.0, '2024-12-19', '2024-12-19')
    ]

    insert_discount_query = '''
    INSERT INTO discounts (product_id, discount_percentage, discount_amount, start_date, end_date)
    VALUES (%s, %s, %s, %s, %s)
    '''
    try:
        cursor_mysql.executemany(insert_discount_query, discounts)
        conn_mysql.commit()  # 提交事务
        print("优惠数据插入成功")
    except pymysql.MySQLError as e:
        print(f"插入优惠数据时发生错误: {e}")
        conn_mysql.rollback()  # 回滚事务
    
    # 关闭游标和连接
    cursor_mysql.close()
    conn_mysql.close()

    print("优惠数据成功插入到 sports_db 中!")
```

这段代码的功能是在 `sports_db` 数据库中创建一个 `discounts` 表，专门用来存储每个产品的优惠信息。每条优惠信息包括产品名称、折扣百分比、折扣金额以及优惠的开始和结束日期。

调用函数：

```
# 调用函数
create_and_populate_discounts()
```

预期输出：

```
优惠数据插入成功
优惠数据成功插入到 sports_db 中!
```

![](img/c5e50fe386cf706984274fd61b606990.png)

## 查询产品优惠信息

```
import pymysql

def query_discount_by_product_name(product_name):
    # 连接 MySQL 数据库
    conn_mysql = pymysql.connect(
        host='localhost',         # 这里替换成你自己的 MySQL 主机地址
        user='root',              # 这里替换成你自己的用户名
        password='snowball2019',  # 这里替换成你自己的密码
        database='sports_db',     # 这里替换成你的数据库名称
        charset='utf8mb4'         # 字符集
    )
    cursor_mysql = conn_mysql.cursor()

    # 使用 SQL 查询按产品名称查找优惠信息。'%' 符号允许部分匹配。
    cursor_mysql.execute("SELECT * FROM discounts WHERE product_id LIKE %s", ('%' + product_name + '%',))

    # 获取所有查询到的数据
    rows = cursor_mysql.fetchall()

    # 关闭连接
    conn_mysql.close()

    return rows
```

测试函数功能：

```
product_name = '瑜伽垫'
promotion_details = query_discount_by_product_name(product_name)
print(promotion_details)
```

```
(('瑜伽垫', 8.0, 12.0, datetime.date(2024, 12, 19), datetime.date(2024, 12, 19)),)
```

## 定义函数工具 Schema

定义 `query_discount_by_product_name` 函数的 Json Schema 描述，并添加到 `tools` 列表中。

```
tools = [
    {
        "type": "function",
        "function": {
            "name": "query_by_product_name",
            "description": "Query the database to retrieve a list of products that match or contain the specified product name. This function can be used to assist customers in finding products by name via an online platform or customer support interface.",
            "parameters": {
                "type": "object",
                "properties": {
                    "product_name": {
                        "type": "string",
                        "description": "The name of the product to search for. The search is case-insensitive and allows partial matches."
                    }
                },
                "required": ["product_name"]
            }
        }

    },
    {
        "type": "function",
        "function": {
            "name": "query_discount_by_product_name",
            "description": "Query the database to find specific promotions related to the product names offered.",
            "parameters": {
                "type": "object",
                "properties": {
                    "product_name": {
                        "type": "string",
                        "description": "The name of the product to search for in the promotion document. The function returns the promotional details if found."
                    }
                },
                "required": ["product_name"]
            }
        }
    }
]
```

```
available_functions = {"query_by_product_name": query_by_product_name, "query_discount_by_product_name": query_discount_by_product_name}
```

## 构建智能客服对话循环

```
from openai import OpenAI
client = OpenAI()

messages = [
    {"role": "system", "content": "当别人和你打招呼时，比如 你好、在吗等问候语时，你需要回复：你是电商平台的智能客服，你叫木小羽。可以实时查询后台的数据库来实时回答用户提出的问题"},
]

while True:
    prompt = input('\n提出一个问题： ')
    if prompt.lower() == "退出":
        break  # 如果输入的是“退出”，则结束循环
    
    # 添加用户的提问到消息列表
    messages.append({'role': 'user', 'content': prompt})
    
    # 检查是否需要调用外部函数
    completion = client.chat.completions.create(
        model="gpt-4o", 
        messages=messages,
        tools=tools,  
    )
    
    # 提取回答内容
    response = completion.choices[0].message
    tool_calls = completion.choices[0].message.tool_calls
    
    # 处理外部函数调用
    if tool_calls:
        function_name = tool_calls[0].function.name
        function_args = json.loads(tool_calls[0].function.arguments)
        
        function_response = available_functions[function_name](**function_args)
        
        messages.append(response)  
       
        messages.append(
                {
                    "role": "tool",
                    "name": function_name,
                    "content": str(function_response),
                    "tool_call_id": tool_calls[0].id,
                }
            )  
     
        second_response = client.chat.completions.create(
            model="gpt-4o-mini", 
            messages=messages,
        )  
        # 获取最终结果

        final_response = second_response.choices[0].message.content
        messages.append({'role': 'assistant', 'content': final_response})
        print(final_response)
    else:
        # 打印响应并添加到消息列表
        print(response.content)
        messages.append({'role': 'assistant', 'content': response.content})
```
