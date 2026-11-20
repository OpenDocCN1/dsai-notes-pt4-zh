
## 参数调优指南

### 精确生成

`temperature=0~0.3 + max_tokens=500`

### 探索性生成

`temperature=0.7 + top_p=0.9`

## 结果验证方法

- 对生成的代码用 `print`/`console.log` 插入验证点
- 要求模型自己写测试用例：“请为这段代码编写3个单元测试”
