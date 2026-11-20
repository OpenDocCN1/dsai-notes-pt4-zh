
这个文件实现的功能主要是，通过 `Replicate` API 获取特定图片模型生成的结果

```
import { NextResponse } from "next/server";
import Replicate from "replicate";
 
const replicate = new Replicate({
  auth: process.env.REPLICATE_API_TOKEN,
});
 
export async function GET(request, context) {
  const { id } = await context.params;
  const prediction = await replicate.predictions.get(id);
 
  if (prediction?.error) {
    return NextResponse.json({ detail: prediction.error }, { status: 500 });
  }
 
  return NextResponse.json(prediction);
}
```

在第 2）和第 4）中，我们分别建了一个文件，这两个文件共同实现了一个完整的图像模型生成的 API：

+ 第一个文件 `POST` 请求用于创建新的图像生成，接收用户输入并返回预测结果。

+ 第二个文件 `GET` 请求用于根据图像生成 `ID` 获取已创建的图像生成结果，支持错误处理。

通过这两个接口，用户可以方便地提交图像生成请求并查询图像生成结果。

#### 4.2.5 构建前端页面代码

在上面一步中，我们完成了与 `Replicate` 图像生成通信的服务器端代码编写，现在我们在前端页面创建一个表单，当用户输入提示词并且提交表单时，前端会把数据发送到我们在上一步中写好的服务器端代码，然后服务器端代码调用 `replicate` 图像模型给我们生成图像

##### 1）编写 `page.js` 文件

我们项目目录下，有一个 `page.js` 文件，这个文件在项目刚初始化的时候，就被自动创建了，他默认渲染的是“欢迎来到 `Next.js`” 页面

![](img/320538772dee5bcf1eec73757fd7631f.png)

现在我们把 `page.js` 里面的代码全部删掉，然后粘贴如下代码

这个 js 文件会帮我渲染一个前端页面

+ 显示一个标题和一个输入框，用户可以在输入框输入提示词。

+ 提交按钮用于触发图像生成。

+ 如果有错误，显示错误信息。

+ 如果有图像生成结果，显示生成的图像和当前状态。

```
'use client';

import { useState } from "react";
import Image from "next/image";

const sleep = (ms) => new Promise((r) => setTimeout(r, ms));

export default function Home() {
  const [prediction, setPrediction] = useState(null);
  const [error, setError] = useState(null);

  const handleSubmit = async (e) => {
    e.preventDefault();
    const response = await fetch("/api/predictions", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        prompt: e.target.prompt.value,
      }),
    });
    let prediction = await response.json();
    if (response.status !== 201) {
      setError(prediction.detail);
      return;
    }
    setPrediction(prediction);

    while (
      prediction.status !== "succeeded" &&
      prediction.status !== "failed"
    ) {
      await sleep(1000);
      const response = await fetch("/api/predictions/" + prediction.id);
      prediction = await response.json();
      if (response.status !== 200) {
        setError(prediction.detail);
        return;
      }
      console.log({ prediction: prediction });
      setPrediction(prediction);
    }
  };

  return (
    <div className="container max-w-2xl mx-auto p-5">
      <h1 className="py-6 text-center font-bold text-2xl">
        Dream something with{" "}
        <a href="https://replicate.com/black-forest-labs/flux-schnell?utm_source=project&utm_project=getting-started">
          Flux Schnell
        </a>
      </h1>

      <form className="w-full flex" onSubmit={handleSubmit}>
        <input
          type="text"
          className="flex-grow"
          name="prompt"
          placeholder="Enter a prompt to display an image"
        />
        <button className="button" type="submit">
          Go!
        </button>
      </form>

      {error && <div>{error}</div>}

      {prediction && (
        <>
          {prediction.output && (
            <div className="image-wrapper mt-5">
              <Image
                src={prediction.output[prediction.output.length - 1]}
                alt="output"
                sizes="100vw"
                height={768}
                width={768}
              />
            </div>
          )}
          <p className="py-3 text-sm opacity-50">status: {prediction.status}</p>
        </>
      )}
    </div>
  );
}
```

总的来说，这个文件实现了一个简单的用户界面，允许用户在浏览器页面输入文本提示词，然后这个前端页面会把请求发送到后台服务器代码，然后调用 `replicate` 图像模型生成图像

#### 4.2.6 添加基本的样式

我们把项目初始化时生成的 `globals.css` 里面的样式全部删掉

![](img/d43c4d3061285270963f7c5a94b05289.png)

然后粘贴我们的基本样式：

这一步就是给前端页面添加一点基本的样式

```
@tailwind base;
@tailwind components;
@tailwind utilities;

.container {
  padding: 2rem;
  font-size: 1.3rem;
  max-width: 48rem;
  margin: 0 auto;
}

form {
  display: flex;
  margin-bottom: 2rem;
}

form input {
  width: 100%;
  padding: 1rem;
  border: 1px solid #000;
  border-radius: 0.25rem;
  font-size: 1.3rem;
  margin-right: 1rem;
}

form button {
  padding: 1rem;
  border: none;
  border-radius: 0.25rem;
  box-sizing: border-box;
  cursor: pointer;
  font-size: 1.3rem;
}

.imageWrapper {
  width: 100%;
  aspect-ratio: 1 / 1;
  position: relative;
}
```

#### 4.2.7 配置图片 `host`

