
# 数字人形象克隆工作流设计

## 节点参数配置

`material_video`：引用【`upload_file_to_oss`】节点的`oss_file_url`

`name`：引用【开始节点】节点的`name`

`train_type`：设置为`figure`

![](img/5cfb8a17a9492b47987cf0bd659dab25.png)

`create_customised_person`是创建数字人形象的插件，`code`为`0`表示请求成功，如果不为`0`那就是出错了。

## 工作流详细流程

### 10、添加选择器，判断`create_customised_person`的`code`等于`0`

![](img/c62d2adb180226e72a92db64ea475cf2.png)

如果分支连接到下一个【循环】节点，否则分支连接到【创建克隆数字人错误】节点，【创建克隆数字人错误】文本处理节点的内容引用`create_customised_person`节点的`msg`：

![](img/cadf82d7d91f3fa04bf8729284c0180b.png)

### 11、添加循环节点

#### 循环设置

循环类型：无限循环

#### 中间变量设置

`id`：`string`类型，设置为`""`

`pic_url`：`string`类型，设置为`""`

`preview_url`：`string`类型，设置为`""`

`audio_man_id`：`string`类型，设置为`""`

`height`：`int`类型，设置为`1`

`width`：`int`类型，设置为`1`

`name`：`string`类型，设置为`""`

`msg`：`string`类型，设置为`""`

#### 输出设置

`id`：引用循环的`id`

`pic_url`：引用循环的`pic_url`

`preview_url`：引用循环的`preview_url`

`audio_man_id`：引用循环的`audio_man_id`

`height`：引用循环的`height`

`width`：引用循环的`width`

`name`：引用循环的`name`

`msg`：引用循环的`msg`

![](img/3ed0d4fda6bd7f9c9a35ec7ca2f9ef44.png)

### 11.1、循环体内容设置，添加【蝉镜数字人`API`】插件的【`get_customised_person`】

#### 输入设置

`access_token`：引用【`getAccessToken`】节点的`access_token`

`id`：引用【`create_customised_person`】节点的`data`

![](img/c6ee77ef0bb5a1bb97b2d602578836c7.png)

`get_customised_person`是获取数字人形象克隆结果的插件，该插件的`process`表示数字人形象克隆的进度，`100`表示完成，还有个`status`表示状态（当前状态：1制作中，2成功，4失败，5系统错误），如果要严谨些是要加上`status`状态判断的。

### 11.2、循环体内容设置，添加【蝉镜数字人`API`】插件的【`list_customised_person`】（Tips：该节点可以不用设置）

#### 输入设置

`access_token`：引用【`getAccessToken`】节点的`access_token`

`page`：设置为`1`

`page_size`：设置为`20`

![](img/cd9bf3c6a2e485675ee76f5306fd77e0.png)

`list_customised_person`是获取定制数字人形象的插件，可以不用添加的，因为之前忘记删了。

### 11.3、添加选择器，判断【`get_customised_person`】节点的`process`等于`100`

![](img/7d92b2b238b89d525b8bd492dc03c243.png)

如果分支连接到【设置变量】节点，否则分支连接到【代码(停顿)】节点。

### 11.4、添加【设置变量】节点

![](img/b76ed4a6e9f37d06d5ce3804d1fde384.png)

设置对应设置即可，弄完后链接到【终止循环】节点。

### 11.5、添加【代码(停顿)】节点

#### 输入设置

`sleep_time`：设置为`5`，也可以设置更长些，添加停顿时间是为了避免循环过快超出工作流的节点运行次数。

#### 输出设置

`sleep_time`：`string`类型

代码：`Python`代码，不是`JavaScript`代码，需要通过左上角切换为`Python`。

```
import time

async def main(args: Args) -> Output:
    params = args.params
    sleep_time = params["sleep_time"]

    time.sleep(sleep_time)
    # 构建输出对象
    ret: Output = {
        "sleep_time": sleep_time
    }
    return ret
```

![](img/2d54d36c929f5f7acd16cd2ce5dd26d8.png)

### 12、添加【变量聚合】节点

输入设置：

设置分组名称为`info`，设置为：

- 获取`AccessToken`失败-output
- 创建克隆数字人错误-output
- 视频地址提取错误-output

![](img/0b61a222e74513eac9f563823277f614.png)

### 13、【结束】节点设置

#### 输出变量设置

`id`：引用【循环】节点的`id`

`pic_url`：引用【循环】节点的`pic_url`

`preview_url`：引用【循环】节点的`preview_url`

`audio_man_id`：引用【循环】节点的`audio_man_id`

`height`：引用【循环】节点的`height`

