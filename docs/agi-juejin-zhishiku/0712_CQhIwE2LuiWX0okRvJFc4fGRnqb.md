
# 豆包MarsCode 蛇年编程大作战-蛇年贺岁刮刮乐

> 来源：[https://agijuejin.feishu.cn/docx/QDUldXyc2oHqBmxk0zmc9LvZn9c](https://agijuejin.feishu.cn/docx/QDUldXyc2oHqBmxk0zmc9LvZn9c)

> 作者：BLACK595

> 链接：https://juejin.cn/post/7463113713200021556

> 来源：稀土掘金

> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 前言

新年总是充满了惊喜与期待，刮刮乐作为一种广受欢迎的互动形式，能为人们带来即时的欢乐和惊喜。借助蛇年这个特殊的时间节点，我希望通过MarsCode实现一个线上的蛇年贺岁刮刮乐，让大家在享受刮奖乐趣的同时，也能感受到蛇年的独特魅力。

## 完整代码及在线体验

<https://code.juejin.cn/pen/7463028184590909474>

![](img/d4796673abe0a140bff61d234135cef1.png)

## 素材准备

使用蛇年贺卡作为我们刮刮卡的背景图，将背景图上传到云，获取图片链接，之后我们就可以通过更换链接来随机获取一张背景图了。

![](img/b12684f1d572a0d542ab7b19e423d735.png)

## 刮擦效果

刮刮乐最核心的部分便是刮擦效果，像这种功能相对单一，代码又比较复杂的我们可以先交给MarsCode来处理，再在他的基础上进行整合。首先 Ctrl + U 打开MarsCode的对话界面，接着询问我们需要的功能：“帮我使用`Vue`和`Canvas`实现刮擦效果”

![](img/100eb86e87acd108f1771a87122640f9.png)

可以看到MarsCode帮助我们生成了鼠标移动和按下事件的监听，以及使用了刮擦效果的关键属性 `this.ctx.globalCompositeOperation = 'destination-out';`

`destination-out` 的作用简单讲就是，当在画布上铺满了颜料后，之后的绘制便是在画布上进行擦除。 接着，我们直接将代码插入到文件中。

![](img/844656f4220836cb0f9d03a6742a062c.png)

修改一下背景为我们自己的背景图，将鼠标滑动时的图形设置为圆形，`ctx.arc(event.offsetX, event.offsetY, 36, 0, 2 * Math.PI);`效果就是这样

![](img/b2f6b8a9c29cb58ad95610c871af1549.png)

## 面值和中奖金额

刮擦刮刮卡时会有这张卡的面值，而且只需要刮擦刮刮卡的部分，刮开后显示中奖金额，因此我们调整灰色部分位置，并添加相应中奖金额和刮刮卡的面值在灰色刮卡区域的下层和上层。

不过嘛😕，这样之后，发现刮掉之后并没有显示下层的中奖金额

![](img/cde7c43edbde76781e7ca260419b83af.png)

自我感觉代码没什么问题，再次使用MarsCode对话试试。

![](img/38e1e13bdbf509253431bce8d5ac2e88.png)

原来是因为中奖金额和灰色背景的像素重叠，而使用了`destination-out`属性后，擦除灰色背景时就一并把中奖金额也一起擦除了。听从豆包的建议，我们将图像分层，使用两个`canvas`，`frontCanvas`和`backCanvas`分别表示灰色前景和中奖金额背景，这样擦除的时候也就不会一起擦除啦。

```javascript
// 在背景canvas上填充黄色背景
backCtx.fillStyle = '#bd0101';
backCtx.fillRect(0, 400, backCanvas.value.width, 100);

// 在背景canvas上写上中奖金额
backCtx.fillStyle = '#f3f3f3';
backCtx.font = '40px Arial';
backCtx.textAlign = 'center';
backCtx.textBaseline = 'middle';

// 获奖金额
const tempValue = (Math.floor(Math.random() * parValue.value) + 1) * (Math.floor(Math.random() * parValue.value) + 1) * parValue.value;

// 保留两位小数
getValue.value = tempValue.toFixed(2);
backCtx.fillText('￥' + getValue.value, backCanvas.value.width / 2, 450);

// 在frontCanvas上绘制灰色背景
frontCtx.fillStyle = 'gray';
frontCtx.fillRect(0, 0, frontCanvas.value.width, frontCanvas.value.height);

// 在灰色背景上写上文字
frontCtx.fillStyle = 'white';
frontCtx.font = '40px Arial';
frontCtx.textAlign = 'center';
frontCtx.textBaseline = 'middle';
frontCtx.fillText(parValue.value + '元', frontCanvas.value.width / 2, 50);
frontCtx.globalCompositeOperation = 'destination-out';
```

![](img/718c94d408f5693ec9a84dabb6823917.png)

## 面值选择

核心的刮卡功能完成后，想到在外面玩刮刮乐都有面值选择，10元的，20的，50的，咱们也将这三个面值设置成按钮添加到里面去，每次选择面值后，咱们都扣除他的金额并初始化刮刮卡。

```javascript
const parClick = (value) => {
  let tempValue = myValue.value - value;
  if (tempValue < 0) {
    alert("余额不足");
    return;
  }
  myValue.value = tempValue;
  parValue.value = value;
  // 重新初始化
  init();
};
```

结果发现只有第一次能够正常刮卡，后面就直接把金额显示出来了

咦🤔，为啥只有第一次有灰色刮卡背景，再仔细悄悄代码，看不出有什么问题。再问问MarsCode吧， 这时我们的代码也比较多了，如果直接问它，需要描述较为详细的上下文背景，因此我们通过"`#`"选择代码中的`init()`方法指定上下文。

![](img/b3a1fd8da122196add40fa2872b161ad.png)

可以看到，原来是在绘制新的图形时，会将目标区域的颜色清除，从而实现刮开效果。然而，在 `init` 函数中，没有重置 `globalCompositeOperation` 的值，这可能导致在第二次初始化时，`globalCompositeOperation` 仍然保持为 `destination-out`，从而使得灰色前景无法正常显示。 我们只需要再`init`中重置就行

```javascript
const init = () => {
  ...
  // 重置 globalCompositeOperation
  frontCtx.globalCompositeOperation = 'source-over';
  ...
};
```

## 最终效果

最后，添加“提出”按钮，将用户的金额提出，提示不同的吉祥话，清零金额。

![](img/ecca4042fd6e37bb645d5dc08e053d58.png)

### 完整代码及在线体验

快来试试你的运气怎么样，能不能成为百万富翁吧。

<https://code.juejin.cn/pen/7463028184590909474>

![](img/610821326cd44aa29953b8f85df4c37a.png)

## 总结

多次使用MarsCode后，我的一点小小的使用经验送给大家：

+ MarsCode只是我们的辅助工具，我们需要细化问题描述，将功能拆解，一点点的将积木拼搭。

+ 多用“`#`”，确定上下文，明确要询问的是一个文件一个方法片段还是整个工作区文件。

+ 当多次询问无果后，新开一个对话可能会有意外之喜。

最后，祝各位2025蛇年快乐，钱包鼓鼓，笑口常开。
