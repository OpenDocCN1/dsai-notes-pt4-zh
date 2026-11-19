
```
&lt;template&gt;
  &lt;div class=&quot;ai-website&quot;&gt;
    &lt;!-- Logo --&gt;
    &lt;div class=&quot;logo&quot;&gt;
      &lt;img src=&quot;https://via.placeholder.com/150x50?text=AI+Logo&quot; alt=&quot;AI Logo&quot; /&gt;
    &lt;/div&gt;

    &lt;!-- 左侧时间轴 --&gt;
    &lt;div class=&quot;timeline&quot;&gt;
      &lt;div
        v-for=&quot;(item, index) in timelineItems&quot;
        :key=&quot;index&quot;
        class=&quot;timeline-item&quot;
        :class=&quot;{ active: activeSection === item.id }&quot;
        @click=&quot;scrollToSection(item.id)&quot;
      &gt;
        &lt;div class=&quot;timeline-dot&quot;&gt;&lt;/div&gt;
        &lt;div class=&quot;timeline-label&quot;&gt;{{ item.label }}&lt;/div&gt;
      &lt;/div&gt;
    &lt;/div&gt;

    &lt;!-- 右侧内容区域 --&gt;
    &lt;div class=&quot;content&quot;&gt;
      &lt;!-- AI 内容创作 --&gt;
      &lt;section id=&quot;content-creation&quot; class=&quot;section&quot;&gt;
        &lt;h1 class=&quot;section-title&quot;&gt;AI 内容创作&lt;/h1&gt;
        &lt;div class=&quot;feature-grid&quot;&gt;
          &lt;div
            v-for=&quot;(item, index) in contentFeatures&quot;
            :key=&quot;index&quot;
            class=&quot;feature-card&quot;
            @click=&quot;handleFeatureClick(item)&quot;
          &gt;
            &lt;h2&gt;{{ item.title }}&lt;/h2&gt;
            &lt;p&gt;{{ item.description }}&lt;/p&gt;
          &lt;/div&gt;
        &lt;/div&gt;
      &lt;/section&gt;

      &lt;!-- AI 绘画 --&gt;
      &lt;section id=&quot;ai-painting&quot; class=&quot;section&quot;&gt;
        &lt;h1 class=&quot;section-title&quot;&gt;AI 绘画&lt;/h1&gt;
        &lt;div class=&quot;feature-grid&quot;&gt;
          &lt;div
            v-for=&quot;(item, index) in paintingFeatures&quot;
            :key=&quot;index&quot;
            class=&quot;feature-card&quot;
            @click=&quot;handleFeatureClick(item)&quot;
          &gt;
            &lt;h2&gt;{{ item.title }}&lt;/h2&gt;
            &lt;p&gt;{{ item.description }}&lt;/p&gt;
          &lt;/div&gt;
        &lt;/div&gt;
      &lt;/section&gt;

      &lt;!-- AI 探索 --&gt;
      &lt;section id=&quot;ai-exploration&quot; class=&quot;section&quot;&gt;
        &lt;h1 class=&quot;section-title&quot;&gt;AI 探索&lt;/h1&gt;
        &lt;div class=&quot;feature-grid&quot;&gt;
          &lt;div
            v-for=&quot;(item, index) in explorationFeatures&quot;
            :key=&quot;index&quot;
            class=&quot;feature-card&quot;
            @click=&quot;handleFeatureClick(item)&quot;
          &gt;
            &lt;h2&gt;{{ item.title }}&lt;/h2&gt;
            &lt;p&gt;{{ item.description }}&lt;/p&gt;
          &lt;/div&gt;
        &lt;/div&gt;
      &lt;/section&gt;
    &lt;/div&gt;

    &lt;!-- 登录模态框 --&gt;
    &lt;el-dialog v-model=&quot;showLoginModal&quot; title=&quot;登录&quot; width=&quot;30%&quot; center&gt;
      &lt;Login @login-success=&quot;handleLoginSuccess&quot; /&gt;
    &lt;/el-dialog&gt;
  &lt;/div&gt;
&lt;/template&gt;

&lt;script setup&gt;
import { ref, onMounted, onUnmounted } from &#x27;vue&#x27;;
import Login from &#x27;@/views/Login/Login.vue&#x27;;

// 时间轴数据
const timelineItems = ref([
  { id: &#x27;content-creation&#x27;, label: &#x27;AI 内容创作&#x27; },
  { id: &#x27;ai-painting&#x27;, label: &#x27;AI 绘画&#x27; },
  { id: &#x27;ai-exploration&#x27;, label: &#x27;AI 探索&#x27; }
]);

// 当前激活的区域
const activeSection = ref(&#x27;content-creation&#x27;);

// AI 内容创作功能列表
const contentFeatures = ref([
  { title: &#x27;长文创作&#x27;, description: &#x27;生成高质量的长篇文章&#x27; },
  { title: &#x27;创作灵感&#x27;, description: &#x27;获取创作灵感和思路&#x27; }
]);

// AI 绘画功能列表
const paintingFeatures = ref([
  { title: &#x27;文生图&#x27;, description: &#x27;根据文字描述生成图像&#x27; },
  { title: &#x27;图生图&#x27;, description: &#x27;根据图像生成新的图像&#x27; },
  { title: &#x27;二维码制作&#x27;, description: &#x27;生成个性化的二维码&#x27; },
  { title: &#x27;艺术字&#x27;, description: &#x27;生成艺术字体&#x27; },
  { title: &#x27;百家姓&#x27;, description: &#x27;生成百家姓艺术字&#x27; },
  { title: &#x27;百变头像&#x27;, description: &#x27;生成个性化头像&#x27; }
]);

// AI 探索功能列表
const explorationFeatures = ref([
  { title: &#x27;AI 实验室&#x27;, description: &#x27;探索最新的 AI 技术&#x27; },
  { title: &#x27;AI 社区&#x27;, description: &#x27;加入 AI 爱好者社区&#x27; },
  { title: &#x27;AI 新闻&#x27;, description: &#x27;了解 AI 领域的最新动态&#x27; }
]);

// 是否显示登录模态框
const showLoginModal = ref(false);

// 处理功能点击
const handleFeatureClick = (item) =&gt; {
  const token = localStorage.getItem(&#x27;token&#x27;);
  if (!token) {
    showLoginModal.value = true; // 显示登录模态框
  } else {
    alert(`跳转到: ${item.title}`); // 模拟跳转
  }
};

// 处理登录成功
const handleLoginSuccess = () =&gt; {
  showLoginModal.value = false;
  alert(&#x27;登录成功，可以访问功能了！&#x27;);
};

// 滚动到指定区域
const scrollToSection = (id) =&gt; {
  const section = document.getElementById(id);
  if (section) {
    section.scrollIntoView({ behavior: &#x27;smooth&#x27; });
    activeSection.value = id; // 更新当前激活的区域
  }
};

// 监听滚动事件，更新激活的区域
const handleScroll = () =&gt; {
  const sections = timelineItems.value.map((item) =&gt; document.getElementById(item.id));
  const scrollPosition = window.scrollY + 100; // 偏移量

  for (let i = sections.length - 1; i &gt;= 0; i--) {
    if (sections[i].offsetTop &lt;= scrollPosition) {
      activeSection.value = timelineItems.value[i].id;
      break;
    }
  }
};

onMounted(() =&gt; {
  window.addEventListener(&#x27;scroll&#x27;, handleScroll);
});

onUnmounted(() =&gt; {
  window.removeEventListener(&#x27;scroll&#x27;, handleScroll);
});
&lt;/script&gt;

&lt;style lang=&quot;scss&quot; scoped&gt;
.ai-website {
  display: flex;
  min-height: 100vh;
  background: linear-gradient(135deg, #1e1e2f, #2a2a40);
  color: white;
  font-family: Arial, sans-serif;
  position: relative;

  .logo {
    position: absolute;
    top: 20px;
    left: 20px;
    z-index: 10;

    img {
      width: 150px;
      height: auto;
    }
  }
}

.timeline {
  width: 200px;
  padding: 20px;
  position: fixed;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start; /* 左对齐 */

  .timeline-item {
    display: flex;
    align-items: center;
    margin: 20px 0;
    cursor: pointer;
    transition: all 0.3s;

    &:hover {
      opacity: 0.8;
    }

    &.active {
      .timeline-dot {
        background: #409EFF;
        box-shadow: 0 0 10px #409EFF;
      }
      .timeline-label {
        color: #409EFF;
        font-weight: bold;
      }
    }

    .timeline-dot {
      width: 12px;
      height: 12px;
      background: white;
      border-radius: 50%;
      margin-right: 10px;
      transition: all 0.3s;
    }

    .timeline-label {
      font-size: 16px;
      transition: all 0.3s;
      text-align: left; /* 文字左对齐 */
    }
  }
}

.content {
  margin-left: 200px;
  flex: 1;
  padding: 20px;

  .section {
    margin-bottom: 100px;

    .section-title {
      font-size: 32px;
      margin-bottom: 40px;
      text-align: center;
      color: #409EFF;
    }

    .feature-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 20px;

      .feature-card {
        background: rgba(255, 255, 255, 0.1);
        border-radius: 10px;
        padding: 20px;
        cursor: pointer;
        transition: all 0.3s;

        &:hover {
          transform: scale(1.05);
          box-shadow: 0 0 20px rgba(64, 158, 255, 0.5);
        }

        h2 {
          font-size: 20px;
          margin-bottom: 10px;
        }

        p {
          color: #aaa;
        }
      }
    }
  }
}

/* 移动端适配 */
@media (max-width: 768px) {
  .ai-website {
    flex-direction: column;

    .logo {
      position: static;
      text-align: center;
      margin-top: 20px;
    }

    .timeline {
      width: 100%;
      height: auto;
      position: static;
      flex-direction: row;
      justify-content: center;
      padding: 10px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);

      .timeline-item {
        margin: 0 10px;
      }
    }

    .content {
      margin-left: 0;
      padding: 10px;

      .section {
        margin-bottom: 50px;

        .section-title {
          font-size: 24px;
        }

        .feature-grid {
          grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
        }
      }
    }
  }
}
&lt;/style&gt;
```
