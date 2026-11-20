
# Dify自定义工具

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/W06odNsLuoWMhQxDaqhcwodSnSf](https://qqs7y1hozd1.feishu.cn/docx/W06odNsLuoWMhQxDaqhcwodSnSf)

## 工具定义

工具可以扩展 LLM的能力，比如联网搜索、科学计算或绘制图片，赋予并增强了 LLM 连接外部世界的能力。Dify 提供了两种工具类型：第一方工具和自定义工具。

你可以直接使用 Dify 生态提供的第一方内置工具，或者轻松导入自定义的 API 工具（目前支持 OpenAPI / Swagger 和 OpenAI Plugin 规范）。

## 工具的作用

工具使用户可以在 Dify 上创建更强大的 AI 应用，如你可以为智能助理型应用（Agent）编排合适的工具，它可以通过任务推理、步骤拆解、调用工具完成复杂任务。

方便将你的应用与其他系统或服务连接，与外部环境交互，如代码执行、对专属信息源的访问等。

## 配置第一方工具

第一方工具就是Dify自己开发的工具，也称Dify内置工具。Dify目前已经支持多个内置工具：

![](img/d3c2669245b741a427f2813f4d607198.png)

当然你也可以为Dify贡献自己[的工具，让自己的工具](https://github.com/langgenius/dify/blob/main/CONTRIBUTING.md)成为Dify的内置工具。关于如何贡献的具体方法请查看Dify开发贡献文档。

### 第一方工具授权

若你需要直接使用 Dify 生态提供的第一方内置工具，你需要在使用前配置相应的凭据。

![](img/2fba84080bbfa90d8b57b301e582fbf4.png)

## 案例：Agent应用中使用维基百科工具

![](img/8ed2f5983c02539792bec0ba2320626b.png)

![](img/23dfff6040b537a7e263048af3eec49d.png)

![](img/ade9b0cfcb38275beb511146313dd1db.png)

输出结果：

```
好的,我会为您搜索巴黎的旅游信息,并根据搜索结果为您制定一份详细的巴黎旅行攻略。让我们开始吧。

根据搜索结果，我为您制作了一份详细的巴黎旅行攻略：

巴黎旅行攻略

概述
巴黎是世界著名的旅游目的地，每年吸引数百万游客。2018年，巴黎接待了近1800万国际过夜游客，主要来自英国、美国、德国、意大利、中国、印度和加拿大。

主要景点
a) 埃菲尔铁塔：每年约590万游客
b) 卢浮宫：每年约690万游客
c) 凯旋门
d) 巴黎圣母院：2017年接待1200万游客（注：2019年火灾后暂时关闭）
e) 蒙马特圣心教堂：每年约1000万游客
f) 凡尔赛宫：每年约770万游客
g) 蓬皮杜中心：每年约333万游客
h) 奥赛博物馆：每年约300万游客
i) 迪士尼乐园：每年约1100万游客

最佳旅游季节
春季（3月至5月）和秋季（9月至11月）是游览巴黎的最佳时间，气候宜人，游客相对较少。

交通
a) 地铁：巴黎地铁系统发达，是游览城市的最佳方式
b) 公交车：覆盖面广，可欣赏沿途景色
c) 出租车和优步：方便但相对昂贵
d) 步行：巴黎市中心适合步行，可以近距离感受城市魅力

美食
a) 法式面包和糕点
b) 法式奶酪
c) 红酒
d) 法式大餐：尝试米其林星级餐厅
e) 咖啡馆文化：享受露天咖啡馆的悠闲时光

购物
a) 香榭丽舍大街：高端品牌
b) 老佛爷百货和春天百货：综合购物体验
c) 马莱区：独立设计师品牌和复古商店
d) 圣日耳曼德佩区：艺术和古董店

文化体验
a) 参观博物馆：建议购买博物馆通票
b) 欣赏歌剧或芭蕾舞表演
c) 塞纳河游船：欣赏巴黎夜景
d) 参加烹饪课程，学习法式烹饪

注意事项
a) 警惕扒手，特别是在热门旅游区和公共交通工具上
b) 学习几句基本的法语，如问候语和感谢词
c) 提前预订热门景点的门票，避免长时间排队
d) 注意"巴黎综合症"：部分游客可能因文化冲击而产生不适应症状，保持开放和包容的心态

住宿
根据预算和喜好选择酒店、民宿或公寓。建议住在市中心区域，如马莱区、拉丁区或圣日耳曼德佩区，方便游览主要景点。

周边游
如果时间允许，可以考虑参观巴黎周边景点，如:
a) 吉维尼：莫奈花园
b) 枫丹白露宫
c) 沙特尔主教座堂

记住，巴黎是一个充满艺术、历史和浪漫的城市。放慢脚步，细细品味每一个角落，感受这座城市独特的魅力。祝您在巴黎度过一个难忘的假期！
```

## 自定义工具

如何创建自定义工具是本文的重点。

你可以在“工具-自定义工具”内导入自定义的 API 工具，目前支持 OpenAPI / Swagger 和 ChatGPT Plugin 规范。

### 什么是OpenAPI / Swagger规范？

OpenAPI规范文档: 这些文档是使用OpenAPI规范编写的API描述文档。它们通常以YAML或JSON格式编写，包括API的端点（Endpoints）、HTTP方法（GET、POST等）、请求和响应体结构、认证要求等详细信息。

简而言之，OpenAPI / Swagger规范文档就是API接口的描述文档

例如我们自己服务端编写了一个API接口，我们便可以生成这个API接口的OpenAPI规范文档。然后将这个OpenAPI规范文档，导入到Dify的自定义工具中，它就成了一个自定义的工具，我们在Agent或工作流中就可以使用。

当然如果你在互联网上发现了一个好用的API接口，比如查询天气，你也可以将编写它的OpenAPI规范文档，同样也导入到Dify的自定义工具中。

### 什么是ChatGPT Plugin规范？

如果你在开发一个ChatGPT插件，它需要一个类似于OpenAPI的规范来定义插件的功能、输入、输出和其他相关信息。这种规范文档帮助开发人员理解如何集成和使用你的插件。

### 如何创建自定义工具

#### 编写SpringBoot API接口

本例中，我们使用SpringBoot来编写一个简单的API接口，模拟查询天气的功能。当然你在创建自定义工具的时候，可以自行选择其他服务端语言和框架。

这个接口方法非常简单，接收一个城市名称作为参数，然后根据这个城市名称查询天气信息，查询天气信息我们直接返回一个固定的JSON字符串。因为如何调用第三方天气查询的接口不是本文讲解的重点。

```
@RestController
@RequestMapping("/weather")
@Tag(name = "天气接口", description = "查询天气的接口")

public class WeatherController {
    @GetMapping("/query")
    @Operation(summary = "查询天气")
    public ResponseEntity<Object> getWeather(@Parameter(name = "city", description = "查询天气传入的城市名", schema = @Schema(type = "string"))
                                                             @RequestParam(required = false) String city) {
        Map<String, Object> response = new HashMap<>();

        if (city == null || city.isEmpty()) {
            response.put("status", HttpStatus.BAD_REQUEST.value());
            response.put("message", "参数错误");
            return new ResponseEntity<>(response, HttpStatus.BAD_REQUEST);
        }

        // Assuming the temperature is always 20 degrees Celsius for demonstration
        response.put("status", HttpStatus.OK.value());
        response.put("message", "20摄氏度");

        return new ResponseEntity<>(response, HttpStatus.OK);
    }

}
```

#### SpringBoot集成Swagger

其实，上一步中API接口编写完成后，我们就可以根据接口的信息去编写OpenAPI / Swagger规范文档了。

如何编写OpenAPI / Swagger规范文档，三种方法：

1. Swagger Editor官网在线编辑。
2. 根据API接口信息，让ChatGpt、文心一言等帮你生成。
3. SpringBoot集成Swagger依赖，自动生成OpenAPI / Swagger规范文档。

前两种方式比较简单，本节我们介绍第三种方式，SpringBoot集成Swagger依赖，自动生成OpenAPI / Swagger规范文档。

#### 添加Swagger依赖

```
pom.xml

<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.4.0</version>
</dependency>
```

#### 配置Swagger

```
@Configuration
public class SwaggerConfig {

    @Bean
    public OpenAPI springShopOpenAPI() {

        return new OpenAPI()
                .info(new Info().title("SpringBoot3 API")
                        .description("SpringBoot3 sample application")
                        .version("v1.0.0")
                        .license(new License().name("Apache 2.0").url("http://springdoc.org")))
                .externalDocs(new ExternalDocumentation()
                        .description("SpringShop Wiki Documentation")
                        .url("https://springshop.wiki.github.org/docs"));

    }
}
```

#### 使用Swagger

```
@RestController
@RequestMapping("/weather")
@Tag(name = "天气接口", description = "查询天气的接口")

public class WeatherController {
    @GetMapping("/query")
    @Operation(summary = "查询天气")
    public ResponseEntity<Object> getWeather(@Parameter(name = "city", description = "查询天气传入的城市名", schema = @Schema(type = "string"))
                                                             @RequestParam(required = false) String city) {
        Map<String, Object> response = new HashMap<>();

        if (city == null || city.isEmpty()) {
            response.put("status", HttpStatus.BAD_REQUEST.value());
            response.put("message", "参数错误");
            return new ResponseEntity<>(response, HttpStatus.BAD_REQUEST);
        }

        // Assuming the temperature is always 20 degrees Celsius for demonstration
        response.put("status", HttpStatus.OK.value());
        response.put("message", "20摄氏度");

        return new ResponseEntity<>(response, HttpStatus.OK);
    }

}
```

在线预览OpenAPI / Swagger规范文档 启动上面的`[SpringBoot程序，在浏览器](http://localhost:8088/swagger-ui.html)`中访问：`http://localhost:8088/swagger-ui.html` （注意端口号需要根据实际配置，我的SpringBoot程序端口号是8088）。

![](img/c16ac6da001d56bccb7ce23b9cf3c9cc.png)

以下就是一份标准的OpenAPI / Swagger规范文档，你可以直接复制粘贴到Dify自定义工具中。
