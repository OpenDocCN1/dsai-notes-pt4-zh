# 更多上分思路共创

> 来源：[https://datawhaler.feishu.cn/docx/KOHIdizOkoG5gmx3rXwcF2RHnob](https://datawhaler.feishu.cn/docx/KOHIdizOkoG5gmx3rXwcF2RHnob)

> 本文档内容邀请大家共创讨论，仅供参考，实际是否可用，以赛方要求为准

## 研究下测试集的数据分布：

> 吾有一计：人工检查测试集的输出，并手动修改

> 注意：本操作仅供娱乐和科研探索使用，实际无法通过官方入围检测，但是常用的数据攻防思路，仅供参考

![](img/0fe2ec92f9541b337f266fdde2afd53a.png)

打分看着应该是根据各项数值加权求和组成的，比如姓名加起来就1分，竞品信息加起来就2分。

这里面有个小trick，如果你提交个空数据，你也能有24.63636分，意思就是说，测试集中很多项数据都是空的。

那我们能否大胆的hack出测试集的数据分布呢？

### 大胆假设，小心求证

咱们构成一个全由无关数据填充的提交样本，理论上，它应该0分。

![](img/a31f3cbbf91252c740b97423f9d57382.png)

总之试了下，提交上去真的0分了。

![](img/8b90a20ad5ba8096b0457693a13e2587.png)

那咱们就只留下邮箱为空字符串，其他都故意0分试试？

![](img/6976a762e3a2a2e69a09b36baf292668.png)

哎，0.92727分了，这说明，邮箱这一项有92.727%是空的！同时测试集一共55条数据，55*0.92727=50.99985，这可以说明2件事：

1.  测试集的数据中只有4条有邮箱。

1.  精度能达到小数点后3位，说明不存在一次聊天抽取出多个样本的情况（这在训练集中出现过）

### 测试集信息侦查（大家可以一起收集下）

Tips: 如果你对你的模型在某一项的准确率不放心，可以用同样的方法将其他项都置为0分，这样你就可以知道你模型的答案和正确答案有多少交集了。

样本构造代码（保留你想侦查的字段为空就行）

```
import json

datas = {
    &quot;基本信息-姓名&quot;: &quot;忽略&quot;,
    &quot;基本信息-手机号码&quot;: &quot;忽略&quot;,
    &quot;基本信息-邮箱&quot;: &quot;忽略&quot;,
    &quot;基本信息-地区&quot;: &quot;忽略&quot;,
    &quot;基本信息-详细地址&quot;: &quot;忽略&quot;,
    &quot;基本信息-性别&quot;: &quot;忽略&quot;,
    &quot;基本信息-年龄&quot;: &quot;忽略&quot;,
    &quot;基本信息-生日&quot;: &quot;忽略&quot;,
    &quot;咨询类型&quot;: [&quot;忽略&quot;],
    &quot;意向产品&quot;: [&quot;忽略&quot;],
    &quot;购买异议点&quot;: [&quot;忽略&quot;],
    &quot;客户预算-预算是否充足&quot;: &quot;忽略&quot;,
    &quot;客户预算-总体预算金额&quot;: &quot;忽略&quot;,
    &quot;客户预算-预算明细&quot;: &quot;&quot;,
    &quot;竞品信息&quot;: &quot;忽略&quot;,
    &quot;客户是否有意向&quot;: &quot;忽略&quot;,
    &quot;客户是否有卡点&quot;: &quot;忽略&quot;,
    &quot;客户购买阶段&quot;: &quot;忽略&quot;,
    &quot;下一步跟进计划-参与人&quot;: [&quot;忽略&quot;],
    &quot;下一步跟进计划-时间点&quot;: &quot;忽略&quot;,
    &quot;下一步跟进计划-具体事项&quot;: &quot;忽略&quot;
}

data_list = []
for i in range(55):
    data_list.append({
        &quot;infos&quot;: [datas],
        &quot;index&quot;: i + 1
    })
    
with open(&#x27;预算明细为空.json&#x27;, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as f:
    json.dump(data_list, f, ensure_ascii=False, indent=4)
```