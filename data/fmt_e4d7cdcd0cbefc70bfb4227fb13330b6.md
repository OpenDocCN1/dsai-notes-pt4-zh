
# LangChain Dark

> 来源：`https://chatmemo.feishu.cn/docx/Lrxwd3jtIo7wE4xmyQucMxaBn6c`

> `https://chatmemo.feishu.cn/sync/SIErdovINs8WT1ba0sJc5f4fnsh`

> 作者：沧海九粟（`space.bilibili.com/28357052`）

```
<style>
  @import url('https://cdn.jsdelivr.net/npm/lxgw-wenkai-webfont@1.1.0/style.css');
  :root {
    --langchain-primary: #1B4542;     /* 深绿色主色 */
    --langchain-secondary: #2B5753;    /* 次要绿色 */
    --langchain-bg: #0A1F1D;          /* 深色背景 */
    --langchain-accent: #2D8277;      /* 强调色 */
  }
  .card {
    width: 424px;
    box-sizing: border-box;
    padding: 32px;
    background: linear-gradient(180deg,
      rgba(27,69,66,0.95) 0%,
      rgba(10,31,29,1) 100%
    );
    border-radius: 2px 2px 20px 20px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.3);
    position: relative;
    overflow: hidden;
  }
  
  .card::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 3px;
    background: linear-gradient(90deg, var(--langchain-primary) 0%, var(--langchain-secondary) 100%);
    opacity: 0.8;
  }
  .langchain-decoration {
    position: absolute;
    top: 20px;
    right: 28px;
    font-size: 24px;
    opacity: 0.15;
    color: #FFFFFF;
  }
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    padding-bottom: 16px;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  }
  .date {
    font-size: 14px;
    color: #9DB4B0;
    font-family: 'LXGW WenKai', serif;
  }
  .product-name {
    font-family: 'LXGW WenKai', serif;
    font-size: 13px;
    font-weight: 500;
    color: #9DB4B0;
    letter-spacing: 0.1em;
  }
  .title {
    font-family: 'LXGW WenKai', serif;
    font-size: 22px;
    font-weight: 700;
    color: #FFFFFF;
    margin-bottom: 12px;
    line-height: 1.5;
    letter-spacing: 0.03em;
    display: flex;
    align-items: center;
  }
  .title:before {
    content: "🦜";
    margin-right: 8px;
    font-size: 0.9em;
    text-shadow: 0 0 15px rgba(45,130,119,0.3);
  }
  .authors {
    font-size: 14px;
    color: #9DB4B0;
    margin-bottom: 24px;
    font-family: 'LXGW WenKai', serif;
  }
  .summary {
    background: rgba(45, 130, 119, 0.1);
    padding: 20px;
    border-radius: 12px;
    margin-bottom: 24px;
    font-size: 15px;
    color: #E6E6E6;
    line-height: 1.8;
    letter-spacing: 0.02em;
    border-left: 3px solid var(--langchain-accent);
    font-family: 'LXGW WenKai', serif;
  }
  .points {
    margin-bottom: 24px;
  }
  .point {
    display: flex;
    align-items: flex-start;
    margin-bottom: 16px;
    font-size: 15px;
    color: #E6E6E6;
    padding: 2px 0;
    line-height: 1.6;
    font-family: 'LXGW WenKai', serif;
  }
  .point:before {
    content: "🔗";
    margin-right: 8px;
    font-size: 14px;
  }
  .qr-section {
    border-top: 1px solid rgba(255, 255, 255, 0.1);
    padding-top: 24px;
    display: flex;
    align-items: center;
  }
  #qrcode {
    width: 76px;
    height: 76px;
    margin-right: 16px;
    border-radius: 4px;
    padding: 6px;
    background: rgba(45, 130, 119, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.1);
    display: flex;
    align-items: center;
    justify-content: center;
    box-sizing: border-box;
  }
  #qrcode img {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }
  .qr-info {
    flex: 1;
  }
  .qr-title {
    font-family: 'LXGW WenKai', serif;
    font-size: 16px;
    font-weight: 700;
    color: #FFFFFF;
    margin-bottom: 6px;
  }
  .qr-subtitle {
    font-family: 'LXGW WenKai', serif;
    font-size: 14px;
    color: #9DB4B0;
    margin-bottom: 6px;
    line-height: 1.4;
  }
  .platform {
    font-family: 'LXGW WenKai', serif;
    font-size: 14px;
    color: #9DB4B0;
    display: flex;
    align-items: center;
  }
</style>
<div class="card">
  <div class="langchain-decoration">🦜🔗</div>
  <div class="header">
    <div class="date">{{DATE}}</div>
    <div class="product-name"></div>
  </div>
  <h1 class="title">{{TITLE}}</h1>
  <div class="authors">{{AUTHORS}}</div>
  <div class="summary">
    {{SUMMARY}}
  </div>
  <div class="points">
    {{#each POINTS}}
    <div class="point">{{this}}</div>
    {{/each}}
  </div>
  <div class="qr-section">
    <div id="qrcode">
      <!-- 这里放置二维码图片 -->
    </div>
    <div class="qr-info">
      <div class="qr-title">{{QR_TITLE}}</div>
      <div class="qr-subtitle">{{QR_SUBTITLE}}</div>
      <div class="platform">{{PLATFORM}}</div>
    </div>
  </div>
</div>
```
