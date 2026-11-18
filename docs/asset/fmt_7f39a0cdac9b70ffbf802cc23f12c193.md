
```
from faker import Faker
import random

fk_data = Faker(locale="zh_CN")


def random_session(user_id=fk_data.user_name()) -> Session:
    session = Session(user_id=user_id, session_id=fk_data.uuid4())
    session.metadata["title"] = random.choice(["abc", "def", "ghi", "abcd", "abcdef", "abcgh"])
    session.metadata["meta_string"] = fk_data.name()
    session.metadata["meta_long"] = random.randint(0, 9999999999999)
    session.metadata["meta_double"] = random.uniform(0.0, 1.0)
    session.metadata["meta_boolean"] = random.choice([True, False])
    session.metadata["meta_bytes"] = bytearray(fk_data.name(), encoding="utf8")
    return session


total_count = 100
for i in range(total_count):
    user_id = random.choice(["1", "2"])
    session = random_session(user_id=user_id)
    session.update_time = random.randint(1, 100)
    memory_store.put_session(session)
```

## 展示某一个用户最近的活跃会话列表

```
# 方式1：接口返回 iterator 迭代器
iterator = memory_store.list_recent_sessions(user_id="1")
for session in iterator:
    print(session)
```

## 根据 metadata 进行 filter

```
from tablestore_for_agent_memory.base.filter import Filters

# 使用 单个 Filter 进行过滤
sessions = list(memory_store.list_recent_sessions(user_id="1", metadata_filter=Filters.eq("meta_boolean", True)))
print(len(sessions))
```

```
from tablestore_for_agent_memory.base.filter import Filters

# 使用 多个 Filter 进行过滤
sessions = list(
    memory_store.list_recent_sessions(
        user_id="1",
        metadata_filter=Filters.logical_and([Filters.gt("meta_double", 0.5), Filters.eq("meta_boolean", True)]),
    )
)
print(len(sessions))
```

## 设置查询`Session`的截止时间

```
sessions = list(
    memory_store.list_recent_sessions(
        user_id="1",
        inclusive_end_update_time=50,
        metadata_filter=Filters.eq("meta_boolean", True),
    )
)
print(len(sessions))
print([item.update_time for item in sessions])
```

## Search 搜索 Meta 字段

```
all_sessions = []
# 查询 user_id=1的所有记录，使用 next_token 进行翻页
response = memory_store.search_sessions(metadata_filter=Filters.eq("user_id", "1"), limit=3)
sessions, next_token = (response.hits, response.next_token)
all_sessions.extend(sessions)
print(len(sessions))
print(next_token)
# 连续翻页示例
next_token = None
while True:
    response = memory_store.search_sessions(metadata_filter=Filters.eq("user_id", "1"), limit=3, next_token=next_token)
    sessions, next_token = (response.hits, response.next_token)
    all_sessions.extend(sessions)
    # 当 next_token 为 None 时候，表示翻页结束
    if next_token is None:
        break
print("all_sessions:", len(all_sessions))
```

## Message 聊天消息

一个`Session`有n个聊天消息`Message`，本章节主要介绍`Message`的使用。

### 声明 Message

```
from faker import Faker

fk_data = Faker(locale="zh_CN")

from tablestore_for_agent_memory.base.base_memory_store import Message
import random

# 创建一条message（session_id 和 message_id 联合起来确认唯一一行数据）
message = Message(
    session_id="推荐uuid当做session_id",
    message_id="可以使用uuid或其他业务string当做message_id",
    create_time=random.randint(
        0, 999999999
    ),  # create_time 不可变，用来确认这条消息在一个会话(session)里的位置信息(或者叫排序信息)
)

message.content = fk_data.text(20)
message.metadata["meta_string"] = fk_data.name_male()
message.metadata["meta_long"] = random.randint(0, 999999999)
message.metadata["meta_double"] = random.uniform(1.0, 2.0)
message.metadata["meta_boolean"] = random.choice([True, False])
message.metadata["meta_bytes"] = bytearray(fk_data.city_name(), encoding="utf8")
```