因为我们在自己项目里面调用的是 `replicate` 图像模型 `API` ，我们不确定这个第三方 `API` 是否存在风险。配置图片 `host` 可以做到一下两点

+ 安全防御：防止攻击者通过图片 `URL` 注入恶意内容（比如加载钓鱼网站图片或窃取用户数据）

+ 资源控制：避免应用随意加载未经审核的外部图片资源

在 `next.config.mjs` 文件 中添加如下代码

```
const nextConfig = {
  reactStrictMode: true,
  images: {
    remotePatterns: [
      {
        protocol: "https",
        hostname: "replicate.com",
      },
      {
        protocol: "https",
        hostname: "replicate.delivery",
      },
    ],
  },
};

export default nextConfig;
```

![](img/c18bcdd8bae6d622131ed071b617f5e9.png)

配置内容解析：

```
// next.config.js
const nextConfig = {
  reactStrictMode: true, // 与图片配置无关，是 React 严格模式开关
  images: {
    remotePatterns: [ // 远程图片规则配置
      {
        protocol: "https",  // 强制使用 HTTPS 协议
        hostname: "replicate.com", // 允许的域名1
      },
      {
        protocol: "https",
        hostname: "replicate.delivery", // 允许的域名2
      },
    ],
  },
};
```

#### 4.2.8 启动项目，测试图片生成功能

##### 1）启动项目

在 `cursor` 控制台输入 `npm run dev` 启动项目

![](img/871e34aebdd49874aeb94dabd76ffc56.png)

成功启动之后，访问 `http://localhost:3000` 就可以看到我们的页面了

![](img/ede6bd6824a33e9d1020137f8737725c.png)

输入框输入提示词，输入完成之后点击 `Go` 生成图片。`Flux Schnell` 是一个超链接，链接到的是我们调用的图像生成模型

##### 2）报错解决

我们提示词输入一个 `woman` 然后点击 `Go` ，结果页面报错了

![](img/1748a35132ed8477f100eb06ce99ba98.png)

![](img/ff52d88a91bfbaa48b297bd1d6c4a2e3.png)

这个时候我们直接回 `cursor` 看控制台，选中错误信息，把错误交给 `cursor` 解决即可

我们按照 `cursor` 提示，安装 `replicate` 包，然后重启项目

![](img/b08b38d32232475971414584e25a06e5.png)

重新打开页面，`http://localhost:3000` 输入提示词 `woman` ，发现已经能够正常生成了

![](img/8c835584d7f336583e4db00538ccc671.png)

到这里我们已经完成了一个前端页面搭建，用户可以编写提示词，然后提交之后由后端服务器调用 `replicate` `API` 生成图像的整个过程。

### 4.3 本地代码提交 `github`

目前我们开发的代码都在本地，因为我们最终想把这个网站部署到 `vercel` 上，所以我们需要把本地的代码提交到 `github` 进行托管

#### 1）`github` 上新建一个仓库

直接建一个空仓库就行

![](img/4120d176dfeb44d4df3aa679ea4f9a4f.png)

#### 2）关联本地仓库与 `github` 远程仓库

```
git remote add origin 你的GitHub仓库地址.git
```

仓库地址在这里：

![](img/1a0d74726d77aebba6dc181b4d559254.png)

#### 3）提交本地代码并且推送远程仓库

```
# 添加所有文件到暂存区（或指定文件）
git add .

# 提交更改到本地仓库
git commit -m "Initial commit"

# 推送到 GitHub（首次推送需设置上游分支）
git push -u origin main
```

上面命令执行完之后，`github` 远程仓库就会出现我们本地代码了

![](img/fa1b70aff3d7a0efd1924144ca93318c.png)

### 4.4 网站部署到 `vercel`

#### 1）登录 `vercel` 之后，选择 `Add New Project`

![](img/5191cc98bdf8ba568037496f8df85971.png)

#### 2) 从 `github` 仓库导入你要部署的项目

![](img/74ea767dfcee9a018171b78b4c3803a6.png)

#### 3）`deploy` 发布，等待部署完成

![](img/86258c1b4690013cfb89ca44d79325be.png)

#### 4）部署成功页面

![](img/5378f558e6c747fffc27e1e6ae09dfdb.png)

#### 5）打开域名验证

部署成功之后，`vercel` 会给你分配一个临时域名，我们打开测试一下

输入提示词 `woman` 然后点击 `Go` 生成

![](img/11d0f14cec6ab85ffd9fca95c6e089d2.png)

我们发现没有任何反应，接下来我们去看调用日志排查问题

#### 6）看运行日志，排查问题

![](img/4450939765dcac3b020dd226a82bd0de.png)

进入之后，如果没有看到报错

![](img/51aec9fe406ccb97ff490660a8eb3124.png)

把右下角的这个勾去掉，然后再看

![](img/daff48a34c137f999684ce6c304eb4db.png)

![](img/c5502f43bc55f38d234b90c12b68b169.png)

通过上面的错误日志，我们知道是因为部署到 `vercel` 没有配置 `replicate` 平台的 `API token` ，所以没办法调用平台的图像模型去生成图像

现在我们去把 `API TOKEN` 配置到 `vercel` 线上环境

#### 7）解决报错问题

![](img/140d6804c2427c46f48c97e5c09546e0.png)

我们之前在本地使用的 `token` 同样要配置到 `vercel` 才能生效，你可以去本地的环境变量文件，获取这个值，或者你也可以去 `replicate` 平台去获取 `api token`
