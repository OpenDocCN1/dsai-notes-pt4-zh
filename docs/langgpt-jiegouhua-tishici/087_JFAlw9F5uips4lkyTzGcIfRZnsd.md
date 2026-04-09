# Gif 制作工作流

> 来源：[https://langgptai.feishu.cn/docx/GD4HdnAhJozB2Sx6bXpcjrX5nXc](https://langgptai.feishu.cn/docx/GD4HdnAhJozB2Sx6bXpcjrX5nXc)

用于 GPT-4o

> 来自 @大鞠  [GIF flow](https://cvvy65wh9yv.feishu.cn/docx/F5SddNDTto3iQUxZBuJc8DQunFb?from=from_copylink)

```
基于这个IP形象帮我生成一张“xxxx”[风格的表情包
动作：
保持IP基本形象面不变的情况下
**不要丢失logo**
带有文字**xxxxx**
```

```
&quot;Create a detailed pixel art frame animation for a game, where the final image is divided into multiple sub-images, each serving as a continuous animation keyframe. Design the sequence to depict [a wizard casting a spell: begin with intricate hand motions, then show the wizard conjuring a vibrant fireball, and finally capture the moment of casting the fireball.] Ensure the keyframes transition smoothly and continuously, and include as many frames as possible to achieve a high level of fluidity and detail in the animation.&quot;
```

> Replace this part with your character + animation description: [a wizard casting a spell: begin with intricate hand motions, then show the wizard conjuring a vibrant fireball, and finally capture the moment of casting the fireball.]

```
注意观察一下这段提示词“a wizard casting a spell: begin with intricate hand motions, then show the wizard conjuring a vibrant fireball, and finally capture the moment of casting the fireball.”，现在我要描述一个IP角色“如图所示”，在电脑前进行办公，采用示例提示词的格式输出给我
```

```
step1:请创建一个4×4的网格布局，将整个画面均匀切割成16个大小相同的方格。每个方格应当清晰可辨，并保持相同的比例。 
step2:为每个子区域分配唯一标识符，从左上角开始按行优先顺序编号为1到16。 
step3:根据1-16的顺序帮我制作成gif
```