
# 中秋贺卡样式与 HTML 规范

## CSS 样式定义

```css
background-color: #E6E4E1;
border-radius: 15px;
box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
display: flex;
flex-direction: column;
padding: 20px;
box-sizing: border-box;
}

.header {
    background-color: #A49E8D;
    padding: 15px;
    text-align: center;
    color: #FFF;
    font-size: 28px;
    border-radius: 10px;
    margin-bottom: 15px;
}

.separator {
    height: 1px;
    background-color: #B0A99F;
    margin: 15px 0;
}

.subtitle {
    color: #7D7468;
    font-size: 14px;
    text-align: center;
}

.blessing {
    color: #8C7B6E;
    font-size: 24px;
    text-align: center;
    margin: 15px 0;
}

.image-container {
    width: 360px;
    height: 360px;
    background-image: url('https://via.placeholder.com/360x360');
    background-size: cover;
    border-radius: 15px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
    margin: 20px auto;
}

.poem {
    font-size: 15px;
    color: #7D7468;
    text-align: center;
    margin: 10px 0;
}

.footer {
    background-color: #A49E8D;
    padding: 15px;
    text-align: center;
    border-radius: 10px;
    color: #FFF;
    font-size: 18px;
    margin-top: 15px;
}

.footer-note {
    font-size: 12px;
    color: #7D7468;
    text-align: center;
    margin-top: 10px;
    opacity: 0.7;
}
```

## HTML 结构

```html
&lt;/style&gt;

&lt;/head&gt;

&lt;body&gt;

&lt;div class=&quot;card&quot;&gt;

    &lt;div class=&quot;header&quot;&gt;中秋贺卡&lt;/div&gt;

    &lt;div class=&quot;subtitle&quot;&gt;Zhōng Qiū&lt;/div&gt;

    &lt;div class=&quot;subtitle&quot;&gt;Mid-Autumn Festival&lt;/div&gt;

    &lt;div class=&quot;separator&quot;&gt;&lt;/div&gt;

    &lt;div class=&quot;blessing&quot;&gt;月圆人圆，喜乐团圆&lt;/div&gt;

    &lt;div class=&quot;separator&quot;&gt;&lt;/div&gt;

    &lt;div class=&quot;image-container&quot;&gt;&lt;/div&gt;

    &lt;div class=&quot;separator&quot;&gt;&lt;/div&gt;

    &lt;div class=&quot;poem&quot;&gt;银轮高挂夜空明，桂子飘香满庭芳。&lt;/div&gt;

    &lt;div class=&quot;poem&quot;&gt;阖家欢聚共良宵，千里共婵娟此时。&lt;/div&gt;

    &lt;div class=&quot;separator&quot;&gt;&lt;/div&gt;

    &lt;div class=&quot;footer&quot;&gt;愿您中秋佳节阖家团圆，幸福安康！&lt;/div&gt;

    &lt;div class=&quot;footer-note&quot;&gt;此卡片由user_id的AI助手生成&lt;/div&gt;

&lt;/div&gt;

&lt;/body&gt;

&lt;/html&gt;
```

## 设计要求与注意事项

1. 分隔线与上下元素垂直间距相同，具有分割美学。

2. 卡片整体底色尽量采用浅色的莫兰迪色，标题和页脚可以用重点色填充强调以保证构图美观。

3. 一定不要调整400*900的卡片尺寸和400*400（含内边距）的图片占位区尺寸。

4. 从用户提供的信息中提取`user_id`的值替换最终代码中的“`user_id`”

5. 最终的Html代码与`&lt;div class=&quot;poem&quot;&gt;`相关的部分永远不要超过两行。

6. 永远记住最终输出结果中不要包含任何格式化标记。
