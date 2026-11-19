
# GitHub Pages 部署完整指南（从0开始详细版）

> 来源：`https://my.feishu.cn/docx/Gun1dnEKxoIBm4xUTrwc4hP1noh`

## 适用场景

从创建 `GitHub` 仓库开始，到将 HTML 静态网页部署上线，让别人可以通过网址访问你的网页。

## 准备工作

+ 有 `GitHub` 账号（没有的话去 `https://github.com` 注册）
+ 准备好你的 `HTML` 代码（或其他文件代码）
+ 一台能上外网的电脑
+ 一个浏览器（`Chrome`、`Edge`、`Safari` 等）

## 核心部分（必读）

### 第一步：创建新仓库（超详细）

#### 1.1 登录 `GitHub`

1. 打开浏览器，访问 `https://github.com`
1. 点击右上角的 `Sign in`（登录）
1. 输入你的用户名/邮箱和密码
1. 点击 `Sign in` 按钮登录

#### 1.2 进入创建仓库页面

**方法一：通过右上角加号**

1. 登录后，看到页面右上角你的头像旁边有一个 + 号
1. 点击这个 + 号
1. 在下拉菜单中选择 `New repository`（新建仓库）

**方法二：通过主页按钮**

1. 登录后，在 `GitHub` 主页左侧
1. 找到绿色的 `New` 按钮
1. 点击这个按钮

**方法三：直接访问链接**

+ 直接访问：`https://github.com/new`

#### 1.3 填写仓库信息（重要）

现在你会看到 `Create a new repository` 页面，按顺序填写：

##### ① `Repository name`（仓库名称）- 必填

+ 在第一个输入框输入仓库名称
+ 例如：`my-website` 或 `blog` 或 `xhs-content-checker`

**命名规则（重要）：**

+ ✅ 只能用：英文字母、数字、短横线（-）、下划线（_）
+ ✅ 好的例子：`my-website`、`personal-blog`、`project-2024`
+ ❌ 不能用：中文、空格、特殊符号（!@#$%等）
+ ❌ 坏的例子：我的网站、`my website`、`my-website!`

##### ② `Description`（描述）- 可选

+ 在第二个输入框简单描述你的项目
+ 例如：我的个人网站 或 `📕违禁词检测工具`
+ 这个可以不填，留空也行

##### ③ `Public / Private`（公开/私有）- 必选 `Public`

这里有两个选项，默认选中 `Public`：

+ 🔘 `Public`（公开） ← 必须选这个！
+ 任何人都能看到你的代码
+ 免费使用 `GitHub Pages`
+ ✅ 选这个
+ ⚪ `Private`（私有）
+ 只有你自己能看到
+ 需要付费才能用 `GitHub Pages`
+ ❌ 不要选这个（除非你是付费用户）

##### ④ `Initialize this repository with:`（初始化选项）

这里有几个复选框：

`Add a README file`（添加 `README` 文件）

+ ✅ 建议勾选
+ 这会创建一个 `README.md` 文件，介绍你的项目
+ 勾选后仓库不会是空的，方便操作

`Add .gitignore`（添加忽略文件）

+ ⚪ 不用勾选
+ 这个是用来忽略某些文件的，新手不需要

`.gitignore template`：下拉菜单

+ 不用管，保持 `None` 即可

`Choose a license`（选择许可证）

+ ⚪ 不用勾选
+ 除非你想开源并指定使用协议
+ 新手可以跳过

**推荐配置：**

+ ✅ 只勾选 `Add a README file`
+ ⚪ 其他都不勾选

#### 1.4 创建仓库

1. 检查所有信息是否正确
1. 点击页面底部的绿色按钮 `Create repository`
1. 等待 1-2 秒

#### 1.5 创建成功

恭喜！你现在应该看到：

+ 页面跳转到你的新仓库
+ 网址格式：`https://github.com/你的用户名/仓库名`
+ 页面显示 `README.md` 文件（如果你勾选了）
+ 顶部显示仓库名称

### 第二步：创建 `index.html` 文件（超详细）

#### 2.1 进入文件创建界面

**确保你在仓库首页：**

+ 网址应该是：`https://github.com/你的用户名/仓库名`
+ 如果不是，点击页面左上角的仓库名回到首页

**开始创建文件：**

1. 找到页面中间偏右的 `Add file` 按钮（添加文件）
1. 点击 `Add file` 按钮
1. 在下拉菜单中选择 `Create new file`（创建新文件）

#### 2.2 命名文件

现在你会看到一个新页面，顶部有个输入框写着 `Name your file...`

1. 在这个输入框里输入：`index.html`
+ ⚠️ 必须全部小写
+ ⚠️ 文件名必须是 `index.html`，不能改成其他名字
+ ⚠️ 注意后缀是 `.html` 不是 `.txt` 或其他
1. 输入后，下面会出现一个大的代码编辑框

#### 2.3 粘贴 `HTML` 代码

1. 点击大的编辑框（里面可能有一些灰色的提示文字）
1. 把你的 `HTML` 代码全选复制
1. 粘贴到这个编辑框里

如果你还没有 `HTML` 代码，可以先用这个简单的测试代码：

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>我的第一个网站</title>
</head>
<body>
    <h1>Hello World!</h1>
    <p>这是我的第一个 GitHub Pages 网站！</p>
