
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
