
# Dify工作流入门

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/IN0bdEp61oQmZxx22qScFS9snhg](https://qqs7y1hozd1.feishu.cn/docx/IN0bdEp61oQmZxx22qScFS9snhg)

我们之前讲到的聊天助手应用、文本生成应用、Agent应用都是依赖提示词Prompt技术和LLM大语言模型推理能力。

试想我们需要对一个数字数组进行自定义的复杂运算或者调用我们自己服务的Api接口时，仅仅依靠LLM大语言模型的推理能力是不够的。

工作流通过将复杂的任务分解成较小的步骤（节点）降低系统复杂度，减少了对提示词技术和模型推理能力的依赖，提高了 LLM 应用面向复杂任务的性能，提升了系统的可解释性、稳定性和容错性。

> Dify工作流分为两种类型：

+ `Chatflow`：面向对话类情景，包括客户服务、语义搜索、以及其他需要在构建响应时进行多步逻辑的对话式应用程序。

+ `Workflow`：面向自动化和批处理情景，适合高质量翻译、数据分析、内容生成、电子邮件自动化等应用程序。

多说无益，我们直接创建一个客户评价处理的工作流。学习完这个工作流的案例，你就入门Dify工作流了。

## 需求

一个商城网站，用户在PC端对商品进行评价，传统的运营模式是运营人员登录管理后台系统查看用户对商品的评价。我们现在学习了AI技术后，对评价进行智能分类，分为正面评价和负面评价，正面评价发送至公司品牌宣传部邮箱。对负面评价再进行二次分类，分为商品问题、运输问题和其他问题，商品问题发送至售后邮箱，运输问题发送至公司运输部邮箱，其他问题发送至默认的客户支持邮箱。是不是很酷？！😎

本应用属于Dify工作流的`Workflow`类型。

## 创建Workflow

![](img/3930b5f89e20b6a4f6cba851dfbcafc0.png)

## 重要节点

节点是工作流中的关键构成，通过连接不同功能的节点，执行工作流的一系列操作。

Dify中目前支持14种节点，我们不打算流水账式一个个去介绍他们，因为对于初学者来说，这种介绍方式只会让初学者一知半解，看得云里雾里，我们直接在不断的实战案例中讲解每一个节点！

本小节的客户评价处理工作流包括开始、结束、问题分类器、`HTTP请求`、变量聚合器节点。

### 开始

每一个工作流都需要一个开始节点，你可以在开始节点中自定义启动工作流的输入变量。

客户评价处理的工作流的开始节点定义了`productId`（商品id）、`review`（客户评价）和默认的`sys.user_id`（客户id）输入变量。

![](img/b62c9a6fdd6df37038bd4a91f0d1d65c.png)

### 问题分类

通过定义分类描述，问题分类器能够根据用户输入推理与之相匹配的分类并输出分类结果。问题分类器其实就是依赖LLM大语言自身的能力对用户输入进行分类。

客户评价处理工作流中问题分类节点的作用是对用户输入的商品评价文本进行正面评价和负面评价分类，其中负面评价再分为售后问题、运输问题和其他问题。

输入变量是用户输入的客户评价文本，输出变量是设置的分类名称。

别忘了在问题分类节点设置大模型。

![](img/48b09995e6f3ce67f6534e630b3f1502.png)

![](img/90bddbec9461ff661b104b0299f294c0.png)

### HTTP请求

`HTTP请求`节点能够向指定的网络地址发送定制化的HTTP请求，实现与各种外部服务的互联互通。 客户评价处理的工作流中的`HTTP请求`节点将客户评价文本根据问题分类节点的分类发送给各部门的邮箱。

`HTTP请求`节点可以支持`GET`、`POST`、`HEAD`、`PATCH`、`PUT`、`DELETE`请求方法。

这里设置了`productId`（商品id）、`review`（客户评价）和默认的`sys.user_id`（客户id）、`type`（问题类型）。

> `type`

+ `type = 1` 正面评价

+ `type = 2` 售后问题

+ `type = 3` 运输问题

+ `type = 其他` 其他问题

4种问题对应4个`HTTP请求`节点中的`type`变量。

![](img/848c6b1c4c153e56b37efb46640ebeea.png)

`Java SpringBoot`编写客户评价处理接口。

`http://localhost:8089/customer/handle?userId=1&productId=100&review=非常好，下次还买&type=1`

接口方法中直接返回`message`模拟发送邮件。

```
```java
// CustomerServiceController.java
@RestController
@RequestMapping("/customer")
public class CustomerServiceController {

    @GetMapping("/handle")
    public String handle(
            @RequestParam String userId,
            @RequestParam String productId,
            @RequestParam String type,
            @RequestParam String review){
        String message;
        switch (type) {
            case "1":
                //发送至品牌宣传部邮箱
                message = "已发送至品牌宣传部邮箱";
                break;
            case "2":
                //发送至售后邮箱
                message = "已发送至售后邮箱";
                break;
            case "3":
                //发送至运输部邮箱
                message = "已发送至运输部邮箱";
                break;
            default:
                //发送至默认客户支持邮箱
                message = "已发送至默认客户支持邮箱";
                break;
        }

        return message;
    }
}
```
```

### 变量聚合

将多路分支的变量聚合为一个变量，以实现下游节点统一配置。

客户评价处理的工作流中4个`HTTP请求`节点输出的变量（即`Java SpringBoot`接口返回值）聚合成一个变量在结束节点中输出。

![](img/4f787f14b8ba8cc36bdd837fd397c652.png)

### 结束

定义一个工作流程结束的最终输出内容。每一个工作流在完整执行后都需要至少一个结束节点，用于输出完整执行的最终结果。

结束节点为流程终止节点，后面无法再添加其他节点，工作流应用中只有运行到结束节点才会输出执行结果。若流程中出现条件分叉，则需要定义多个结束节点。

结束节点直接输出变量聚合节点输出的变量。

![](img/66c82e720354d884ff6ab424beb807f8.png)

### 连接各个节点

连接工作流中的每一个节点，上一个节点的输出变量作为下一个节点的输入变量。

## 导出DSL

客户评价处理工作流应用可以导出为`DSL`，方便你在Dify[工作区中导入查看整个应用的](https://personel-zhouxinle888-a66353926f9185cff28f2bd374a5c3a9dd89d5206.gitlab.io/dify/%E5%AE%A2%E6%88%B7%E8%AF%84%E4%BB%B7%E5%A4%84%E7%90%86%E5%B7%A5%E4%BD%9C%E6%B5%81.yml)编排。点击`客户评价处理工作流.yml`下载。

创建应用时，选择导入`DSL`文件即可。