</body>
</html>
```

#### 2.4 提交文件

滚动到页面最底部，你会看到 `Commit new file` 区域：

##### 第一个输入框（`Commit message`）

+ 默认显示：`Create index.html`
+ 你可以改成中文：创建首页文件 或 添加 `index.html`
+ 也可以不改，保持默认

##### 第二个输入框（`Extended description`）

+ 这是可选的详细描述
+ 可以留空不填

##### 单选按钮

+ 保持默认选中 `Commit directly to the main branch`
+ 不要选另一个选项

##### 提交按钮

1. 点击绿色按钮 `Commit new file`（提交新文件）
1. 等待 1-2 秒

#### 2.5 确认文件已创建

提交后，页面会自动跳转回仓库首页，你应该能看到：

+ 文件列表中出现了 `index.html` 文件
+ 文件名左边有个文档图标
+ 右边显示刚才的提交信息（如 "创建首页文件"）
+ 右边还显示提交时间（`now` 或 `1 minute ago`）

如果看到了 `index.html` 文件，说明创建成功！

### 第二步补充：如何创建其他文件（如 `.jsx` 文件）

#### 2A.1 创建 `.jsx` 文件的步骤

如果你需要创建像 `XiaohongshuChecker.jsx` 这样的文件：

1. **返回仓库首页**
+ 点击左上角的仓库名
1. **点击添加文件**
+ 点击 `Add file` 按钮
+ 选择 `Create new file`
1. **输入文件名**
+ 在 `Name your file...` 输入框输入：`XiaohongshuChecker.jsx`
+ 注意大小写要一致
+ 注意后缀是 `.jsx`
1. **粘贴代码**
+ 在大的编辑框里粘贴你的 `JSX` 代码
1. **提交文件**
+ 滚动到底部
+ 提交信息输入：添加 `XiaohongshuChecker` 组件
+ 点击 `Commit new file`

#### 2A.2 创建 `CSS` 文件

如果需要创建样式文件：

1. 点击 `Add file` → `Create new file`
1. **文件名输入：** `style.css` 或 `styles.css`
1. **粘贴 `CSS` 代码**
1. **提交**

#### 2A.3 创建子文件夹中的文件

如果要创建 `css/style.css`（在 `css` 文件夹里的文件）：

1. 点击 `Add file` → `Create new file`
1. **在文件名输入框输入：** `css/style.css`
+ 注意：输入 `css/` 后 `GitHub` 会自动识别这是文件夹
+ 页面会显示 `css` 文件夹和 `style.css` 文件分开
1. **粘贴代码**
1. **提交**

`GitHub` 会自动创建 `css` 文件夹，你不需要单独创建文件夹！

#### 2A.4 创建 `JavaScript` 文件

1. 点击 `Add file` → `Create new file`
1. **文件名输入：** `script.js` 或 `app.js`
1. 粘贴 `JavaScript` 代码
1. **提交**

#### 2A.5 批量创建多个文件

注意：`GitHub` 网页版一次只能创建一个文件

如果要创建多个文件：

1. 创建第一个文件（如 `index.html`）
1. 回到仓库首页
1. 再次点击 `Add file` → `Create new file`
1. 创建第二个文件（如 `style.css`）
1. 重复以上步骤

### 第三步：启用 `GitHub Pages`（超详细）

#### 3.1 进入仓库设置

1. **确保在仓库首页**
+ 网址：`https://github.com/你的用户名/仓库名`
1. **找到 `Settings` 标签**
+ 看页面顶部，从左到右依次是：
+ `< > Code`（代码）
+ `Issues`（问题）
+ `Pull requests`（拉取请求）
+ `Actions`（操作）
+ `Projects`（项目）
+ `Wiki`（维基）
+ `Security`（安全）
+ `Insights`（洞察）
+ ⚙️ `Settings`（设置）← 点这个！
+ ⚠️ 注意：这是仓库的 `Settings`，不是你账号的设置
1. **点击 `Settings`**
+ 页面会跳转到设置页面

#### 3.2 找到 `Pages` 设置

进入 `Settings` 后，你会看到：

1. **左侧有一个菜单栏**，从上到下依次是：
+ `General`（常规）
+ `Options`（选项）
+ `Access`（访问）
+ `Collaborators`（协作者）
+ `Code and automation`（代码和自动化）
+ ...（其他选项）
+ `Pages` ← 找到这个！
1. **滚动左侧菜单栏**
+ 往下滚动，找到 `Pages` 选项
+ 通常在靠下的位置
+ 图标是一个小文档的形状
1. **点击 `Pages`**
+ 点击后右侧会显示 `Pages` 配置页面

#### 3.3 配置 `GitHub Pages`

现在你应该看到 `Pages` 设置页面，上面写着 `GitHub Pages`

##### 情况 A：如果看到 `Build and deployment` 区域

这是正常情况，按以下步骤操作：

**① `Source`（来源）设置**

1. 找到 `Source` 标签（在 `Build and deployment` 下面）
1. 点击 `Source` 下面的第一个下拉菜单
1. 你会看到两个选项：
+ `Deploy from a branch` ← 选这个！
+ `GitHub Actions`
1. 选择 `Deploy from a branch`

**② `Branch`（分支）设置**

选择 `Deploy from a branch` 后，下面会出现分支选择：

1. 找到 `Branch` 标签
1. 点击第一个下拉菜单（可能显示 `None` 或 `main`）
1. 选择 `main`（或者 `master`，看你的仓库用哪个）
+ 大多数新仓库默认是 `main`
+ 老仓库可能是 `master`
1. 点击旁边的第二个下拉菜单（文件夹选择）
1. 选择 `/ (root)`（根目录）
+ 不要选 `/docs`

**③ 保存设置**

1. 点击旁边的蓝色按钮 `Save`
1. 页面会刷新

##### 情况 B：如果只看到 `Verified domains`

这说明 `Pages` 还没有激活，需要先创建 `index.html` 文件：

1. 返回第二步，确保创建了 `index.html` 文件
1. 创建完成后，重新进入 `Settings` → `Pages`
1. 这时应该能看到 `Build and deployment` 区域了
1. 按照情况 A 的步骤操作

##### 情况 C：如果仓库是私有的

你会看到提示：

> "Upgrade to GitHub Pro or make this repository public to enable Pages"

解决方法：

1. 回到 `Settings` 首页（点击左侧菜单最顶部的 `General`）
1. 滚动到最底部的 `Danger Zone`（危险区域）
1. 找到 `Change repository visibility`（更改仓库可见性）
1. 点击右边的 `Change visibility` 按钮
1. 选择 `Make public`（设为公开）
1. 输入仓库全名确认（格式：你的用户名/仓库名）
1. 输入你的密码确认
1. 点击确认按钮
1. 返回 `Settings` → `Pages`，现在应该能看到配置选项了

