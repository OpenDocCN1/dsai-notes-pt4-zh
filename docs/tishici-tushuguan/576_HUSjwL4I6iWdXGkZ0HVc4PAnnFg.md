# 信息抽取严格输出（金融行业）

> 来源：[https://vxc3hj17dym.feishu.cn/docx/R0qXdD7xYo3oFVxScCJcs3OXn7e](https://vxc3hj17dym.feishu.cn/docx/R0qXdD7xYo3oFVxScCJcs3OXn7e)

```
信息抽取，遵循以下原则：        
1、严格基于“已知文本”进行具体信息抽取，不允许在答案中添加编造成分
2、没有的相关字段则填充空白
3、以json形式输出
其中涉及文本为金融监管类数据
已知文本：行政处罚决定书文号x银保监罚决字〔xxxx〕xx号被处罚当事人姓名或名称个人姓名xxx、xxx单位名称中国xxxx保险股份有限公司xx分公司法定代表人（主要负责人）姓名xx主要违法违规事实（案由）给予投保人保险合同约定以外利益行政处罚……予以警告并处8万元罚款的行政处罚。作出处罚决定的机关名称xx银保监局作出处罚决定的日期      20xx年x月xx日
附件信息:
标准输出格式：
{
&quot;行政处罚决定书文号&quot;: &quot;&quot;,
&quot;被处罚机构&quot;:&quot;&quot;,
&quot;被处罚机构法定代表人(主要负责人姓名)&quot;:&quot;&quot;,
&quot;被处罚当事人（个人）&quot;: &quot;&quot;,
&quot;主要违法违规事实（案由）&quot;: &quot;&quot;,
&quot;行政处罚依据&quot;: &quot;&quot;,
&quot;行政处罚决定&quot;: &quot;&quot;,
&quot;作出处罚决定的机关名称&quot;: &quot;&quot;,
&quot;作出处罚决定的日期&quot;: &quot;YYYY年MM月DD日&quot;        
}
                  
{
&quot;行政处罚决定书文号&quot;: &quot;xx保监罚决字〔xxxx〕xx号&quot;,
&quot;被处罚机构&quot;:&quot;中国xxxx保险股份有限公司xx分公司&quot;,
&quot;被处罚机构法定代表人(主要负责人姓名)&quot;:&quot;齐x&quot;,
&quot;被处罚当事人（个人）&quot;: &quot;张xx、陈xx&quot;,
&quot;主要违法违规事实（案由）&quot;: &quot;给予投保人保险合同约定以外利益&quot;,
&quot;行政处罚依据&quot;: &quot;《中华人民共和国保险法》第一百六十一条、第一百七十一条&quot;,
&quot;行政处罚决定&quot;: &quot;对中国xxxx保险股份有限公司xx分公司予以xx万元罚款……予以警告并处x万元罚款的行政处罚。&quot;,
&quot;作出处罚决定的机关名称&quot;: &quot;xx银保监局&quot;,
&quot;作出处罚决定的日期&quot;: &quot;20xx年x月xx日&quot;
}
```