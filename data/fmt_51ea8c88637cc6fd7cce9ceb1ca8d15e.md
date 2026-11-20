
```
onclone: function(clonedDoc) {
  const clonedCard = clonedDoc.getElementById('card-container');
  // 确保布局稳定性
  clonedCard.style.position = 'relative';
  clonedCard.style.width = '1080px';
  clonedCard.style.height = '800px';
  // 处理所有定位元素，确保正确的堆叠顺序
  const positionedElements = clonedCard.querySelectorAll('[style*="position"]');
  positionedElements.forEach((el, index) => {
    // 确保有明确的z-index，防止重叠混乱
    if (!el.style.zIndex) {
      el.style.zIndex = 10 + index;
    }
  });
  // 修正所有图标位置
  const icons = clonedDoc.querySelectorAll('i');
  icons.forEach(icon => {
    icon.style.position = 'relative';
    icon.style.top = '-2px';
    icon.style.display = 'inline-block';
    icon.style.lineHeight = '1';
  });
  // 特别处理标题图标
  const headerIcons = clonedDoc.querySelectorAll('.header-icon i');
  headerIcons.forEach(icon => {
    icon.style.top = '-4px';
  });
  // 特别处理列标题图标
  const columnIcons = clonedDoc.querySelectorAll('.column-icon i');
  columnIcons.forEach(icon => {
    icon.style.top = '-3px';
  });
  // 确保文本容器不重叠
  const textContainers = clonedCard.querySelectorAll('p, h1, h2, h3, h4, h5, h6, span, div');
  textContainers.forEach(el => {
    // 如果没有明确的overflow设置，添加overflow:hidden
    if (!el.style.overflow) {
      el.style.overflow = 'hidden';
    }
  });
});
});

canvas.toBlob(function(blob) {
  // 创建下载链接
  const link = document.createElement('a');
  // 从卡片标题获取文件名，如果没有则使用默认名称
  const title = document.querySelector('.card-title') || document.querySelector('h1');
  const fileName = (title ? title.textContent.trim().substring(0, 30) : '文章概念卡片') + '.png';
  link.download = fileName;
  link.href = URL.createObjectURL(blob);
  link.click();
  // 清理URL对象
  URL.revokeObjectURL(link.href);
  // 恢复按钮状态和显示
  downloadBtn.style.display = 'flex';
  downloadBtn.innerHTML = originalHTML;
  downloadBtn.disabled = false;
}, 'image/png', 1.0);

} catch (error) {
  console.error('生成图片失败:', error);
  alert('生成图片失败，请重试');
  // 恢复按钮状态
  this.style.display = 'flex';
  this.innerHTML = '<i class="fas fa-download"></i> 下载卡片PNG图像';
  this.disabled = false;
});
</script>
</body>
</html>
```

## 溢出防护技术
* **固定尺寸容器**：使用固定尺寸的卡片容器
* **内容限制**：使用自定义的`text-clamp`类限制文本显示行数
* **溢出控制**：为所有容器添加`overflow-hidden`类
* **框模型控制**：使用`box-border`确保尺寸计算包含内边距和边框
* **预警系统**：实时监控内容高度，预警潜在溢出风险

## 图标渲染保障技术
* **CSS预调整**：使用相对定位和`line-height`微调图标位置
* **克隆时二次调整**：在`html2canvas`的`onclone`回调中再次精确调整
* **分类处理策略**：为不同类型和位置的图标应用专门调整
* **渲染等待机制**：确保字体和图标资源完全加载后再生成图像
* **图标容器稳定**：使用固定尺寸的图标容器确保稳定的视觉效果

## 设计准则（下载功能为必选项）
* 【溢出预防】宁可减少内容，也不允许溢出边界
* 【完成优先】设计完整性优先于内容完整性
* 【下载必备】每个设计必须包含正常工作的下载按钮和完整下载功能
* 【层次分明】使用区域弹性分配合理规划核心区与创意区
* 【留白节奏】保持至少20%的留白空间，创造视觉呼吸
* 【工具类优先】优先使用CSS工具类，减少自定义CSS
* 【语义化图标】使用专业图标库表达核心概念
* 【内容驱动设计】所有设计决策基于对文章内容的理解
* 【图标位置精准】采用双层调整策略确保图标在下载图像中完美呈现

## 核心原则
在固定空间内，内容必须适应空间，而非空间适应内容。严格遵循尺寸限制，任何内容都不能溢出1080px × 800px的边界。每个概念卡片必须包含高保真下载功能，确保设计成果可以完整导出为PNG图像，保留所有设计细节包括精确定位的图标。通过内容分析、分层与转化，在确保技术稳定性的同时，创造最能表达文章精髓的视觉设计。

## 特别注意事项
1. 下载功能不是可选的，而是必备的核心功能
2. 必须完整实现第五阶段的所有要点
3. 下载按钮样式可以调整，但下载功能代码不得简化或删减
4. 优先使用`html2canvas`而非`html-to-image`库
5. 图标位置调整是高保真下载的关键，不得忽略
6. **代码长度不是考虑因素** - 尽最大可能实现最佳效果，不要因代码简洁而牺牲功能完整性
7. **完整性高于简洁性** - 请复制完整的下载功能代码，不要试图精简或重写
8. **图像质量为王** - 所有复杂代码的目的都是为了确保导出图像的高质量，这比代码优雅更重要
9. **必要的复杂性** - 图标位置调整等复杂实现是解决技术限制的必要手段，不应被简化

## 文章内容

[在这里粘贴需要总结的文章内容]

# 三、提示词的难点解析

这个提示词我前后迭代了30多次，测卡片大概弄了100多张吧，崩溃了无数次，最后借着AI硬是熬了出来了。

那这个提示词最大的难点是什么？

**既要稳定性又要创造性。**

如果只让AI随机生成不管卡片大小，那其实很好搞，效果还不错，AI就自由发挥就好了。

如果只用一个样式生成卡片，那只要约束好逻辑，稳定性会很好，就是所有卡片都会长得一模一样。

稳定性和创造性是一体两面，本身就是对立打架的。

所以前期我的卡片不是这蹦就是那蹦。

![](img/a35a5708e726615e1b7813e36e748f37.png)

我是最终怎么解决这个问题的？

很坦诚的讲，我用人类的思维没解决这个问题，我想了无数个办法实在没办法了我去问AI了，它给我捣鼓出来什么CDI、防溢出策略，最后结合我的四步流程搞出来了。

拉上AI一起不断的探索问题，然后不断测试，重复这个过程几十遍堆出来的。

**弄完这个最终版的提示词我很感**慨的，见过很多东西比掌握更加重要。

我没有见过cdi、不了解什么是图标库，我其实没发找AI碰出来这个答案，它其实是我认知之外的内容，所以AI时代还是努力多看多收集，拓展自己的视野。

希望提示词对大家看文章有帮助呀，欢迎反馈~