#### 3.4 等待部署

保存设置后：

1. **页面顶部会出现蓝色提示框：**

> "GitHub Pages source saved"

1. **等待构建**

+ `GitHub` 开始自动构建你的网站
+ 这个过程需要 `30秒 - 2分钟`
+ 不要关闭页面，可以刷新查看进度

1. **查看部署状态**

+ 保持在 `Pages` 设置页面
+ 每隔 `10` 秒刷新一次页面（按 `F5` 或点击刷新按钮）
+ 等待绿色提示框出现

#### 3.5 获取网站地址

部署完成后，你会看到：

1. **页面顶部出现绿色提示框：**

```
✅ Your site is live at https://你的用户名.github.io/仓库名/
```

1. **提示框里有个链接：**

+ 点击这个链接可以直接访问你的网站
+ 或者复制这个网址

1. **网址格式：**

+ `https://你的用户名.github.io/仓库名/`
+ 例如：`https://zhangsan.github.io/my-website/`

### 第四步：访问和测试你的网站

#### 4.1 第一次访问

1. **复制网站地址**

+ 从绿色提示框复制网址
+ 或者手动输入：`https://你的用户名.github.io/仓库名/`

1. **打开新标签页**

+ 在浏览器打开一个新标签页
+ 按 `Ctrl+T`（`Windows`）或 `Command+T`（`Mac`）

1. **访问网站**

+ 粘贴网址到地址栏
+ 按回车键
+ 等待页面加载

#### 4.2 可能遇到的问题

##### 问题 1：显示 `404 Not Found`

**如果看到 `404` 错误页面：**

1. **检查网址是否正确**

+ 确认格式：`https://用户名.github.io/仓库名/`
+ 注意最后有个斜杠 `/`
+ 用户名和仓库名都是小写

1. **检查是否还在部署**

+ 回到 `GitHub` 的 `Settings` → `Pages`
+ 看看绿色提示框是否已经出现
+ 如果还是蓝色或没有提示，说明还在部署
+ 再等 `1-2` 分钟

1. **检查文件名**

+ 回到仓库首页
+ 确认文件名是 `index.html`（全小写）
+ 不是 `Index.html` 或 `index.HTML`

1. **检查文件位置**

+ 确认 `index.html` 在仓库根目录
+ 不是在某个子文件夹里

##### 问题 2：页面是空白的

**如果页面打开了但是空白：**

1. **检查 `HTML` 代码**

+ 回到仓库，点击 `index.html`
+ 检查代码是否完整
+ 确认有 `<html>`、`<head>`、`<body>` 标签

1. **查看浏览器控制台**

+ 按 `F12` 打开开发者工具



## 部署检查与故障排查

### 控制台与源代码检查

+ 点击 "Console"（控制台）标签

+ 看看有没有红色错误信息

1. 查看页面源代码

+ 在页面上右键，选择 "查看页面源代码"

+ 确认代码已经正确加载

### 问题 3：样式或图片没有显示

如果 HTML 结构正常但样式或图片缺失：

#### 检查文件路径

+ CSS 文件路径是否正确

+ 图片路径是否正确

+ 使用相对路径，如：`./style.css` 或 `./images/logo.png`

#### 检查文件是否已上传

+ 回到仓库首页

+ 确认 CSS 文件、图片文件都已上传

#### 检查文件名大小写

+ 确保 HTML 里写的文件名和实际文件名大小写一致

+ Linux 系统区分大小写

### 4.3 成功访问

如果一切正常，你应该能看到：

+ 你的网页正确显示

+ 标题、内容、样式都正常

+ 可以正常交互（如果有 `JavaScript`）

恭喜你！你的网站已经成功上线了！🎉

---

简单来说，到这里就OK了，小结一下：

## 《GitHub Pages 部署指南（精简版）》

**只需要 4 步：**

1. 创建 `Public` 仓库

1. 创建 `index.html` 文件（粘贴你的 HTML 代码）

1. `Settings` → `Pages` → `Deploy from branch` → `main` → `/ (root)` → `Save`

1. 等 2 分钟，访问 `https://用户名.github.io/仓库名/`

**完成！**

## 后续维护和进阶👇

## 补充部分（可选阅读）

---

## 第五步：更新网站内容

### 5.1 编辑已有文件

假设你要修改 `index.html`：

1. **进入仓库首页**

+ 访问：`https://github.com/你的用户名/仓库名`

1. **点击要编辑的文件**

+ 点击 `index.html` 文件名

+ 页面会显示文件内容

1. **进入编辑模式**

+ 看到文件内容后，找到右上角的工具栏

+ 点击 `铅笔图标 ✏️`（`Edit this file`）

+ 或者点击文字 `Edit this file`

1. **修改代码**

+ 页面变成编辑模式，可以直接修改代码

+ 按 `Ctrl+F`（`Command+F`）可以查找内容

+ 直接输入或删除即可

1. **预览（可选）**

+ 编辑框上方有 `Edit` 和 `Preview` 两个标签

+ 点击 `Preview` 可以预览（但 HTML 预览效果有限）

1. **提交修改**

+ 滚动到页面底部

+ 在 `Commit changes` 第一个输入框输入修改说明

+ 例如：更新标题 或 修改联系方式

+ 第二个输入框可以写详细说明（可选）

+ 点击绿色按钮 `Commit changes`

1. **等待重新部署**

+ 提交后 GitHub 会自动重新部署

+ 等待 30秒 - 2分钟

1. **刷新网站查看更新**

+ 打开你的网站

+ 按 `Ctrl+F5`（Windows）或 `Command+Shift+R`（Mac）强制刷新

+ 查看更新是否生效

### 5.2 上传新文件

如果要添加图片、`CSS`、`JS` 等新文件：