### 写入、查询、更新、删除一条聊天记录

```
# 写入
memory_store.put_message(message)

# 查询 (不指定create_time)
message_read = memory_store.get_message(session_id=message.session_id, message_id=message.message_id)
print(message_read)

# 查询 (指定create_time，性能更好，查单条message场景的QPS较高的用户可以传。因该领域大多数业务查询单条聊天消息的场景或qps很少，所以不带也没关系，大多数用户的场景是list出来一批最近的聊天消息，而不是查询单条)
message_read = memory_store.get_message(
    session_id=message.session_id, message_id=message.message_id, create_time=message.create_time
)
print(message_read)

# 更新
message.content = "update to: 123"
memory_store.update_message(message)
message_read_after_update = memory_store.get_message(
    session_id=message.session_id, message_id=message.message_id, create_time=message.create_time
)
print(message_read_after_update)

# 删除 (不指定create_time)
memory_store.delete_message(session_id=message.session_id, message_id=message.message_id)
# 删除（(指定create_time，性能更好，原因参考上述查询接口描述）
memory_store.delete_message(
    session_id=message.session_id, message_id=message.message_id, create_time=message.create_time
)
# 删除后查询不到
print(memory_store.get_message(session_id=message.session_id, message_id=message.message_id))
```

### 获取某一个session会话的聊天消息

#### 写入样例数据

```
from faker import Faker
import random

fk_data = Faker(locale="zh_CN")


def random_message(session_id=fk_data.user_name()) -> Message:
    message = Message(session_id=session_id, message_id=fk_data.uuid4())
    message.create_time = random.randint(0, 999999999)
    message.content = " ".join(
        random.choices(
            ["abc", "def", "ghi", "abcd", "adef", "abcgh", "apple", "banana", "cherry"], k=random.randint(1, 10)
        )
    )
    message.metadata["meta_string"] = fk_data.name_male()
    message.metadata["meta_long"] = random.randint(0, 999999999)
    message.metadata["meta_double"] = random.uniform(1.0, 2.0)
    message.metadata["meta_boolean"] = random.choice([True, False])
    message.metadata["meta_bytes"] = bytearray(fk_data.city_name(), encoding="utf8")
    return message


total_count = 50
for i in range(total_count):
    session_id = random.choice(["1", "2"])
    message = random_message(session_id=session_id)
    message.create_time = random.randint(1, 100)
    memory_store.put_message(message)
```

#### 获取所有消息

根据业务方的经验，在大模型聊天场景下，越早的消息其实不太需要的，仅需要最近的个别几条即可，因此更推荐下一小章节的“获取最近的部分消息”。

```
# 方式1：获取迭代器
# 获取某一个session_id的全部聊天消息（默认根据 create_time 从最新到旧排序，接口返回 iterator 迭代器）
iterator = memory_store.list_messages(session_id="1")
# 将 iterator 迭代器 转为list使用
messages = list(iterator)
print(len(messages))

# 方式2：token分页返回数据（该token可以传递给前端，方便进行按批次分页查询）
response = memory_store.list_messages_paginated(session_id="1", page_size=5, next_token=None)
print(len(response.hits))
print(response.next_token)
# 翻页查询（使用上一次结果里返回的token，传递给下次使用）
response = memory_store.list_messages_paginated(session_id="1", page_size=5, next_token=response.next_token)
print(len(response.hits))
print(response.next_token)
# 后续循环此方式，直到token为None表示查询结束。
```

### 获取最近的部分消息

#### 获取最近的 3 条消息

```
iterator = memory_store.list_messages(session_id="1", max_count=3)
messages = list(iterator)
print(len(messages))

# 根据 metadata 过滤，返回最近的3条消息
messages = list(
    memory_store.list_messages(session_id="1", metadata_filter=Filters.eq("meta_boolean", True), max_count=3)
)
print(len(messages))
```

#### 按照时间获取
