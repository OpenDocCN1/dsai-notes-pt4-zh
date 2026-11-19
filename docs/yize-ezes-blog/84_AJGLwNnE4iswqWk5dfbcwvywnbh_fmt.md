
# 竹韵

> 来源：[https://chatmemo.feishu.cn/docx/E1YhdZQ6EoeXN5xDRTNcNr9Jnbe](https://chatmemo.feishu.cn/docx/E1YhdZQ6EoeXN5xDRTNcNr9Jnbe)

<https://chatmemo.feishu.cn/sync/SIErdovINs8WT1ba0sJc5f4fnsh>



# 卡片组件 HTML/CSS 模板

```html
<style>
  @import url('https://cdn.jsdelivr.net/npm/lxgw-wenkai-webfont@1.1.0/style.css');

  .card {
    width: 424px;
    box-sizing: border-box;
    padding: 32px;
    background: #FDFDF9;
    border-radius: 2px 2px 20px 20px;
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
    background: linear-gradient(90deg, #7A936F 0%, #90A437 100%);
    opacity: 0.8;
  }

  .bamboo-decoration {
    position: absolute;
    top: 20px;
    right: 28px;
    width: 45px;
    height: 90px;
    opacity: 0.12;
    pointer-events: none;
    transform: scale(0.95);
  }

  .bamboo-decoration svg {
    filter: saturate(0.95) brightness(1.02);
  }

  /* 其他样式保持不变 */
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    padding-bottom: 16px;
    border-bottom: 1px solid #E8EDE3;
  }

  .date {
    font-size: 14px;
    color: #7A936F;
    font-family: 'LXGW WenKai', serif;
  }

  .product-name {
    font-family: 'LXGW WenKai', serif;
    font-size: 13px;
    font-weight: 500;
    color: #7A936F;
    letter-spacing: 0.1em;
  }

  .title {
    font-family: 'LXGW WenKai', serif;
    font-size: 22px;
    font-weight: 700;
    color: #2C3A28;
    margin-bottom: 12px;
    line-height: 1.5;
    letter-spacing: 0.03em;
  }

  .authors {
    font-size: 14px;
    color: #7A936F;
    margin-bottom: 24px;
    font-style: italic;
    font-family: 'LXGW WenKai', serif;
  }

  .summary {
    background: #F0F4EA;
    padding: 20px;
    border-radius: 12px;
    margin-bottom: 24px;
    font-size: 15px;
    color: #2C3A28;
    line-height: 1.8;
    letter-spacing: 0.02em;
    border-left: 3px solid #7A936F;
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
    color: #2C3A28;
    padding: 2px 0;
    line-height: 1.6;
    font-family: 'LXGW WenKai', serif;
  }

  .point:before {
    content: "•";
    color: #7A936F;
    margin-right: 8px;
    font-size: 17px;
    line-height: 1.5;
    margin-top: 0.5px;
  }

  .qr-section {
    border-top: 1px solid #E8EDE3;
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
    background: #FFFFFF;
    border: 1px solid #E8EDE3;
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
    color: #2C3A28;
    margin-bottom: 6px;
  }

  .qr-subtitle {
    font-family: 'LXGW WenKai', serif;
    font-size: 14px;
    color: #5C6F54;
    margin-bottom: 6px;
    line-height: 1.4;
  }

  .platform {
    font-family: 'LXGW WenKai', serif;
    font-size: 14px;
    color: #7A936F;
    display: flex;
    align-items: center;
  }
</style>

<div class="card">
  <div class="bamboo-decoration">
    <svg version="1.1" xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100" preserveAspectRatio="xMidYMid meet">
      <path d="M0 0 C0.66 0 1.32 0 2 0 C2.33 3.3 2.66 6.6 3 10 C3.50789063 9.4946875 4.01578125 8.989375 4.5390625 8.46875 C10.23156089 3.09605489 10.23156089 3.09605489 14.5 2.5625 C15.325 2.706875 16.15 2.85125 17 3 C17.66 4.98 18.32 6.96 19 9 C16.07284059 12.24623365 13.37089537 14.00516159 9.375 15.75 C8.41335937 16.17796875 7.45171875 16.6059375 6.4609375 17.046875 C4 18 C4 18 C2 18 C2.06960937 19.24136719 2.13921875 20.48273437 2.2109375 21.76171875 C2.26723028 23.40359158 2.32182881 25.04552311 2.375 26.6875 C2.45041016 27.91243164 2.45041016 27.91243164 2.52734375 29.16210938 C2.67199594 35.11695772 2.67199594 35.11695772 0.70800781 37.69775391 C-3.1242284 41 C-3.1242284 41 C-6 41 C-5.9278125 42.06089844 -5.855625 43.12179688 -5.78125 44.21484375 C-4.75938034 60.80519817 -4.9103601 77.38673823 -5 94 C-9.62 94 C-14.24 94 C-19 94 C-18.67 92.35 C-18.34 90.7 C-18 89 C-17.91002429 86.98980682 C-17.86937485 84.97704837 C-17.8671875 82.96484375 C-17.86589844 81.82595703 C-17.86460938 80.68707031 C-17.86328125 79.51367188 C-17.86714844 78.33353516 C-17.87101562 77.15339844 C-17.875 75.9375 C-17.86919922 74.16149414 C-17.86919922 74.16149414 C-17.86328125 72.34960938 C-17.86457031 71.21201172 C-17.86585937 70.07441406 C-17.8671875 68.90234375 C-17.86831543 67.86585693 C-17.86944336 66.82937012 C-17.87060547 65.76147461 C-17.90095329 63.07034856 C-17.90095329 63.07034856 C-18.57080078 60.81860352 C-19 59 C-19 59 C-18 56 C-17.90525114 53.98220689 C-17.86927784 51.9614216 C-17.8671875 49.94140625 C-17.86589844 48.75095703 C-17.86460938 47.56050781 C-17.86328125 46.33398438 C-17.86714844 45.08939453 C-17.87101562 43.84480469 C-17.875 42.5625 C-17.87113281 41.32177734 C-17.86726563 40.08105469 C-17.86328125 38.80273438 C-17.86457031 37.61099609 C-17.86585937 36.41925781 C-17.8671875 35.19140625 C-17.86831543 34.10029541 C-17.86944336 33.00918457 C-17.87060547 31.88500977 C-17.98192154 29.40308133 C-18.38924812 27.38429475 C-19 25 C-18 24 C-18 24 C-15.05859375 23.90234375 C-11.70572917 23.93489583 C-8.35286458 23.96744792 C-5 24 C-5 28.29 C-5 32.58 C-5 37 C-1.9894594 35.39030905 C-1.9894594 35.39030905 C-1.3125 33.375 C-0.85528579 29.900172 C-0.94159769 26.50413832 C-1 23 C-4.3 21.35 C-7.6 19.7 C-11 18 C-11 16.02 C-11 14.04 C-11 12 C-9.3125 11.3125 C-9.3125 11.3125 C-7 11 C-4.75643706 12.14786941 C-2.7904233 13.2095767 C-1 15 C-0.67 10.05 C-0.34 5.1 C0 0 Z" fill="#527481" transform="translate(50,3)"/>
      <path d="M0 0 C0 10.56 C0 21.12 C0 32 C-2.31 32 C-4.62 32 C-7 32 C-7 21.77 C-7 11.54 C-7 1 C-5 0 C-5 0 C0 0 Z" fill="#719B5D" transform="translate(42,29)"/>
      <path d="M0 0 C1.65 0 C3.3 0 C5 0 C5 10.56 C5 21.12 C5 32 C3.35 32 C1.7 32 C0 32 C0 21.44 C0 10.88 C0 0 Z" fill="#91CA6D" transform="translate(37,63)"/>
      <path d="M0 0 C1.98 0.495 C1.98 0.495 C4 1 C4 10.9 C4 20.8 C4 31 C2.35 31 C0.7 31 C-1 31 C-0.67 20.77 C-0.34 10.54 C0 0 Z" fill="#A7EA7B" transform="translate(38,30)"/>
      <path d="M0 0 C0.5775 0.226875 C1.155 0.45375 C1.75 0.6875 C1.52734375 2.54296875 C1.52734375 2.54296875 C0.75 4.6875 C-1.24609375 5.98828125 C-1.24609375 5.98828125 C-3.6875 7 C-4.89212891 7.52013672 C-4.89212891 7.52013672 C-6.12109375 8.05078125 C-8.25 8.6875 C-8.25 8.6875 C-10.25 7.6875 C-4.01086957 -0.55706522 C-4.01086957 -0.55706522 C0 0 Z" fill="#93CC73" transform="translate(64.25,8.3125)"/>
      <path d="M0 0 C1.65 0 C3.3 0 C5 0 C6.125 2.8125 C6.125 2.8125 C7 6 C6.01 7.485 C6.01 7.485 C5 9 C4.01 8.67 C3.02 8.34 C2 8 C2.33 6.35 C2.66 4.7 C3 3 C1.68 3 C0.36 3 C-1 3 C-0.67 2.01 C-0.34 1.02 C0 0 Z" fill="#4D6C87" transform="translate(62,6)"/>
    </svg>
  </div>
  <div class="header">
    <div class="date">{{DATE}}</div>
    <div class="product-name">好文分享</div>
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
    <div id="qrcode"></div>
    <div class="qr-info">
      <div class="qr-title">{{QR_TITLE}}</div>
      <div class="qr-subtitle">{{QR_SUBTITLE}}</div>
      <div class="platform">{{PLATFORM}}</div>
    </div>
  </div>
</div>
```
