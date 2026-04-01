# 多模态整理学习文档及自动化录入知识库

> 来源：[https://datawhaler.feishu.cn/docx/RRbGd2GapoDbR0xvbgDcQdvhnvh](https://datawhaler.feishu.cn/docx/RRbGd2GapoDbR0xvbgDcQdvhnvh)

# 一、需求

+   基于[扣子](https://www.coze.cn/)多模态识别和python脚本，自动化整理网上英文PDF学习文档，且数据自动化录入到飞书知识库，便于后续学习。

+   使用**多模态大模型**主要是解决的是，针对复杂的PDF内容布局，日常需要人工一条一条复制或OCR识别，然后录入到知识库，此流程过于繁琐机械化，且效果不理想。

# 二、效果

## 1. 要提取的PDF文档部分内容

如果用人工一条一条复制或者用**OCR识别**，然后存到知识库，流程繁琐机械化，且效果不理想。

![](img/721cc892f3bddc0be30289f99b8ab62f.png)

## 2. 使用python脚本进行根据页面切分转化为图像

![](img/f8d808babf178722e0f8db305861c2d4.png)

## 3. 批量上传图片到工作流，进行多模态识别及传到多维表格

批量上传图片和知识库多维表格链接效果，如下：

![](img/fea4c41274a942a73224a2fbdb5ceb4c.png)

**多维表格**效果，如下：

![](img/76047f40ee89e0c0e933007ff392e818.png)

# 三、实现思路

1.  由于扣子工作流直接读取上传文件内容不佳，所以先使用python脚本将PDF文件按页码自动化切分为图片集。

1.  到飞书创建多维表格，并自定义字段名称及类型，然后修改分享权限为互联网获得链接的人可编辑。

1.  在扣子资源库创建个提取图片的中中英文内容的工作流。

(1)循环使用多模态豆包·视觉理解·Pro识别图片并提取图片中英文文本。

(2)使用代码组件，将大模型识别结果文本提取出来，并转化为能上传到飞书多维表格插件的请求格式。

(3)使用扣子官方的飞书多维表格插件，将数据自动录入到2步骤先前飞书所创建的多维表格中。

故，工作流已经搭建完成，并发布。

1.  在扣子项目开发中创建智能体，填写人设与回复逻辑、工作流，然后发布。

1.  将PDF的图片集直接上传到扣子平台，然后自动整理到飞书知识库。

# 四、实现

## 1. 新建飞书多维表格

(1) 登录飞书客户端，新建多维表格。

![](img/f7e47e3e59b8172a198a9819737db74f.png)

（2）分别新建三个文本类型字段：No、English、Chinese。

![](img/9329e35a762437f68c83d91bb2630530.png)

(3)设置分享权限为互联网获得链接的人。

![](img/983c336b64a1e38d56a5cb6afb57c3ee.png)

![](img/845d09b1ad328bc1f16ce3d2708067ea.png)

> **该链接就是后续****扣子工作流的飞书多维表格插件****的URL**

## 2. 搭建工作流

### 工作流整体总览

到新增插件节点上搜索飞书多维表格，然后找到add_records。

![](img/07592a23c224428a747e4334b8925df6.png)

#### 1. 开始

新建变量：

+   input：Array<Image>

+   feishu_url：String

![](img/631ce67d69d40d8106aac621689e98ee.png)

#### 2. 循环

+   循环类型：使用数组循环

+   参数：

+   input：开始-input Array<Image>

![](img/90faa0a33b549ed55b1c986210ff9119.png)

+   中间变量：

+   variable：开始-input Array<Image>

![](img/58a89d470dec90f9dfdf7cc6ac126f51.png)

+   输出：

+   output：

![](img/6d804bcd44c108431d249abda2368b17.png)

> 至此，工作流已经搭好了。最后发布。

![](img/5949724aceaacc678cab131c0195af0e.png)

#### 3. 循环体

##### 3.1.  循环体-大模型

+   大模型类型：豆包·视觉理解·Pro

+   输入：

+   input：item

+   系统提示词：

```
你是高级版面ocr助手，帮我提取图片里的文本，并保持原来图片上的版面布局，然后你输出的内容变成markdown的json对象数组。
格式示例如下：
[
 { &quot;No&quot;: 61, &quot;English&quot;: &quot;Is it yours?&quot;, &quot;Chinese&quot;: &quot;这是你的吗?&quot; }
],并且对象中的&quot;English&quot;只要英文，不要有中文内容， &quot;Chinese&quot;只要中文内容。
```

+   用户提示词：

```
图片如下：{{input}}
```

+   输出：

+   output：String

![](img/bfbdb8b897821c41755bffcd6f9e6f65.png)

![](img/80aeef987cde130fec55367d0c3123b0.png)

![](img/60ac4bdf9664d22210a53d5206960d9b.png)

##### 3.2.  循环体-代码

+   输入：

+   input：

![](img/efae191f38d1509058e4c8625253637c.png)

+   python代码：

```
async def main(args: Args) -&gt; Output:
    import json
    params = args.params
    data_str = params[&#x27;input&#x27;]
    record_obj_list = json.loads(data_str)
    records = []
    # # 遍历 list 数据，生成每个记录
    for item in record_obj_list:
        record = {&quot;fields&quot;: &quot;{}&quot;}  # 初始化 record
        fields = {}
        fields[&quot;No&quot;] = str(item[&quot;No&quot;])
        fields[&quot;English&quot;] = item[&quot;English&quot;]
        fields[&quot;Chinese&quot;] = item[&quot;Chinese&quot;]
    #     # 将 fields 转换为 JSON 字符串
        record[&quot;fields&quot;] = json.dumps(fields, ensure_ascii=False)
        
    #     # 将 record 添加到 records 列表
        records.append(record)

    # 返回包含 records 的对象
    ret: Output = {
        &quot;key0&quot;: records
        # &quot;key0&quot;: python_obj
    }
    return ret
```

+   输出：

+   key0：Array<Object>

##### 3.3.  循环体-add_records

到新增插件节点上搜索飞书多维表格，然后找到add_records。

![](img/b40a3e4ee87811f19f07025dfccbb083.png)

![](img/8d6fa5349fa257a076bce73e2b6f3f5c.png)

![](img/89a697d44cae7c0ca0cec910d171bb04.png)

![](img/92dffb7efcbbd68a0d55ce0abf9e410d.png)

#### 4. 结束

![](img/383a553a75dea023c7e5675690bf2197.png)

## 3. 创建智能体

![](img/c6307f11d78c9d97d4d064edb3290da5.png)

![](img/90596c3c443fd3770553da97cda1e721.png)

> 多维表格URL在**飞书的多维表格的分享**复制链接。

![](img/4827215c6cbefef3887d85e71bc1d6b2.png)

+   人设逻辑文本：

```
你只需执行工作流{工作流}就可以了，执行完工作流{工作流}只需回复&quot;已执行完成！&quot;。不要有其他的内容回复。
```

## 4. PDF文件按照页码自动切分转化为图片

源代码链接：

[https://gitee.com/heididi/c](https://gitee.com/heididi/cutting_pdf_to_images)utting_pdf_to_images

### (1) 克隆项目

```
git clone https://gitee.com/heididi/cutting_pdf_to_images.git
```

### (2) 安装依赖

```
pip install -r requirements.txt
```

此外，pdf2image 依赖于 poppler-utils，这需要在系统中安装 Poppler。

+   Windows：可以从Poppler for Windows下载并安装。

链接：<https://github.com/oschwartz10612/poppler-windows/releases/>

操作步骤：

1. 下载 poppler-xx.xx.xx-x86_64.zip 文件。

2. 解压文件。

3. 将解压后的文件夹的bin目录添加到系统的环境变量path中。

4. 记得重启电脑。

5. 验证安装是否成功。在命令行中输入pdfinfo，如果输出了帮助信息，则安装成功。

示例：

![](img/55e16ed2459a91eb395e325cc5edfd18.png)

将bin目录添加到环境变量中：

![](img/5d62105e0872b9d0e584c59bd597ac88.png)

+   macOS：可以使用Homebrew安装。

```
bash brew install poppler  
```

+   Linux：可以使用包管理器安装。

```
bash  sudo apt-get install poppler-utils  
```

### (3) 运行程序

1.  修改main.py中的pdf_path和image_path为自己的路径。

1.  然后运行命令：

```
python main.py
```

程序会将pdf文件夹中的所有pdf文件按照页码自动切分转化为图片，并保存到images文件夹中。

### (4) 注意事项

+   程序会将pdf文件夹中的所有pdf文件按照页码自动切分转化为图片，并保存到images文件夹中。

### 示例

（1）运行前文件目录，将要分割的pdf文件放在pdf文件夹中。

![](img/48c20f7fa0dc6d9ba7a5cf14544f425f.png)

（2）并编辑main.py中的pdf_path和image_path为自己的路径。

![](img/47c1402e8c0ac15587ea73d31cfb2d15.png)

（3）运行命令后文件目录：

![](img/3f279b0e32fcb101cf923e0aa85918dd.png)

现在，就已经完成将pdf文件按页数切割成图片了。