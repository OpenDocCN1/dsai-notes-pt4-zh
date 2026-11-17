
# 鲜花店文案生成模板与 JSON Schema

## JSON Schema

```json
{
  "properties": {
    "flower_type": {
      "title": "Flower Type",
      "description": "鲜花的种类",
      "type": "string"
    },
    "price": {
      "title": "Price",
      "description": "鲜花的价格",
      "type": "integer"
    },
    "description": {
      "title": "Description",
      "description": "鲜花的描述文档",
      "type": "string"
    },
    "reason": {
      "title": "Reason",
      "description": "为什么要这样写这个文档",
      "type": "string"
    }
  },
  "required": [
    "flower_type",
    "price",
    "description",
    "reason"
  ]
}
```

## Prompt 模板

### 模板参数

`input_variables=['flower', 'price']`

`` `partial_variables={'format_instructions': 'The output should be formatted as a JSON instance that conforms to the JSON schema below.\n\nAs an example, for the schema {"properties": {"foo": {"title": "Foo", "description": "a list of strings", "type": "array", "items": {"type": "string"}}}, "required": ["foo"]}\nthe object {"foo": ["bar", "baz"]} is a well-formatted instance of the schema. The object {"properties": {"foo": ["bar", "baz"]}} is not well-formatted.\n\nHere is the output schema:\n```\n{"properties": {"flower_type": {"title": "Flower Type", "description": "鲜花的种类", "type": "string"}, "price": {"title": "Price", "description": "鲜花的价格", "type": "integer"}, "description": {"title": "Description", "description": "鲜花的描述文档", "type": "string"}, "reason": {"title": "Reason", "description": "为什么要这样写这个文档", "type": "string"}}, "required": ["flower_type", "price", "description", "reason"]}\n```'}` ``

`template='您是一位专业的鲜花店文案撰写员。\n对于售价为 {price} 元的 {flower} ，您能提供一个吸引人的简短中文描述吗？\n{format_instructions}'`

### 示例：玫瑰（售价 50 元）

提示： 您是一位专业的鲜花店文案撰写员。

对于售价为 50 元的 玫瑰 ，您能提供一个吸引人的简短中文描述吗？

The output should be formatted as a JSON instance that conforms to the JSON schema below.

As an example, for the schema `{"properties": {"foo": {"title": "Foo", "description": "a list of strings", "type": "array", "items": {"type": "string"}}}, "required": ["foo"]}`

the object `{"foo": ["bar", "baz"]}` is a well-formatted instance of the schema. The object `{"properties": {"foo": ["bar", "baz"]}}` is not well-formatted.

Here is the output schema:

```json
{
  "properties": {
    "flower_type": {
      "title": "Flower Type",
      "description": "鲜花的种类",
      "type": "string"
    },
    "price": {
      "title": "Price",
      "description": "鲜花的价格",
      "type": "integer"
    },
    "description": {
      "title": "Description",
      "description": "鲜花的描述文档",
      "type": "string"
    },
    "reason": {
      "title": "Reason",
      "description": "为什么要这样写这个文档",
      "type": "string"
    }
  },
  "required": [
    "flower_type",
    "price",
    "description",
    "reason"
  ]
}
```

### 示例：百合（售价 30 元）

提示： 您是一位专业的鲜花店文案撰写员。

对于售价为 30 元的 百合 ，您能提供一个吸引人的简短中文描述吗？

The output should be formatted as a JSON instance that conforms to the JSON schema below.

As an example, for the schema `{"properties": {"foo": {"title": "Foo", "description": "a list of strings", "type": "array", "items": {"type": "string"}}}, "required": ["foo"]}`

the object `{"foo": ["bar", "baz"]}` is a well-formatted instance of the schema. The object `{"properties": {"foo": ["bar", "baz"]}}` is not well-formatted.

Here is the output schema:

```json
{
  "properties": {
    "flower_type": {
      "title": "Flower Type",
      "description": "鲜花的种类",
      "type": "string"
    },
    "price": {
      "title": "Price",
      "description": "鲜花的价格",
      "type": "integer"
    },
    "description": {
      "title": "Description",
      "description": "鲜花的描述文档",
      "type": "string"
    },
    "reason": {
      "title": "Reason",
      "description": "为什么要这样写这个文档",
      "type": "string"
    }
  },
  "required": [
    "flower_type",
    "price",
    "description",
    "reason"
  ]
}
```

### 示例：康乃馨（售价 20 元）

提示： 您是一位专业的鲜花店文案撰写员。

对于售价为 20 元的 康乃馨 ，您能提供一个吸引人的简短中文描述吗？

The output should be formatted as a JSON instance that conforms to the JSON schema below.

As an example, for the schema `{"properties": {"foo": {"title": "Foo", "description": "a list of strings", "type": "array", "items": {"type": "string"}}}, "required": ["foo"]}`

the object `{"foo": ["bar", "baz"]}` is a well-formatted instance of the schema. The object `{"properties": {"foo": ["bar", "baz"]}}` is not well-formatted.

Here is the output schema:

```json
{
  "properties": {
    "flower_type": {
      "title": "Flower Type",
      "description": "鲜花的种类",
      "type": "string"
    },
    "price": {
      "title": "Price",
      "description": "鲜花的价格",
      "type": "integer"
    },
    "description": {
      "title": "Description",
      "description": "鲜花的描述文档",
      "type": "string"
    },
    "reason": {
      "title": "Reason",
      "description": "为什么要这样写这个文档",
      "type": "string"
    }
  },
  "required": [
    "flower_type",
    "price",
    "description",
    "reason"
  ]
}
```