#### 方法 A：创建文本文件（`.html`、`.css`、`.js`）

1. 点击 `Add file` → `Create new file`

1. **输入文件名**（如 `style.css` 或 `about.html`）

1. **粘贴代码**

1. **提交**

#### 方法 B：上传文件（图片、字体等）

1. **准备文件**

+ 在你的电脑上准备好要上传的文件

+ 例如：`logo.png`、`favicon.ico` 等

1. **进入上传界面**

+ 在仓库首页点击 `Add file`

+ 选择 `Upload files`

1. **上传文件**

+ **拖拽方式：**

+ 直接把文件从电脑文件夹拖到浏览器页面

+ 松开鼠标，文件开始上传

+ **选择方式：**

+ 点击 `choose your files` 链接

+ 在弹出的文件选择窗口选择文件

+ 点击"打开"

1. **可以一次上传多个文件**

+ 同时选中多个文件上传

+ 或者分批上传

1. **提交**

+ 等待上传完成（显示文件名和大小）

+ 滚动到底部

+ 输入提交说明：上传图片 或 添加资源文件

+ 点击 `Commit changes`

#### 方法 C：创建文件夹并上传文件

如果要创建 `images` 文件夹并上传图片：

1. 点击 `Add file` → `Upload files`

1. **创建文件夹：**

+ 在文件名前面加文件夹名和斜杠

+ 例如上传文件时命名为：`images/logo.png`

+ GitHub 会自动创建 `images` 文件夹

1. **上传文件**

1. **提交**

**注意**：在 `Upload files` 界面无法手动创建文件夹，只能通过 `Create new file` 时在文件名前加路径来创建

### 5.3 删除文件

如果要删除某个文件：

1. **点击要删除的文件**

+ 在仓库首页点击文件名

1. **进入删除界面**

+ 点击右上角的 `垃圾桶图标 🗑️`

+ 或者点击三个点 `...` 选择 `Delete file`

1. **确认删除**

+ 滚动到底部

+ 输入删除原因：删除旧文件 或 移除不需要的图片

+ 点击 `Commit changes`

1. **等待重新部署**

+ 删除文件后网站也会自动重新部署

### 5.4 重命名文件

1. **点击要重命名的文件**

1. **点击 `铅笔图标 ✏️`** 进入编辑模式

1. **修改文件名**

+ 在页面顶部的文件名输入框

+ 直接修改文件名

1. **提交**

+ 滚动到底部提交

### 5.5 查看部署状态

想知道修改后的网站是否部署完成：

1. **进入 `Actions` 页面**

+ 在仓库顶部点击 `Actions` 标签

1. **查看部署任务**

+ 会看到 `pages build and deployment` 列表

+ 最上面的是最新的部署

1. **查看状态**

+ `黄色圆圈`：正在部署中

+ `绿色对勾`：部署成功

+ `红色叉号`：部署失败

1. **等待完成**

+ 黄色圆圈通常会在 30秒 - 1分钟后变成绿色对勾

+ 看到绿色对勾后，刷新网站就能看到更新了

---

## 第六步：完整文件结构示例

### 6.1 简单网站结构

```
my-website/                    (仓库名)
├── index.html                 (首页，必须有)
├── about.html                 (关于页面)
├── style.css                  (样式文件)
├── script.js                  (JS 文件)
└── README.md                  (项目说明)
```

**访问方式：**

+ 首页：`https://用户名.github.io/my-website/`

+ 关于页：`https://用户名.github.io/my-website/about.html`

### 6.2 带子文件夹的结构

```
my-website/
├── index.html
├── about.html
├── css/
│   ├── style.css
│   └── reset.css
├── js/
│   ├── main.js
│   └── utils.js
├── images/
│   ├── logo.png
│   ├── banner.jpg
│   └── favicon.ico
└── README.md
```

**在 HTML 中引用文件：**

```
<!-- 引用 CSS -->
<link rel="stylesheet" href="css/style.css">

<!-- 引用 JavaScript -->
<script src="js/main.js"></script>

<!-- 引用图片 -->
<img src="images/logo.png" alt="Logo">
```

### 6.3 React 项目结构示例

```
xhs-content-checker/
├── index.html                      (入口文件，必须有)
├── XiaohongshuChecker.jsx         (React 组件)
├── components/
│   ├── Header.jsx
│   └── Footer.jsx
├── css/
│   └── style.css
├── .github/
│   └── workflows/
│       └── deploy.yml             (自动部署配置)
└── README.md
```

---

## 第七步：高级技巧

### 7.1 使用 GitHub Desktop（图形界面）

如果你觉得网页操作麻烦，可以用 `GitHub Desktop`：

#### 下载安装

1. 访问：`https://desktop.github.com`

1. 下载并安装 `GitHub Desktop`

1. 登录你的 `GitHub` 账号

#### 克隆仓库到本地

1. 打开 `GitHub Desktop`

1. 点击 `Clone a repository`

1. 选择你的仓库（如 `my-website`）

1. 选择本地保存位置

1. 点击 "Clone"

#### 编辑文件

1. 在电脑上用编辑器（记事本、`VS Code`等）打开文件

1. 修改并保存

#### 提交和推送

1. 回到 `GitHub Desktop`

1. 左侧会显示所有修改的文件

1. 在左下角输入提交信息（如"更新首页"）

1. 点击 `Commit to main`

1. 点击右上角的 `Push origin`

1. 修改会自动上传到 `GitHub`

### 7.2 使用命令行（Git）

#### 安装 Git

+ **Windows：** `https://git-scm.com/download/win`

+ **Mac：** 打开终端输入 `git --version`（会提示安装）

+ **Linux：** `sudo apt install git`（Ubuntu/Debian）

#### 克隆仓库

```
git clone https://github.com/你的用户名/仓库名.git
cd 仓库名
```

#### 修改文件后提交

```
# 查看修改
git status

# 添加所有修改
git add .

# 或添加单个文件
git add index.html

# 提交
git commit -m "更新首页内容"

# 推送到 GitHub
git push
```

