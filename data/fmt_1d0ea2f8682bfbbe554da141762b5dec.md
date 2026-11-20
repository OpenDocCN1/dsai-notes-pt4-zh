
# 「豆包Marscode体验官」用豆包开发Vue实用评分组件，简直不要太高效

> 来源：[https://agijuejin.feishu.cn/docx/JTQYdlxkqo1kJUx0LSzcf75QnMg](https://agijuejin.feishu.cn/docx/JTQYdlxkqo1kJUx0LSzcf75QnMg)

> 作者：拉布拉多拉的多

> 链接：https://juejin.cn/post/7394784501933719579

> 来源：稀土掘金

> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 豆包简介

豆包MarsCode是字节用豆包大模型打造的一款智能、便捷的开发工具，可以不用下载，点击链接就可以直接去注册使用。它拥有智能识别当前编码页上下文信息的功能，还同时拥有代码理解、生成、优化等多个功能，在任何代码片段都可以使用这些功能，然后可以对某一段代码内进行优化处理，无缝衔接各个流程，大大提高开发效率。

---

下面是我用豆包MarsCode帮我写的一个评分组件，让我们一起走进MarsCode，走进智能编程吧。

看到这个首页的第一眼就感觉它很牛x，有没有。

![](img/daf420a81d2510f4da9cece044e6cf02.png)

+ 首先注册好MarsCode并登入，进来之后可以看到如下页面，我们创建一个Vue项目

    ![](img/6718d442f6fcdf16bc7ee728662f87fb.png)

+ 这个工作台提供了很多个项目模板，也有你的项目记录。

    ![](img/f874be33e328a44804de11b7240e57f7.png)

+ 还有一个好处就是可以拉取你的`git`仓库代码，或者选择项目模板快速创建项目：

    ![](img/278bfb7428f4a106aa8657528eccfc84.png)

+ 然后我们去选择模板快速创建一个vue项目：

在项目内，自己删除一些文件，然后在`components`文件下创建一个实现评分组件。使用AI Assistant 帮你生成一段代码。

```
&lt;template&gt;
  &lt;div class=&quot;star-rating&quot;&gt;
    &lt;span class=&quot;star&quot; :class=&quot;{ active: index &lt; rating }&quot; @click=&quot;rate(index + 1)&quot; v-for=&quot;(star, index) in 5&quot; :key=&quot;index&quot;&gt;★&lt;/span&gt;
  &lt;/div&gt;
&lt;/template&gt;

&lt;script setup&gt;
import { ref } from &#x27;vue&#x27;;

const rating = ref&lt;number&gt;(3); // 初始化评分为 3 星

function rate(newRating: number) {
  rating.value = newRating;
}
&lt;/script&gt;

&lt;style scoped&gt;
.star-rating {
  display: inline-block;
  font-family: &#x27;Times New Roman&#x27;, Times, serif;
}

.star {
  display: inline-block;
  cursor: pointer;
  color: rgb(202, 201, 201)
}

.star.active {
  color: gold;
}
&lt;/style&gt;
```

+ 当不理解代码片段时，我们让AI Assistant解释选中的代码片段，然后它会在右侧给出这段代码的解释。

+ 对于一些细致代码却忘记写注释的地方，我们使用AI一样的可以帮你把遗漏的代码注释

+ 注释不会被编译器或解释器执行，它们的存在主要是为了让其他开发者（或未来的你）能够更容易地理解代码的功能、目的和工作原理。

![](img/31763ca2ab639f8fbeb149be0897a3a4.png)

+ 运行效果

    ![](img/331c46b9869f189463f3881a1cf094ac.png)

## 优化生成的组件

AI生成代码确实高效，当然现在还不能完全使用它的全部代码，我们需要稍加修改，目前AI生成的代码大概可以帮程序员完成70%的需求了。

> 我们知道，封装一个组件，目的就是给你的团队用的，组件就是提高某个功能的复用性，提高开发效率，但通常来说，组件只接收父组件的数据并渲染在页面上，其本身很少会使用响应式数据的。

现在我们使用的场景是，评分组件接受父组件传入的顾客评分数据，我们的评分组件只负责渲染评分数据，或者对数据进行一些修改操作。

注意：在Vue内，作者建议（不代表强制）我们不要直接在子组件修改父组件的数据，我们需要借助一些手段，去告知父组件，我修改了你的数据，这样才不影响性能。

所以，我们接下来要做的就是让该组件接收父组件数据，并在修改后用发布订阅模式去通知父组件数据被修改了。

除了直接让AI Assistant 生成代码模板外，我们在写代码时，它也会根据当前代码的上下文信息去预测我接下来想写的代码，使得开发效率又进一步提高了。

![](img/9e4d6cab036d236e74b8df7a185a576f.png)

### 优化一下 `star` 组件内的代码

```
&lt;script setup&gt;
    // 从 Vue 库中导入 ref 函数
import { ref } from &#x27;vue&#x27;;
    // 使用 defineProps 宏定义组件接收的属性
const props = defineProps({
    // rating 属性，类型为数字，默认值为 0
    rating: {
        type: Number,
        default: 0
    }
})
    // 使用 defineEmits 宏定义组件可以触发的事件
const emits = defineEmits([&#x27;update:rating&#x27;]);
    // 定义一个名为 rate 的函数
function rate(newRating) {
    // 通过 emits 触发一个名为 update:rating 的事件，并传递 newRating 参数
    emits(&#x27;update:rating&#x27;, newRating);
}
&lt;/script&gt;
```

### App.vue 也需要修改：

```
&lt;script setup&gt;
  // 导入自定义组件 Star
  import Star from &#x27;./components/star.vue&#x27;
  // 导入 ref 函数，用于创建响应式数据
  import { ref } from &#x27;vue&#x27;
  // 创建 ref 对象 val，用于在组件中保存和传递数据
  const val = ref(3)

  // 定义一个名为 handle 的函数，用于更新 val 的值
  const handle = (newVal)=&gt;{
    val.value = newVal
  }

&lt;/script&gt;

&lt;template&gt;
  &lt;!-- 在模板中使用 Star 组件 --&gt;
  &lt;Star :rating=&quot;val&quot; @update:rating=&quot;handle&quot;/&gt;
&lt;/template&gt;

&lt;style scoped&gt;
&lt;/style&gt;
```

代码涉及到我之前文章内未解释的API `defineEmits`，大家可以自行查阅官方文档学习一下，本文主要体验MarsCode给我们带来的极致体验，高效开发。

![](img/27df1f9ecd1be60226a5998b54c9ebcc.png)

> 最后这个程序运行的结果和之前那个一样，修改后遵循了vue作者的设计理念，组件复用在这一刻能得到体现了。