`width`：引用【循环】节点的`width`

`name`：引用【循环】节点的`name`

`msg`：引用【循环】节点的`msg`

`info`：引用【变量聚合】节点的`info`

![](img/3eea17113d4478fffeb2430223825411.png)

需要把【结束】节点设置为返回变量。

### 14、发布工作流

![](img/d30515a99b982b4859e9b153e9348aa8.png)

点击【坚持发布】发布工作流。

### 15、获取工作流id

![](img/909cd6c776d60aefab31ed24d0832eb4.png)

工作流`id`后面多维表格会用到。

## 声音克隆工作流设计

### 工作流整体流程

![](img/9c0243df84e2b48828f67ade1babbf97.png)

### 工作流详细流程

#### 1、创建一个工作流

创建一个工作流，点击【工作空间➡️资源库➡️+资源➡️工作流】，设置工作流名称和描述为“`create_customised_audio”：

![](img/a3e201ca6d111e6930bd3fbd0ff0b62b.png)

![](img/8849ff7fa3269872765d5f5007b93c4f.png)

#### 2、开始节点设置2个变量`audio_url`、`name`，都设置为`string`类型

![](img/e175027e80fc518a37748afcfe330e16.png)

#### 3、添加个信息提取的插件`info_extract`

在插件商店搜索“工具集合”或者先通过链接获取到插件“[工具集合](https://www.coze.cn/store/plugin/7481310340161159208)”：

![](img/1724448f58aea853885fc13d32588bdb.png)

![](img/8918650cbdea86b6d9682166756c991c.png)

输入值设置：

`content`：引用【开始】节点的【`audio_url`】

`raw_str`：

```
(https://.*?.feishu.cn/space/api/box/stream/download/authcode/\?code=.*?)"
```

![](img/6beab9f1129fa7d4fc77c001b2725c62.png)

#### 4、添加选择器，判断`info_extract`的`info_first`不为空

![](img/9ef9abed532986cb87d7e03dc24d2f51.png)

如果分支连接到下一个【`getAccessToken`】节点，否则分支连接到【音频地址提取错误】节点，【音频地址提取错误】文本处理节点的内容引用`info_extract`节点的`msg`：

![](img/d7637f23ce0f5d5ca304d0c51dec0645.png)

#### 5、添加【[蝉镜数字人`API`](https://www.coze.cn/store/plugin/7476699667410042916)】插件的【`getAccessToken**】**，[`app_id`](https://www.chanjing.cc/user-center)和`secret_key`在蝉镜官网的个人资料获取：

![](img/9ae36bd55df888480a2ea8579b2f89d2.png)

#### 输入值设置

`app_id`：蝉镜官网的`app_id`

`secret_key`：蝉镜官网的`secret_key`

![](img/88ae46b9ebc543bfd388a15d95f863b8.png)

#### 6、添加选择器，判断`getAccessToken`的`data`不为空

![](img/9abcee4309423d299432581cea53b49a.png)

如果分支连接到下一个【设置音频名称】节点，否则分支连接到【获取`AccessToken`失败】节点，【获取`AccessToken`失败】文本处理节点的内容引用`getAccessToken`节点的`msg`：

![](img/27b8e9a0908dfb4632814c04463c3398.png)

#### 7、添加【设置音频名称】文本处理节点

#### 输入值设置

`String1`：引用【开始】节点的`name`

字符串拼接设置为：

```
{{String1}}.mp3
```

![](img/4a720fb57a3daa33a596b1dae9d4e78c.png)

#### 8、添加【[蝉镜数字人`API`](https://www.coze.cn/store/plugin/7476699667410042916)】插件的【`upload_file_to_oss`】

`access_key_id`、`access_key_secret`、`bucket`、`endpoint`需要开通阿里云OSS获取：

#### 输入值设置

`access_key_id`：阿里云OSS `access_key_id`

`access_key_secret`：阿里云OSS `access_key_secret`

`bucket`：阿里云OSS `bucket`

`endpoint`：阿里云OSS `endpoint`

`file_url`：引用`info_extract`节点的`info_first`

`expire_hours`：链接有效时间，默认`24`，可以不设置

`file_name`：引用【设置音频名称】文本处理节点的`output`

![](img/2579d956d510fa6dd26f79d2404ad926.png)

#### 9、添加【[蝉镜数字人`API`](https://www.coze.cn/store/plugin/7476699667410042916)】插件的【`create_customised_audio`】

#### 输入值设置

`access_token`：引用【`getAccessToken`】节点的`access_token`

`url`：引用【`upload_file_to_oss`】节点的`oss_file_url`

`audio_name`：引用【开始节点】节点的`name`

![](img/4a0a3ea3cf1b6f64afceb4a7a567d4ec.png)

#### 10、添加选择器，判断`create_customised_audio`的`code`等于`0`

![](img/1fdf1e4c1c08d183ff15f76d81042083.png)

如果分支连接到下一个【循环】节点，否则分支连接到【创建克隆音频错误】节点，【创建克隆音频错误】文本处理节点的内容引用`create_customised_audio`节点的`msg`：

![](img/9141ab165bb13f614610525d06811090.png)

#### 11、添加循环节点

##### 循环设置

循环类型：无限循环

##### 中间变量设置

`id`：`string`类型，设置为`""`

`name`：`string`类型，设置为`""`

`audio_path`：`string`类型，设置为`""`

`err_msg`：`string`类型，设置为`""`

`progress`：`int`类型，设置为`1`

##### 输出设置

`id`：引用循环的`id`

`name`：引用循环的`name`

`audio_path`：引用循环的`audio_path`

`err_msg`：引用循环的`err_msg`

`progress`：引用循环的`progress`

![](img/b195244aff5ac9d210e5bf0634e39eaf.png)

#### 11.1、循环体内容设置，添加【蝉镜数字人`API`】插件的【`get_customised_audio`】

##### 输入设置

`access_token`：引用【`getAccessToken`】节点的`access_token`

`id`：引用【`create_customised_audio`】节点的`data`

![](img/1f212d5d69506669310f2b2e86ba4528.png)

#### 11.2、循环体内容设置，添加【蝉镜数字人`API`】插件的【`list_customised_audio`】（Tips：该节点可以不用设置）

##### 输入设置

`access_token`：引用【`getAccessToken`】节点的`access_token`

`page`：设置为`1`，可不设置

`page_size`：设置为`20`，可不设置

![](img/50dbfa358120a8f2e35dd6f104335d6d.png)

#### 11.3、添加选择器，如果判断【`get_customised_audio`】节点的`process`等于`100`，否则如果判断【`get_customised_audio`】节点的`err_msg`不为空

![](img/cdf8d0f02258121cb8b302cf0e448a27.png)

如果、否则如果两个分支连接到【设置变量】节点，否则分支连接到【代码(停顿)】节点。

#### 11.4、添加【设置变量】节点

![](img/7854ecaa8c846043378a87c35320bfe6.png)

设置对应设置即可，弄完后链接到【终止循环】节点。

#### 11.5、添加【代码(停顿)】节点

##### 输入设置

`sleep_time`：设置为`5`，也可以设置更长些，添加停顿时间是为了避免循环过快超出工作流的节点运行次数。

##### 输出设置

`sleep_time`：`string`类型

代码：`Python`代码，不是`JavaScript`代码，需要通过左上角切换为`Python`。

```
import time

async def main(args: Args) -> Output:
    params = args.params
    sleep_time = params["sleep_time"]

    time.sleep(sleep_time)
    # 构建输出对象
    ret: Output = {
        "sleep_time": sleep_time
    }
    return ret
```

![](img/0ac78cd53b376a1e379895b9fafcd721.png)

#### 12、添加【变量聚合】节点

输入设置：

设置分组名称为`info`，设置为：

- 获取`AccessToken`失败-output
- 创建克隆音频错误-output
- 音频地址提取错误-output

![](img/a96244eb402da1b001c049e29b40fb02.png)

#### 13、【结束】节点设置

##### 输出变量设置

`id`：引用【循环】节点的`id`

`name`：引用【循环】节点的`name`

`audio_path`：引用【循环】节点的`audio_path`

`err_msg`：引用【循环】节点的`err_msg`

`progress`：引用【循环】节点的`progress`

`info`：引用【变量聚合】节点的`info`

![](img/30a43b62157a755ffe3fca0fa915a824.png)

需要把【结束】节点设置为返回变量。

#### 14、发布工作流

![](img/9134a9c753e06c778728876afbc9dce5.png)

点击【坚持发布】发布工作流。

#### 15、获取工作流id

![](img/64c3e86ff98e419102cde36de8600365.png)

工作流`id`后面多维表格会用到。

## 视频生成工作流搭建过程

### 工作流整体流程

![](img/eead2cf687e2075880f4358c41b836de.png)

### 工作流详细流程

#### 1、创建一个工作流

创建一个工作流，点击【工作空间➡️资源库➡️+资源➡️工作流】，设置工作流名称和描述为“`chanjing_video_generate”：

![](img/ad4d34e5c4a4f5410d66c7aebe19d314.png)

![](img/57c646800e9baf2f2cfad0bdbe38d8ef.png)
