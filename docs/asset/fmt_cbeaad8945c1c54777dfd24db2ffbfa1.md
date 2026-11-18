
```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>我的作品集</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f5f5f5;
        }
        nav {
            background-color: #2c3e50;
            padding: 1rem 0;
            position: sticky;
            top: 0;
            z-index: 100;
        }
        nav ul {
            list-style: none;
            display: flex;
            justify-content: center;
            margin: 0;
            padding: 0;
        }
        nav li {
            margin: 0 2rem;
        }
        nav a {
            color: white;
            text-decoration: none;
            font-weight: bold;
            transition: color 0.3s;
        }
        nav a:hover {
            color: #3498db;
        }
        .hero {
            background: linear-gradient(135deg, #3498db, #2c3e50);
            color: white;
            text-align: center;
            padding: 6rem 2rem;
        }
        .hero h1 {
            font-size: 3.5rem;
            margin-bottom: 1rem;
        }
        .hero p {
            font-size: 1.5rem;
        }
        .gallery {
            max-width: 1200px;
            margin: 4rem auto;
            padding: 0 2rem;
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 2rem;
        }
        .project {
            background: white;
            border-radius: 12px;
            overflow: hidden;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            transition: transform 0.3s, box-shadow 0.3s;
        }
        .project:hover {
            transform: translateY(-10px);
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
        }
        .project-image {
            width: 100%;
            height: 200px;
            background: linear-gradient(135deg, #667eea, #764ba2);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 3rem;
        }
        .project-content {
            padding: 1.5rem;
        }
        .project h3 {
            margin: 0 0 1rem 0;
            color: #2c3e50;
        }
        .project p {
            color: #666;
            line-height: 1.6;
        }
        .project-tags {
            display: flex;
            gap: 0.5rem;
            margin-top: 1rem;
        }
        .tag {
            background: #3498db;
            color: white;
            padding: 0.3rem 0.8rem;
            border-radius: 20px;
            font-size: 0.85rem;
        }
    </style>
</head>
<body>
    <nav>
        <ul>
            <li><a href="#home">首页</a></li>
            <li><a href="#projects">项目</a></li>
            <li><a href="#about">关于</a></li>
            <li><a href="#contact">联系</a></li>
        </ul>
    </nav>

    <div class="hero" id="home">
        <h1>我的作品集</h1>
        <p>展示我的创意与技术</p>
    </div>

    <div class="gallery" id="projects">
        <div class="project">
            <div class="project-image">🎨</div>
            <div class="project-content">
                <h3>项目一</h3>
                <p>这是一个关于界面设计的项目，采用现代化的设计风格。</p>
                <div class="project-tags">
                    <span class="tag">UI设计</span>
                    <span class="tag">网页</span>
                </div>
            </div>
        </div>

        <div class="project">
            <div class="project-image">💻</div>
            <div class="project-content">
                <h3>项目二</h3>
                <p>响应式网站开发，适配各种设备屏幕。</p>
                <div class="project-tags">
                    <span class="tag">前端</span>
                    <span class="tag">响应式</span>
                </div>
            </div>
        </div>

        <div class="project">
            <div class="project-image">📱</div>
            <div class="project-content">
                <h3>项目三</h3>
                <p>移动应用界面设计，注重用户体验。</p>
                <div class="project-tags">
                    <span class="tag">移动端</span>
                    <span class="tag">UX</span>
                </div>
            </div>
        </div>

        <div class="project">
            <div class="project-image">🚀</div>
            <div class="project-content">
                <h3>项目四</h3>
                <p>使用最新技术栈开发的 Web 应用。</p>
                <div class="project-tags">
                    <span class="tag">React</span>
                    <span class="tag">现代化</span>
                </div>
            </div>
        </div>

        <div class="project">
            <div class="project-image">🎯</div>
            <div class="project-content">
                <h3>项目五</h3>
                <p>数据可视化项目，将复杂数据直观展示。</p>
                <div class="project-tags">
                    <span class="tag">数据</span>
                    <span class="tag">可视化</span>
                </div>
            </div>
        </div>

        <div class="project">
            <div class="project-image">✨</div>
            <div class="project-content">
                <h3>项目六</h3>
                <p>交互动画设计，提升用户体验。</p>
                <div class="project-tags">
                    <span class="tag">动画</span>
                    <span class="tag">交互</span>
                </div>
            </div>
        </div>
    </div>
</body>
</html>
```

### 模板 4：简历页面
