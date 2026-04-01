# Trae Windows 初上手，这些问题与解法你必须知道

> 来源：[https://agijuejin.feishu.cn/docx/WMROd86vxoyGrOxN01kcvVGLnug](https://agijuejin.feishu.cn/docx/WMROd86vxoyGrOxN01kcvVGLnug)

> 作者：南湘城南

> 链接：https://juejin.cn/post/7472193292206751782

> 来源：稀土掘金

> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

> 如有侵权，请联系删除。

在如今这个科技日新月异的时代，人工智能对软件开发[方式的变革是有目](https://www.trae.ai/?utm_source=juejin&utm_medium=juejin_trae&utm_campaign=AGI_article)共睹的。字节跳动推出的 Trae.ai ，凭借其独特的设计理念与强大功能，在编程领域中占据了一席之地，成为了不少程序员提升效率的得力助手 。它的 Builder 模式、Chat 模式和多模态开发等功能，配合双核驱动、原生中文支持等技术优势，让编程变得更加智能与便捷。

而当 [Trae.ai ](https://www.trae.ai/?utm_source=juejin&utm_medium=juejin_trae&utm_campaign=AGI_article)延伸到 Windows 系统，推出 Trae Windows 版本时，无疑为广大 Windows 用户打开了一扇新的大门。我也在第一时间进行了安装体验，在使用过程中，既有新奇的感受，也碰到了一些挑战。接下来，就详细和大家分享我使用 Trae Windows 的经历。

## 一、Trae Windows 初体验

当字节跳动宣布 Trae 支持 Windows 系统时，我第一时间就进行了安装体验。作为一款集成强大 AI 辅助功能的 IDE 工具，Trae 给我的第一印象就是界面简洁友好，操作流程也十分直观，对新手非常友好。简体中文和英文两种版本的支持，消除了语言障碍，让全球开发者都能轻松上手。

在使用过程中，其 AI 问答功能让我眼前一亮。以往遇到编程问题，需要在搜索引擎中反复查找资料，现在只需在 Trae 中用自然语言提问，就能快速得到代码示例和解决方案，大大提高了开发效率。代码自动补全和基于 Agent 的 AI 编程功能也十分实用，能够根据上下文智能推荐代码，减少了手动输入的工作量和错误率 。

对于有一定编程基础，但在某些复杂框架和样式上存在不足的开发者来说，Trae 简直就是福音。例如在前端开发中，只需给出交互设计的大致描述，Trae 就能帮助完成对应界面的开发，这是以往开发工具难以做到的。

## 二、使用中遇到的问题

### （一）与现有开发流程融合困难

目前各种前后端应用的开发生态已经非常成熟，开发流程相对封闭。Trae 虽然强大，但很难完全融入原有的开发流程。以开发 iOS 应用为例，Trae 可以创建项目和编写代码，但实际的运行和调试还是需要在 Xcode 中完成。在 Trae 和 Xcode 之间来回切换，严重影响了开发效率和连贯性。

### （二）对复杂项目支持不足

在尝试开发一些大型复杂项目时，Trae 的局限性就显现出来了。虽然它能快速生成代码框架，但在处理项目中涉及的复杂业务逻辑、系统架构以及与其他第三方工具的集成时，还存在一定的困难。例如在一个涉及多个微服务架构的项目中，Trae 生成的代码在与现有的服务注册与发现组件集成时，出现了兼容性问题。

### （三）依赖安装和配置问题

新手在安装 Trae 时，可能会遇到依赖安装失败的情况，尤其是在网络环境不佳时。另外，配置文件如果出现错误，也会导致项目无法正常运行。例如在配置 tsconfig.json 文件时，由于对某些配置项不熟悉，导致项目编译出错。

## 三、解决方案探索

### （一）流程融合解决方案

假设你正在开发一个 iOS 应用，在 Trae 中完成代码编写后，需要在 Xcode 中运行调试。为了减少切换带来的效率损耗，你可以利用脚本工具来简化流程。比如使用 AppleScript（Mac 系统）或 PowerShell（Windows 系统）编写脚本，实现一键从 Trae 导出代码到 Xcode 指定目录，并自动打开 Xcode 进行编译运行。

1.  AppleScript 示例（Mac 系统）

```
-- 假设Trae项目目录为 /Users/yourusername/TraeProjects/iOSApp
-- Xcode项目目录为 /Users/yourusername/XcodeProjects/iOSApp
set traeProjectPath to &quot;/Users/yourusername/TraeProjects/iOSApp&quot;
set xcodeProjectPath to &quot;/Users/yourusername/XcodeProjects/iOSApp&quot;
-- 复制Trae项目中的代码文件到Xcode项目目录
tell application &quot;Finder&quot;
    duplicate files of folder traeProjectPath to folder xcodeProjectPath with replacing
end tell
-- 打开Xcode并编译运行项目
tell application &quot;Xcode&quot;
    open xcodeProjectPath
    activate
    tell window 1
        set buildAction to &quot;build_and_run&quot;
        perform action buildAction
    end tell
end tell
```

1.  **PowerShell 示例（Windows** 系统，需安装 Xcode 命令行工具）

```
# 假设Trae项目目录为 C:\Users\yourusername\TraeProjects\iOSApp
# Xcode项目目录为 C:\Users\yourusername\XcodeProjects\iOSApp
$traeProjectPath = &quot;C:\Users\yourusername\TraeProjects\iOSApp&quot;
$xcodeProjectPath = &quot;C:\Users\yourusername\XcodeProjects\iOSApp&quot;
# 复制Trae项目中的代码文件到Xcode项目目录
Copy-Item -Path $traeProjectPath* -Destination $xcodeProjectPath -Recurse -Force
# 打开Xcode并编译运行项目（需配置Xcode命令行工具路径）
&amp; &quot;C:\Program Files (x86)\Xcode\Xcode.app\Contents\Developer\usr\bin\xcodebuild&quot; -project $xcodeProjectPath -scheme &quot;YourSchemeName&quot; build &amp;&amp; open -a Xcode $xcodeProjectPath
```

### （二）复杂项目支持改进

以一个电商微服务项目为例，Trae 生成了基础代码框架，但在与 Nacos（服务注册与发现组件）集成时出现问题。

1.  **手动调整代码 **：仔细检查 Trae 生成的服务注册代码，发现其与 Nacos 的通信接口参数格式不匹配。手动修改代码中服务注册的方法，使其符合 Nacos 的接口规范。例如，Nacos 要求服务注册时传递的服务名、IP、端口等信息格式为 JSON，而 Trae 生成的代码使用了自定义格式。修改如下：

```
// 原Trae生成代码
ServiceRegistration registration = new ServiceRegistration();
registration.setName(&quot;ecommerce-service&quot;);
registration.setIp(&quot;127.0.0.1&quot;);
registration.setPort(8080);
// 发送注册请求，格式不符合Nacos要求
// 修改后代码
JSONObject registrationJson = new JSONObject();
registrationJson.put(&quot;serviceName&quot;, &quot;ecommerce-service&quot;);
registrationJson.put(&quot;ip&quot;, &quot;127.0.0.1&quot;);
registrationJson.put(&quot;port&quot;, 8080);
RestTemplate restTemplate = new RestTemplate();
restTemplate.postForObject(&quot;http://nacos-server:8848/nacos/v1/ns/instance&quot;, registrationJson, String.class);
```

1.  **优化系统架构 **：在项目架构层面，Trae 生成的代码可能没有充分考虑微服务之间的通信负载均衡和容错机制。引入 Spring Cloud Ribbon 实现客户端负载均衡，以及 Hystrix 实现服务熔断和降级。在 Trae 生成的代码基础上，添加相关依赖和配置：

```
&lt;!-- 添加Spring Cloud Ribbon和Hystrix依赖 --&gt;
&lt;dependency&gt;
    &lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
    &lt;artifactId&gt;spring-cloud-starter-netflix-ribbon&lt;/artifactId&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
    &lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
    &lt;artifactId&gt;spring-cloud-starter-netflix-hystrix&lt;/artifactId&gt;
&lt;/dependency&gt;
```

然后在主应用类上添加注解开启相关功能：

```
import org.springframework.cloud.netflix.hystrix.EnableHystrix;
import org.springframework.cloud.netflix.ribbon.RibbonClient;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
@SpringBootApplication
@EnableHystrix
@RibbonClient(name = &quot;ecommerce-service&quot;)
public class EcommerceApplication {
    public static void main(String[] args) {
        SpringApplication.run(EcommerceApplication.class, args);
    }
}
```

### （三）依赖安装和配置问题解决

在配置tsconfig.json文件时出现编译错误，错误提示为Property 'strictNullChecks' does not exist on type '{}'。

1.  **检查语法错误 **：仔细检查tsconfig.json文件，发现是由于在配置对象中多写了一个逗号导致语法错误。例如原文件内容：

```
{
    &quot;compilerOptions&quot;: {
        &quot;target&quot;: &quot;ES6&quot;,
        &quot;module&quot;: &quot;commonjs&quot;,
        &quot;strictNullChecks&quot;: true,
    }
}
```

正确的应该是去掉最后一个逗号：

```
{
    &quot;compilerOptions&quot;: {
        &quot;target&quot;: &quot;ES6&quot;,
        &quot;module&quot;: &quot;commonjs&quot;,
        &quot;strictNullChecks&quot;: true
    }
}
```

1.  **参考官方文档 **：如果不确定某些配置项的含义和用法，可以参考 TypeScript 官方文档。例如，想要了解lib配置项的作用，可以在官方文档中搜索，找到其详细说明：lib用于指定要包含在编译中的库文件列表，如["dom", "es6"]表示包含 DOM 和 ES6 相关的库。根据项目需求，合理配置该选项，避免因错误配置导致编译问题。

## 四、总结

总体来说，Trae Windows 版的推出为开发者带来了全新的编程体验，其强大的 AI 功能极大地降低了编程门槛，提高了开发效率。虽然在使用过程中遇到了一些问题，但通过具体的解决方法和不断的探索，这些问题都能得到有效的处理。无论是编程新手还是有经验的开发者，都不妨尝试使用 Trae，感受 AI 编程带来的便利和乐趣。同时，也期待 Trae 能够不断进化，为我们带来更多惊喜。