#### 拉取最新代码

```
git pull
```

### 7.3 自定义域名

如果你有自己的域名（如 `www.mysite.com`）：

#### 在 GitHub 设置

1. 进入 `Settings` → `Pages`

1. 在 `Custom domain` 输入框输入你的域名

1. 点击 `Save`

1. 勾选 `Enforce HTTPS`

#### 在域名服务商设置 DNS

添加以下记录（以阿里云为例）：

**方式一：使用 A 记录**

```
类型：A
主机记录：@（或 www）
记录值：185.199.108.153
       185.199.109.153
       185.199.110.153
       185.199.111.153
```

**方式二：使用 CNAME 记录**

```
类型：CNAME
主机记录：www
记录值：你的用户名.github.io
```

#### 等待生效

+ `DNS` 生效时间：几分钟到 48 小时

+ 可以用 `https://dnschecker.org` 查询是否生效

### 7.4 添加网站统计

#### 使用 `Google Analytics`

1. 注册 `Google Analytics`

+ 访问：`https://analytics.google.com`

+ 创建账号和属性

+ 获取跟踪代码

1. **添加到网站**

+ 编辑 `index.html`

+ 在 `<head>` 标签内添加跟踪代码：

```
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

1. **提交更新**

1. **查看统计**

+ 24小时后在 `Google Analytics` 查看访问数据

### 7.5 添加 favicon（网站图标）

#### 准备图标

1. 制作一个 `16x16` 或 `32x32` 像素的 `.ico` 文件

1. 或使用在线工具转换：`https://www.favicon-generator.org`

#### 上传到仓库

1. 在仓库根目录上传 `favicon.ico` 文件

1. 或上传 `PNG` 格式：`favicon.png`

#### 在 HTML 中引用

编辑 `index.html`，在 `<head>` 标签内添加：

```
<!-- ICO 格式 -->
<link rel="icon" href="favicon.ico" type="image/x-icon">

<!-- PNG 格式 -->
<link rel="icon" href="favicon.png" type="image/png">

<!-- 多种尺寸 -->
<link rel="icon" sizes="16x16" href="favicon-16x16.png">
<link rel="icon" sizes="32x32" href="favicon-32x32.png">
<link rel="apple-touch-icon" sizes="180x180" href="apple-touch-icon.png">
```

### 7.6 添加 SEO 优化

在 `index.html` 的 `<head>` 标签内添加：

```
<meta property="og:image" content="https://用户名.github.io/仓库名/images/share.jpg">

<meta name="twitter:image" content="https://用户名.github.io/仓库名/images/share.jpg">
```



```
&lt;!-- 页面描述 --&gt;
&lt;meta name=&quot;description&quot; content=&quot;这是我的个人网站，分享技术文章和项目&quot;&gt;

&lt;!-- 关键词 --&gt;
&lt;meta name=&quot;keywords&quot; content=&quot;个人网站,博客,技术,编程&quot;&gt;

&lt;!-- 作者 --&gt;
&lt;meta name=&quot;author&quot; content=&quot;你的名字&quot;&gt;

&lt;!-- Open Graph（社交分享） --&gt;
&lt;meta property=&quot;og:title&quot; content=&quot;我的网站&quot;&gt;
&lt;meta property=&quot;og:description&quot; content=&quot;网站描述&quot;&gt;
&lt;meta property=&quot;og:url&quot; content=&quot;https://用户名.github.io/仓库名/&quot;&gt;

&lt;!-- Twitter Card --&gt;
&lt;meta name=&quot;twitter:card&quot; content=&quot;summary_large_image&quot;&gt;
&lt;meta name=&quot;twitter:title&quot; content=&quot;我的网站&quot;&gt;
&lt;meta name=&quot;twitter:description&quot; content=&quot;网站描述&quot;&gt;
```

## 7.7 启用 HTTPS（加密访问）

GitHub Pages 默认支持 HTTPS，但需要手动启用：

1. 进入 Settings → Pages
2. 找到 "`Enforce HTTPS`" 选项
3. 勾选这个复选框
4. 等待几分钟生效
5. 你的网站会强制使用 https:// 访问

---

## 常见问题完整汇总

### 问题 1：找不到 Settings

**原因：**

- 可能没有登录
- 可能在别人的仓库（你没有权限）

**解决：**

- 确认已登录
- 确认在自己的仓库页面

---

### 问题 2：Settings 里没有 Pages

**原因：** 仓库是私有的

**解决：**

1. Settings → General
2. 滚动到底部 Danger Zone
3. Change visibility → Make public
4. 输入仓库全名确认

---

### 问题 3：Pages 没有 Source 选项

**原因：** 仓库没有 `index.html`

**解决：** 先创建 `index.html` 文件

---

### 问题 4：网站显示 404

**可能原因：**

1. 部署还没完成 → 等待 2 分钟
2. 文件名不对 → 必须是 `index.html`
3. 文件不在根目录 → 移到根目录
4. 网址输错了 → 检查网址格式

---

### 问题 5：修改后网站没更新

**解决步骤：**

1. 确认修改已提交（查看文件是否更新）
2. 等待重新部署（1-2分钟）
3. 清除浏览器缓存（Ctrl+Shift+Delete）
4. 强制刷新（Ctrl+F5）
5. 用无痕模式访问

---

### 问题 6：样式不显示

**检查：**

1. CSS 文件路径是否正确
2. CSS 文件是否已上传
3. 文件名大小写是否一致
4. 使用相对路径：`./css/style.css` 或 `css/style.css`

---

### 问题 7：图片不显示

**检查：**

1. 图片路径是否正确
2. 图片文件是否已上传
3. 文件名大小写是否一致
4. 图片格式是否支持（.jpg、.png、.gif、.svg）

---

### 问题 8：JavaScript 不工作

**检查：**

1. 按 F12 打开控制台查看错误
2. JS 文件路径是否正确
3. JS 文件是否已上传
4. 是否有语法错误

