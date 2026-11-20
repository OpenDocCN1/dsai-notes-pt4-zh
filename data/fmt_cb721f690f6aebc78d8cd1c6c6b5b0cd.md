
## New Relic 与 Datadog 定价模式差异

+   对于New Relic，主要是定价模式的不同。 New Relic的定价模型过去根据数据量和安装的探针数量来收取费用，现在变成按坐席和数据量。但实际上我们计算过，New Relic并不一定比原来便宜，而且这导致了一个错误的引导。我们公司只需要两个人使用New Relic这个产品，就付两个人的钱。虽然这种方式在流量上定价比较DDOG便宜，但打破了一个规则，即这个平台并非让公司所有人使用，而是变成一个运维工具。New Relic的功能偏向于运维，而非产品研发团队获得。Datadog完全按照数据量，按客户数据规模收费。

+   所以会出现一个现象：当业务较小时，New Relic与比Datadog相比较便宜，情况是公司人少，但是数据量超级大，New Relic按人头收费就花费比较少。但是如果一般公司，人头比较多，但是数据量大，那使用New Relic就非常贵。各家公司都有不同的计费模式，但无论哪种模式，可能对于一类客户来说，都算贵的。

## DDOG未来不会降价，会采用折扣+扩品类

+   Datadog不会对整个市场降价，而是对超大型客户提供特殊折扣。对于中腰部客户来说，这是他们的绝对收入和利润来源，这块价格变化不会很大。

+   头部客户迁移成本非常高，一般很难迁移。因此，Datadog可能会通过打折来留住这些客户。（迁移成本主要是用户习惯的迁移）

+   Datadog正在扩展自己的品类，特别是在安全领域，希望从Splunk那里夺回更多客户。

+   Datadog除了扩展品类之外，还需关注Markplace。Marketplace中已经出现了针对业务的咨询，如何帮助用户将很多业务数据标记在观测性数据上，增加了很多业务方面的维度标签。

# 7 开源与新产品如何与Datadog竞争

## Grafana开源产品与TCO数据整合

+   Grafana具有特殊性，因为它与ELK实际上是相同的。Grafana对大家更多的理解，它实际上是一个BI。

+   Grafana是Kibana 3.0的Fork。Grafana最早的原型是从Elastic里面分叉出来的，大家发现它比Kibana更好用。Grafana早期更多地是一个开源工具，但现阶段开始推出LGTM Stack（Loki-Grafana-Tempo-Mimir），将Log、Trace，Metrics等全部商业化。主要推行Grafana Cloud（SaaS版本）和Enterprise（OP版本）。Grafana最近还收购了做profiling团队叫Pyroscope。它是要产品线上的1:1对标Datadog。

+   Grafana的问题在于原来是基于开源产品，数据整合并没有做好，仍然是一系列独立的系统，比较割裂。虽然大家都带上了Grafana的系统，如Loki、Metrics使用的普罗米修斯等，这些都是收购或合并过来的。可能会陷入一个尴尬的境地，大家可能都会支持Grafana这个产品作为一个BI或insights的工具去使用。但是，是否要用Grafana这一整套的东西，存在疑问。例如，西门子在使用Grafana时，并不是用Grafana的可观测性的东西，西门子是Datadog的大客户。但在工业制造和IT的数据会汇合到Grafana上去展示，西门子也付费给Grafana。

+   同时客户在搭建Grafana时候也要考虑人力成本带来的TCO问题。自建可观测系统至少需要一年，而且需要持续维护、扩容和升级。可观测性平台是地查询频率、高写入频率场景，控制算力也可能带来不稳定。同时可能牺牲前台体验。

+   因此，从目前的整体可观测性角度来看，Grafana目前吆喝大于实质，尤其是其Cloud版本，由于其价格不比Datadog便宜，所以会出现如果要使用Grafana的全家桶，为什么不用Datadog？但如果想省钱，可以考虑将Grafana的全部私有化自建，这对Grafana没有什么好处。

+   但是Grafana的影响力很大，国内有很多可观测性公司都是基于Grafana的。Grafana Cloud的目标市场还是开源Grafana的用户。

## Honeycombo、Chronosphere等创业公司都有自己的竞争方法

+   Honeycombo切入点是从研发测，不考虑运维测。相比Datadog，Honeycombo在云原生和Consumption Model上更加彻底，不按照`Agent`的数量收费，可以解决之前DDOG在`Infra`监控场景会出现浪费的情景。更加从定价模式上与Datadog竞争。

+   Chronosphere主打产品更便宜，其基于开源时序数据库`M3DB`。

+   市面上还有其他的创业公司，例如基于Snowflake的`Observe`。

## 云厂商的产品适合小项目

+   云厂商的监控方案相对简单，适合小项目或应用。

+   但对于部门众多、技术栈多样的公司，需要一个工具来统一管理。云厂商的方案大多面向自身平台，对于有额外`IDC`的公司需要考虑自己的方案。工程水平较高和业务复杂度高的公司对三方可观性工具需求更旺。