---

### 问题 9：Actions 部署失败（红叉）

**解决：**

1. 不要使用 GitHub Actions 方式
2. 删除 `.github/workflows` 文件夹
3. Settings → Pages
4. Source 改为 `Deploy from a branch`
5. 重新部署

---

### 问题 10：想改仓库名

**步骤：**

1. Settings → General
2. 最顶部 Repository name 输入新名字
3. 点击 Rename
4. ⚠️ 网址也会变：https://用户名.github.io/新仓库名/

---

### 问题 11：想删除仓库

**步骤：**

1. Settings → General
2. 滚动到最底部 Danger Zone
3. Delete this repository
4. 输入完整仓库名：用户名/仓库名
5. 确认删除
6. ⚠️ 删除后无法恢复，网站也会下线

---

### 问题 12：提交时出现冲突

**原因：** 多人编辑或多处编辑导致冲突

**解决：**

1. 如果用网页编辑，GitHub 会提示冲突
2. 选择保留哪个版本
3. 或者手动合并

---

### 问题 13：文件上传失败

**可能原因：**

1. 文件太大（单个文件 > 25MB）
2. 网络不稳定

**解决：**

1. 压缩文件大小
2. 分批上传
3. 使用 Git LFS（大文件存储）
4. 换个时间或网络重试

---

### 问题 14：部署很慢

**原因：**

- GitHub 服务器在国外
- 网络延迟

**解决：**

- 耐心等待（通常 2-5 分钟）
- 可以去 Actions 页面查看进度

---

### 问题 15：网站访问慢

**原因：** GitHub Pages 服务器在国外

**解决方案：**

1. 使用 CDN 加速
2. 压缩图片大小
3. 压缩 CSS/JS 文件
4. 使用国内托管服务（如 Gitee Pages）

---

## 关键规则和注意事项总结

### ✅ 必须做的事情

1. 仓库必须是 `Public`（公开）

- 私有仓库需要付费

1. 首页必须叫 `index.html`

- 全部小写
- 不能是其他名字

1. `index.html` 必须在根目录

- 不能在子文件夹里

1. Source 选择 `Deploy from a branch`

- Branch 选择 `main` 或 `master`
- 文件夹选择 `/(root)`

1. 文件命名规范

- 只用英文、数字、短横线、下划线
- 不用空格和中文
- 注意大小写

1. 提交后要等待

- 等待 30秒 - 2分钟让 GitHub 部署
- 查看 Actions 确认部署完成

1. 更新后要强制刷新

- Ctrl+F5 或 Command+Shift+R
- 清除浏览器缓存

---

### ❌ 不要做的事情

1. ❌ 不要把仓库设为 `Private`

- 会导致 Pages 无法使用

1. ❌ 不要用中文命名

- 仓库名、文件名都不要用中文

1. ❌ 不要在文件名里加空格

- 用短横线代替：`my-page.html` 而不是 `my page.html`

1. ❌ 不要把 `index.html` 放子文件夹

- 必须在根目录

1. ❌ 不要用 GitHub Actions

- 除非你很熟悉
- 新手用 `Deploy from a branch`

1. ❌ 不要频繁修改

- 每次修改都会重新部署
- 建议本地测试好再提交

1. ❌ 不要上传超大文件

- 单个文件不要超过 25MB
- 仓库总大小不要超过 1GB

---

## 网址格式说明

### 你的网站网址永远是：

```
https://你的GitHub用户名.github.io/仓库名/
```

### 规则：

- 用户名全小写
- 仓库名全小写
- 最后有斜杠 /

### 示例对照表：

| GitHub 用户名 | 仓库名 | 网站地址 |

| --- | --- | --- |

| zhangsan | my-website | https://zhangsan.github.io/my-website/ |

| LiSi88 | blog | https://lisi88.github.io/blog/ |

| wang_wu | portfolio | https://wang_wu.github.io/portfolio/ |

| XiaoMing | xhs-checker | https://xiaoming.github.io/xhs-checker/ |

### 子页面访问：

- 主页：https://用户名.github.io/仓库名/
- 子页面：https://用户名.github.io/仓库名/about.html
- 子文件夹：https://用户名.github.io/仓库名/blog/post1.html
- 资源文件：https://用户名.github.io/仓库名/images/logo.png

---

## 完整操作时间线

| 步骤 | 操作内容 | 预计时间 | 累计时间 |

| --- | --- | --- | --- |

| 1 | 登录 GitHub | 30秒 | 30秒 |

| 2 | 创建新仓库 | 1分钟 | 1.5分钟 |

| 3 | 创建 `index.html` | 2分钟 | 3.5分钟 |

| 4 | 配置 Pages | 1分钟 | 4.5分钟 |

| 5 | 等待部署 | 1-2分钟 | 6.5分钟 |

| 6 | 访问测试 | 30秒 | 7分钟 |

| **总计** | **从零到上线** | **约7分钟** | **7分钟** |

---

## 快速检查清单

上线前检查这些项目：

### 仓库设置

### Pages 配置

### 部署状态

### 文件检查

### 访问测试

---

## 示例：📕违禁词检测工具完整部署流程

GitHub Pages：https://vincentl666.github.io/xhs-content-checker

### 特别说明

如果你是要部署📕违禁词检测工具，完整步骤如下：

### 1. 创建仓库

- 仓库名：`xhs-content-checker`（或其他英文名）
- 类型：`Public`
- 勾选：Add a `README` file

### 2. 创建 `index.html`

- 点击 Add file → Create new file
- 文件名：`index.html`
- 粘贴完整的 HTML 代码（包含 React 和所有逻辑）
- 提交

### 3. 启用 Pages

- Settings → Pages
- Source: `Deploy from a branch`
- Branch: `main`
- Folder: `/(root)`
- 点击 Save

### 4. 等待部署

- 1-2 分钟后查看 Pages 页面
- 出现绿色提示框

### 5. 访问测试

- 访问：https://你的用户名.github.io/xhs-content-checker/
- 输入文案测试功能

### 完整代码（index.html）

使用我之前提供的包含以下内容的完整 HTML：

- React 库引入
- Babel 编译器
- Tailwind CSS
- 完整的违禁词检测组件
- SVG 图标
- 所有交互逻辑

---

## 备用方案：Gitee Pages（国内访问快）

如果 GitHub Pages 访问太慢，可以用 Gitee：

### Gitee Pages 优点

- 服务器在国内
- 访问速度快
- 操作类似 GitHub

### Gitee Pages 步骤

1. 注册 Gitee 账号：https://gitee.com
2. 创建仓库（步骤类似 GitHub）
3. 上传 `index.html`
4. 进入 服务 → Gitee Pages
5. 点击启动
6. 访问：https://你的用户名.gitee.io/仓库名/

### Gitee Pages 限制

- 免费版需要实名认证
- 更新后需要手动点击"更新"按钮
- 不支持自动部署

---

## 学习资源推荐

### 视频教程

- B站搜索："GitHub Pages 教程"
- B站搜索："如何部署网站"
- YouTube: "GitHub Pages Tutorial"

### 文档

- GitHub Pages 官方文档：https://docs.github.com/pages & https://docs.github.com/zh/pages
- GitHub 中文社区：https://www.github-zh.com

### 练习项目

1. 个人名片页面
2. 简历网站
3. 作品展示页
4. 博客首页
5. 工具类网站（如违禁词检测）

---

## 下一步学习方向

### 如果你想继续深入：

**前端基础**

- HTML5 完整教程
- CSS3 样式设计
- JavaScript 编程
- 响应式设计

**进阶技能**

- React / Vue 框架
- 前端工程化
- 性能优化
- SEO 优化

**部署相关**

- Git 版本控制
- 域名购买和配置
- CDN 加速
- 服务器部署

---

## 结语

**恭喜你完成了整个流程！**

现在你已经学会了：

1. ✅ 创建 GitHub 仓库
2. ✅ 上传和编辑文件
3. ✅ 配置 GitHub Pages
4. ✅ 部署网站上线
5. ✅ 更新和维护网站

**记住这份文档：**

- 保存到本地作为参考
- 或者在仓库创建 `GUIDE.md` 保存
- 下次操作直接查阅

**遇到问题：**

- 先查看本文档的"常见问题"部分
- 检查"快速检查清单"
- 在 GitHub 社区提问
- 搜索相关教程

---

## 附录：常用 HTML 模板

### 模板 1：最简单的测试页面

```
&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;zh-CN&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&gt;
    &lt;title&gt;测试页面&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1&gt;Hello World!&lt;/h1&gt;
    &lt;p&gt;我的第一个 GitHub Pages 网站&lt;/p&gt;
&lt;/body&gt;
&lt;/html&gt;
```

### 模板 2：带样式的个人主页



```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>个人主页</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Arial', sans-serif;
            line-height: 1.6;
            color: #333;
        }
        header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            text-align: center;
            padding: 4rem 2rem;
        }
        header h1 {
            font-size: 3rem;
            margin-bottom: 1rem;
        }
        header p {
            font-size: 1.2rem;
            opacity: 0.9;
        }
        .container {
            max-width: 1000px;
            margin: 0 auto;
            padding: 2rem;
        }
        .section {
            margin: 3rem 0;
        }
        .section h2 {
            font-size: 2rem;
            color: #667eea;
            margin-bottom: 1rem;
            border-bottom: 3px solid #667eea;
            padding-bottom: 0.5rem;
        }
        .card {
            background: #f8f9fa;
            padding: 1.5rem;
            margin: 1rem 0;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        .card h3 {
            color: #764ba2;
            margin-bottom: 0.5rem;
        }
        footer {
            background: #333;
            color: white;
            text-align: center;
            padding: 2rem;
            margin-top: 3rem;
        }
        a {
            color: #667eea;
            text-decoration: none;
        }
        a:hover {
            text-decoration: underline;
        }
    </style>
</head>
<body>
    <header>
        <h1>张三</h1>
        <p>前端开发工程师 | 设计爱好者</p>
    </header>

    <div class="container">
        <section class="section">
            <h2>关于我</h2>
            <div class="card">
                <p>你好！我是张三，一名热爱技术的前端开发者。</p>
                <p>我专注于创建美观、易用的网页应用。</p>
            </div>
        </section>

        <section class="section">
            <h2>技能</h2>
            <div class="card">
                <h3>前端技术</h3>
                <p>HTML5, CSS3, JavaScript, React, Vue</p>
            </div>
            <div class="card">
                <h3>设计工具</h3>
                <p>Figma, Photoshop, Illustrator</p>
            </div>
            <div class="card">
                <h3>其他</h3>
                <p>Git, GitHub, 响应式设计</p>
            </div>
        </section>

        <section class="section">
            <h2>项目展示</h2>
            <div class="card">
                <h3>项目一：个人博客</h3>
                <p>使用 HTML/CSS/JS 开发的响应式博客网站</p>
                <p><a href="#">查看项目 →</a></p>
            </div>
            <div class="card">
                <h3>项目二：待办事项应用</h3>
                <p>基于 React 的任务管理工具</p>
                <p><a href="#">查看项目 →</a></p>
            </div>
        </section>

        <section class="section">
            <h2>联系我</h2>
            <div class="card">
                <p>📧 Email: <a href="mailto:your@email.com">your@email.com</a></p>
                <p>🐙 GitHub: <a href="https://github.com/yourusername">@yourusername</a></p>
                <p>💼 LinkedIn: <a href="#">your-profile</a></p>
            </div>
        </section>
    </div>

    <footer>
        <p>© 2024 张三. All rights reserved.</p>
    </footer>
</body>
</html>
```

### 模板 3：作品集页面



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



```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>个人简历</title>
    <style>
        body {
            margin: 0;
            padding: 20px;
            font-family: 'Arial', sans-serif;
            background-color: #ecf0f1;
        }
        .resume {
            max-width: 900px;
            margin: 0 auto;
            background: white;
            box-shadow: 0 0 20px rgba(0,0,0,0.1);
        }
        .header {
            background: #34495e;
            color: white;
            padding: 3rem 2rem;
            text-align: center;
        }
        .header h1 {
            margin: 0;
            font-size: 2.5rem;
        }
        .header .title {
            font-size: 1.2rem;
            margin-top: 0.5rem;
            opacity: 0.9;
        }
        .contact {
            display: flex;
            justify-content: center;
            gap: 2rem;
            margin-top: 1rem;
            font-size: 0.9rem;
        }
        .content {
            padding: 2rem;
        }
        .section {
            margin-bottom: 2rem;
        }
        .section-title {
            font-size: 1.5rem;
            color: #34495e;
            border-bottom: 2px solid #3498db;
            padding-bottom: 0.5rem;
            margin-bottom: 1rem;
        }
        .experience-item {
            margin-bottom: 1.5rem;
        }
        .experience-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 0.5rem;
        }
        .company {
            font-weight: bold;
            color: #2c3e50;
        }
        .date {
            color: #7f8c8d;
            font-size: 0.9rem;
        }
        .position {
            color: #3498db;
            margin-bottom: 0.5rem;
        }
        ul {
            margin: 0.5rem 0;
            padding-left: 1.5rem;
        }
        li {
            margin: 0.3rem 0;
            color: #555;
        }
        .skills {
            display: flex;
            flex-wrap: wrap;
            gap: 0.5rem;
        }
        .skill {
            background: #3498db;
            color: white;
            padding: 0.5rem 1rem;
            border-radius: 5px;
        }
        .education-item {
            margin-bottom: 1rem;
        }
        .school {
            font-weight: bold;
            color: #2c3e50;
        }
        .degree {
            color: #555;
        }
    </style>
</head>
<body>
    <div class="resume">
        <div class="header">
            <h1>张三</h1>
            <div class="title">前端开发工程师</div>
            <div class="contact">
                <span>📧 zhangsan@email.com</span>
                <span>📱 138-0000-0000</span>
                <span>🔗 github.com/zhangsan</span>
            </div>
        </div>

        <div class="content">
            <div class="section">
                <h2 class="section-title">个人简介</h2>
                <p>
                    3年前端开发经验，熟练掌握 HTML、CSS、JavaScript 及主流前端框架。
                    有较强的学习能力和团队协作精神，追求代码质量和用户体验。
                </p>
            </div>

            <div class="section">
                <h2 class="section-title">工作经历</h2>

                <div class="experience-item">
                    <div class="experience-header">
                        <span class="company">某科技公司</span>
                        <span class="date">2021.07 - 至今</span>
                    </div>
                    <div class="position">前端开发工程师</div>
                    <ul>
                        <li>负责公司官网和管理后台的前端开发与维护</li>
                        <li>使用 React 和 Vue 开发多个核心业务模块</li>
                        <li>优化页面性能，提升加载速度 40%</li>
                        <li>参与前端技术选型和架构设计</li>
                    </ul>
                </div>

                <div class="experience-item">
                    <div class="experience-header">
                        <span class="company">某互联网公司</span>
                        <span class="date">2020.03 - 2021.06</span>
                    </div>
                    <div class="position">前端开发实习生</div>
                    <ul>
                        <li>协助完成移动端 H5 页面开发</li>
                        <li>修复线上 bug，优化用户体验</li>
                        <li>参与前端组件库的建设</li>
                    </ul>
                </div>
            </div>

            <div class="section">
                <h2 class="section-title">项目经验</h2>

                <div class="experience-item">
                    <div class="company">企业管理平台</div>
                    <ul>
                        <li>技术栈：React + TypeScript + Ant Design</li>
                        <li>实现了用户管理、权限控制、数据统计等核心功能</li>
                        <li>采用 React Hooks 和状态管理优化代码结构</li>
                    </ul>
                </div>

                <div class="experience-item">
                    <div class="company">电商小程序</div>
                    <ul>
                        <li>技术栈：Vue + Vant + 小程序原生 API</li>
                        <li>开发商品列表、购物车、订单管理等功能</li>
                        <li>实现了商品搜索和筛选功能</li>
                    </ul>
                </div>
            </div>

            <div class="section">
                <h2 class="section-title">技能清单</h2>
                <div class="skills">
                    <span class="skill">HTML5</span>
                    <span class="skill">CSS3</span>
                    <span class="skill">JavaScript</span>
                    <span class="skill">TypeScript</span>
                    <span class="skill">React</span>
                    <span class="skill">Vue</span>
                    <span class="skill">Webpack</span>
                    <span class="skill">Git</span>
                    <span class="skill">响应式设计</span>
                    <span class="skill">性能优化</span>
                </div>
            </div>

            <div class="section">
                <h2 class="section-title">教育背景</h2>
                <div class="education-item">
                    <div class="experience-header">
                        <span class="school">某某大学</span>
                        <span class="date">2016.09 - 2020.06</span>
                    </div>
                    <div class="degree">计算机科学与技术 · 本科</div>
                </div>
            </div>
        </div>
    </div>
</body>
</html>
```

---

## 最后的提醒

### 🎯 记住这三个关键点：

1. **仓库必须是 Public**

1. 文件必须叫 `index.html`

1. **等待部署完成再访问**

### 📝 保存这份文档：

建议你：

1. 复制到记事本保存为 `GitHub_Pages_完整教程.txt`

1. 或在仓库创建 `GUIDE.md` 文件保存

1. 需要时随时查阅

### 🚀 开始你的创作之旅！

现在你已经掌握了所有必要的知识，可以：



+ 部署个人主页
+ 发布作品集
+ 搭建项目展示站
+ 创建实用工具（如违禁词检测）

祝你成功！有问题随时查阅这份文档！🎉
