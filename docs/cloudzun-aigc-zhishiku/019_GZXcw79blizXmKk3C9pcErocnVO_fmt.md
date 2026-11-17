
# AI-102: Designing and Implementing a Microsoft Azure AI Solution

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/FeJGdoxY5oTbWfxhEhLcFYCFnx8](https://zsyhjtnsa5.feishu.cn/docx/FeJGdoxY5oTbWfxhEhLcFYCFnx8)

## Azure AI 概览

## 在线学习资料

### Azure AI 服务入门

Azure AI 服务是可集成到应用程序中的 AI 功能的构建基块。在此学习路径中，你将了解如何预配、保护、监视和部署 Azure AI 服务资源，并使用它们来生成智能解决方案。

#### 练习1：使用 Azure AI 服务

`https://microsoftlearning.github.io/mslearn-ai-services/Instructions/Exercises/01-use-azure-ai-services.html`

在这个练习中，您将通过在Azure订阅中创建一个Azure AI服务资源并从客户端应用程序中使用它来开始使用Azure AI服务。本练习的目标不是获得任何特定服务的专业知识，而是熟悉作为开发人员进行Azure AI服务配置和工作的一般模式。

代码解析

`Labfiles/01-use-azure-ai-services/C-Sharp/rest-client/Program.cs`

```
using System;
using Newtonsoft.Json.Linq;
using System.Text;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Web;
using Microsoft.Extensions.Configuration;
using System.Threading.Tasks;

namespace rest_client
{
    class Program
    {
        private static string AiSvcEndpoint;
        private static string AiSvCKey;
        static async Task Main(string[] args)
        {
            try
            {
                // Get config settings from AppSettings
                IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile(&quot;appsettings.json&quot;);
                IConfigurationRoot configuration = builder.Build();
                AiSvcEndpoint = configuration[&quot;AIServicesEndpoint&quot;];
                AiSvCKey = configuration[&quot;AIServicesKey&quot;];


                // Get user input (until they enter &quot;quit&quot;)
                string userText = &quot;&quot;;
                while (userText.ToLower() != &quot;quit&quot;)
                {
                    Console.WriteLine(&quot;Enter some text (&#x27;quit&#x27; to stop)&quot;);
                    userText = Console.ReadLine();
                    if (userText.ToLower() != &quot;quit&quot;)
                    {
                        // Call function to detect language
                        await GetLanguage(userText);
                    }

                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
        static async Task GetLanguage(string text)
        {
            // Construct the JSON request body
            try
            {
                JObject jsonBody = new JObject(
                    // Create a collection of documents (we&#x27;ll only use one, but you could have more)
                    new JProperty(&quot;documents&quot;,
                    new JArray(
                        new JObject(
                            // Each document needs a unique ID and some text
                            new JProperty(&quot;id&quot;, 1),
                            new JProperty(&quot;text&quot;, text)
                    ))));
                
                // Encode as UTF-8
                UTF8Encoding utf8 = new UTF8Encoding(true, true);
                byte[] encodedBytes = utf8.GetBytes(jsonBody.ToString());
                
                // Let&#x27;s take a look at the JSON we&#x27;ll send to the service
                Console.WriteLine(utf8.GetString(encodedBytes, 0, encodedBytes.Length));

                // Make an HTTP request to the REST interface
                var client = new HttpClient();
                var queryString = HttpUtility.ParseQueryString(string.Empty);

                // Add the authentication key to the header
                client.DefaultRequestHeaders.Add(&quot;Ocp-Apim-Subscription-Key&quot;, AiSvCKey);

                // Use the endpoint to access the Text Analytics language API
                var uri = AiSvcEndpoint + &quot;text/analytics/v3.1/languages?&quot; + queryString;

                // Send the request and get the response
                HttpResponseMessage response;
                using (var content = new ByteArrayContent(encodedBytes))
                {
                    content.Headers.ContentType = new MediaTypeHeaderValue(&quot;application/json&quot;);
                    response = await client.PostAsync(uri, content);
                }

                // If the call was successful, get the response
                if (response.StatusCode == System.Net.HttpStatusCode.OK)
                {
                    // Display the JSON response in full (just so we can see it)
                    string responseContent = await response.Content.ReadAsStringAsync();
                    JObject results = JObject.Parse(responseContent);
                    Console.WriteLine(results.ToString());

                    // Extract the detected language name for each document
                    foreach (JObject document in results[&quot;documents&quot;])
                    {
                        Console.WriteLine(&quot;\nLanguage: &quot; + (string)document[&quot;detectedLanguage&quot;][&quot;name&quot;]);
                    }
                }
                else
                {
                    // Something went wrong, write the whole response
                    Console.WriteLine(response.ToString());
                }
            }
            catch(Exception ex)
            {
                Console.WriteLine(ex.Message);
            }

        }
    }
}
```

> 这段代码是一个简单的.NET Core控制台应用程序，用于调用某种AI服务（可能是Azure的Text Analytics服务）的语言检测API。主要分为几个部分：

1. 配置读取：从名为`appsettings.json`的配置文件中读取AI服务的端点（`AIServicesEndpoint`）和密钥（`AIServicesKey`）。这两个参数将用于后续的API请求。

2. 用户输入：接收用户的输入（直到用户输入`quit`为止），并将输入的文本发送到AI服务进行语言检测。

3. 构建和发送HTTP请求：创建一个JSON对象，其中包含需要进行语言检测的文本，并将该JSON对象发送到AI服务的语言检测API。请求头中包含认证密钥。

4. 处理HTTP响应：接收API的响应，解析返回的JSON数据，从中提取并打印出检测到的语言。如果API调用失败，将打印出完整的响应信息。

5. 错误处理：主程序和`GetLanguage`函数都有`try/catch`块来处理可能出现的异常，并将异常信息打印到控制台。

总的来说，这是一个典型的使用HTTP客户端调用REST API的例子，展示了如何构建请求、发送请求、处理响应和错误处理等基本步骤。

`Labfiles/01-use-azure-ai-services/C-Sharp/sdk-client/Program.cs`

```
using System;
using Azure;
using Microsoft.Extensions.Configuration;
using System.Text;
using Azure.AI.TextAnalytics;

namespace sdk_client
{
    class Program
    {

        private static string AISvcEndpoint;
        private static string AISvcKey;
        static void Main(string[] args)
        {
            try
            {
                // Get config settings from AppSettings
                IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile(&quot;appsettings.json&quot;);
                IConfigurationRoot configuration = builder.Build();
                AISvcEndpoint = configuration[&quot;AIServicesEndpoint&quot;];
                AISvcKey = configuration[&quot;AIServicesKey&quot;];

                // Get user input (until they enter &quot;quit&quot;)
                string userText = &quot;&quot;;
                while (userText.ToLower() != &quot;quit&quot;)
                {
                    Console.WriteLine(&quot;\nEnter some text (&#x27;quit&#x27; to stop)&quot;);
                    userText = Console.ReadLine();
                    if (userText.ToLower() != &quot;quit&quot;)
                    {
                        // Call function to detect language
                        string language = GetLanguage(userText);
                        Console.WriteLine(&quot;Language: &quot; + language);
                    }

                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
        static string GetLanguage(string text)
        {

            // Create client using endpoint and key
            AzureKeyCredential credentials = new AzureKeyCredential(AISvcKey);
            Uri endpoint = new Uri(AISvcEndpoint);
            var client = new TextAnalyticsClient(endpoint, credentials);

            // Call the service to get the detected language
            DetectedLanguage detectedLanguage = client.DetectLanguage(text);
            return(detectedLanguage.Name);

        }
    }
}
```



这段代码是一个.NET Core控制台应用程序，也是用于调用AI服务（Azure的Text Analytics服务）的语言检测API。这次的调用方式不同于之前的HTTP请求，而是直接使用Azure的SDK。这种方式更简洁，更易于管理和使用。

这个程序主要包含以下几个部分：

1. **配置读取**：从名为`appsettings.json`的配置文件中读取AI服务的端点（`AIServicesEndpoint`）和密钥（`AIServicesKey`）。这两个参数将用于后续的API请求。

2. **用户输入**：接收用户的输入（直到用户输入`quit`为止），并将输入的文本发送到AI服务进行语言检测。

3. **创建客户端并调用API**：使用Azure的Text Analytics SDK创建客户端，然后调用`DetectLanguage`方法进行语言检测。这个方法将文本作为输入，并返回检测到的语言。

4. **输出结果**：显示检测到的语言。

5. **错误处理**：主程序有一个`try/catch`块来处理可能出现的异常，并将异常信息打印到控制台。

总的来说，这是一个典型的使用Azure SDK调用AI服务的例子，展示了如何创建客户端、调用服务、处理结果和错误处理等基本步骤。这种方式比之前的HTTP请求方式更简洁，更符合.NET的开发习惯。

### 练习2：管理 Azure AI 服务安全性

https://microsoftlearning.github.io/mslearn-ai-services/Instructions/Exercises/02-ai-services-security.html

安全是任何应用程序的重要考虑因素，作为开发者，您应确保对资源（如Azure AI服务）的访问仅限于需要它们的人员。通常通过身份验证密钥来控制对Azure AI服务的访问，这些密钥在您最初创建Azure AI服务资源时生成。

### 练习3：监视 Azure AI 服务

https://microsoftlearning.github.io/mslearn-ai-services/Instructions/Exercises/03-monitor-ai-services.html

Azure AI服务可以成为整体应用基础设施的关键部分。能够监控活动并收到需要注意的问题警报非常重要。

### 练习4：使用容器

https://microsoftlearning.github.io/mslearn-ai-services/Instructions/Exercises/04-use-a-container.html

使用托管在Azure中的Azure AI服务使应用程序开发人员能够专注于自己代码的基础架构，同时受益于由Microsoft管理的可扩展服务。然而，在许多情况下，组织需要对其服务基础架构和传递给服务之间的数据拥有更多控制权。

许多Azure AI服务API可以打包并部署在容器中，使组织能够将Azure AI服务托管在自己的基础设施上；例如本地Docker服务器、Azure容器实例或Azure Kubernetes Services集群。容器化的Azure AI服务需要与基于Azure的Azure AI服务账户进行通信以支持计费；但是应用程序数据不会传递到后端服务，并且组织对其容器部署配置具有更大控制权，从而为身份验证、可伸缩性和其他考虑因素提供定制解决方案。

## 使用 Azure AI 服务开发决策支持解决方案

决策支持通过向用户提供建议来帮助自动执行决策。Azure AI 服务包括支持常见决策支持方案的多个服务。

### 练习 1：使用 API 测试控制台测试文本审查

https://learn.microsoft.com/zh-cn/training/modules/classify-and-moderate-text-with-azure-content-moderator/4-exercise-use-the-api-console

Cognitive Services APIs Reference

## 使用 Azure AI 视觉创建计算机视觉解决方案

计算机视觉是一个用于处理视觉感知的人工智能领域。Azure AI 视觉包括支持常见计算机视觉方案的多个服务。

### 练习1：使用 Azure AI 视觉分析图像

https://microsoftlearning.github.io/mslearn-ai-vision/Instructions/Exercises/01-analyze-images.html

Azure AI Vision是一种人工智能功能，通过分析图像来解释视觉输入的软件系统。在Microsoft Azure中，Vision Azure AI服务提供了预构建模型，用于常见的计算机视觉任务，包括分析图像以提供标题和标签建议、检测常见物体和人物。您还可以使用Azure AI Vision服务去除图像背景或创建前景抠图。

修改后C-Sharp的代码如下所示

```
using System;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Text.Json;
using System.Threading.Tasks;
using System.Drawing;
using Microsoft.Extensions.Configuration;
using Azure;

// Import namespaces
using Azure.AI.Vision.ImageAnalysis;

namespace image_analysis
{
    class Program
    {

        static async Task Main(string[] args)
        {
            try
            {
                // Get config settings from AppSettings
                IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
                IConfigurationRoot configuration = builder.Build();
                string aiSvcEndpoint = configuration["AIServicesEndpoint"];
                string aiSvcKey = configuration["AIServicesKey"];

                // Get image
                string imageFile = "images/street.jpg";
                if (args.Length > 0)
                {
                    imageFile = args[0];
                }

                // Authenticate Azure AI Vision client
                ImageAnalysisClient client = new ImageAnalysisClient(
                    new Uri(aiSvcEndpoint),
                    new AzureKeyCredential(aiSvcKey));


                // Analyze image
                AnalyzeImage(imageFile, client);

                // Remove the background or generate a foreground matte from the image
                await BackgroundForeground(imageFile, aiSvcEndpoint, aiSvcKey);

            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

        static void AnalyzeImage(string imageFile, ImageAnalysisClient client)
        {
            Console.WriteLine($"\nAnalyzing {imageFile} \n");

            // Use a file stream to pass the image data to the analyze call
            using FileStream stream = new FileStream(imageFile,
                                                     FileMode.Open);

            // Get result with specified features to be retrieved
            ImageAnalysisResult result = client.Analyze(
                BinaryData.FromStream(stream),
                VisualFeatures.Caption | 
                VisualFeatures.DenseCaptions |
                VisualFeatures.Objects |
                VisualFeatures.Tags |
                VisualFeatures.People);
            
            
            // Display analysis results
            // Get image captions
            if (result.Caption.Text != null)
            {
                Console.WriteLine(" Caption:");
                Console.WriteLine($"   \"{result.Caption.Text}\", Confidence {result.Caption.Confidence:0.00}\n");
            }

            // Get image dense captions
            Console.WriteLine(" Dense Captions:");
            foreach (DenseCaption denseCaption in result.DenseCaptions.Values)
            {
                Console.WriteLine($"   Caption: '{denseCaption.Text}', Confidence: {denseCaption.Confidence:0.00}");
            }

            // Get image tags


            // Get objects in the image


            // Get people in the image
            

        }
        static async Task BackgroundForeground(string imageFile, string endpoint, string key)
        {
            // Remove the background from the image or generate a foreground matte
            
        }
    }
}
```

这段C#代码是一个图像分析程序，使用了Azure AI Vision服务。主要包括以下几个部分：

1. **配置读取**：从`appsettings.json`配置文件中读取AI服务的端点和密钥。

```
IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
IConfigurationRoot configuration = builder.Build();
string aiSvcEndpoint = configuration["AIServicesEndpoint"];
string aiSvcKey = configuration["AIServicesKey"];
```

2. **图像文件获取**：默认使用`images/street.jpg`作为要分析的图像，如果命令行参数有提供图像文件路径，则使用命令行参数。

```
string imageFile = "images/street.jpg";
if (args.Length > 0)
{
    imageFile = args[0];
}
```

3. **AI客户端认证**：使用从配置中获取的AI服务端点和密钥创建一个`ImageAnalysisClient`对象。

```
ImageAnalysisClient client = new ImageAnalysisClient(
    new Uri(aiSvcEndpoint),
    new AzureKeyCredential(aiSvcKey));
```

4. **图像分析**：调用`AnalyzeImage`方法对图像进行分析。这个方法使用`ImageAnalysisClient`对象和图像文件路径作为参数。

```
AnalyzeImage(imageFile, client);
```

5. **背景/前景处理**：调用`BackgroundForeground`方法对图像进行背景/前景处理。这个方法使用图像文件路径、AI服务端点和密钥作为参数。

```
await BackgroundForeground(imageFile, aiSvcEndpoint, aiSvcKey);
```

在`AnalyzeImage`方法中，程序打开图像文件，然后调用`client.Analyze`方法对图像进行分析。分析结果包括图像的标题、详细标题、对象、标签和人物。然后，程序将分析结果打印到控制台。

在`BackgroundForeground`方法中，程序将进行背景/前景处理，但这部分代码尚未完成。

如果在执行过程中发生任何异常，程序将捕获异常并打印异常消息。

正常输出如下

```
zunhua@Azure:~/mslearn-ai-vision/Labfiles/01-analyze-images/C-Sharp/image-analysis$ dotnet run images/street.jpg

Analyzing images/street.jpg 

 Caption:
   "a man walking a dog on a leash on a street", Confidence 0.82

 Dense Captions:
   Caption: 'a man walking a dog on a leash on a street', Confidence: 0.82
   Caption: 'a man walking on a street', Confidence: 0.69
   Caption: 'a yellow car on the street', Confidence: 0.78
   Caption: 'a black dog walking on the street', Confidence: 0.75
   Caption: 'a blurry image of a blue car', Confidence: 0.82
   Caption: 'a yellow taxi cab on the street', Confidence: 0.72
```

![](img/5a5366991ab001fc088594ac784f0ffc.png)

完成体的



```python
from dotenv import load_dotenv
import os
from PIL import Image, ImageDraw
import sys
from matplotlib import pyplot as plt
from azure.core.exceptions import HttpResponseError
import requests

# Import namespaces
from azure.ai.vision.imageanalysis import ImageAnalysisClient
from azure.ai.vision.imageanalysis.models import VisualFeatures
from azure.core.credentials import AzureKeyCredential

def main():
    global cv_client

    try:
        # Get Configuration Settings
        load_dotenv()
        ai_endpoint = os.getenv('AI_SERVICE_ENDPOINT')
        ai_key = os.getenv('AI_SERVICE_KEY')

        # Get image
        image_file = 'images/street.jpg'
        if len(sys.argv) > 1:
            image_file = sys.argv[1]

        with open(image_file, "rb") as f:
            image_data = f.read()

        # Authenticate Azure AI Vision client
        cv_client = ImageAnalysisClient(
            endpoint=ai_endpoint,
            credential=AzureKeyCredential(ai_key)
        )
        
        # Analyze image
        AnalyzeImage(image_file, image_data, cv_client)
        
        # Background removal
        BackgroundForeground(ai_endpoint, ai_key, image_file)

    except Exception as ex:
        print(ex)

def AnalyzeImage(image_filename, image_data, cv_client):
    print('\nAnalyzing image...')

    try:
        # Get result with specified features to be retrieved
        result = cv_client.analyze(
            image_data=image_data,
            visual_features=[
                VisualFeatures.CAPTION,
                VisualFeatures.DENSE_CAPTIONS,
                VisualFeatures.TAGS,
                VisualFeatures.OBJECTS,
                VisualFeatures.PEOPLE],
        )        

    except HttpResponseError as e:
        print(f"Status code: {e.status_code}")
        print(f"Reason: {e.reason}")
        print(f"Message: {e.error.message}")

    # Display analysis results
    # Get image captions
    if result.caption is not None:
        print("\nCaption:")
        print(" Caption: '{}' (confidence: {:.2f}%)".format(result.caption.text, result.caption.confidence * 100))

    # Get image dense captions
    if result.dense_captions is not None:
        print("\nDense Captions:")
        for caption in result.dense_captions.list:
            print(" Caption: '{}' (confidence: {:.2f}%)".format(caption.text, caption.confidence * 100))

    # Get image tags
    if result.tags is not None:
        print("\nTags:")
        for tag in result.tags.list:
            print(" Tag: '{}' (confidence: {:.2f}%)".format(tag.name, tag.confidence * 100))    

    # Get objects in the image
    if result.objects is not None:
        print("\nObjects in image:")

        # Prepare image for drawing
        image = Image.open(image_filename)
        fig = plt.figure(figsize=(image.width/100, image.height/100))
        plt.axis('off')
        draw = ImageDraw.Draw(image)
        color = 'cyan'

        for detected_object in result.objects.list:
            # Print object name
            print(" {} (confidence: {:.2f}%)".format(detected_object.tags[0].name, detected_object.tags[0].confidence * 100))
            
            # Draw object bounding box
            r = detected_object.bounding_box
            bounding_box = ((r.x, r.y), (r.x + r.width, r.y + r.height)) 
            draw.rectangle(bounding_box, outline=color, width=3)
            plt.annotate(detected_object.tags[0].name,(r.x, r.y), backgroundcolor=color)

        # Save annotated image
        plt.imshow(image)
        plt.tight_layout(pad=0)
        outputfile = 'objects.jpg'
        fig.savefig(outputfile)
        print('  Results saved in', outputfile)    

    # Get people in the image
    if result.people is not None:
        print("\nPeople in image:")

        # Prepare image for drawing
        image = Image.open(image_filename)
        fig = plt.figure(figsize=(image.width/100, image.height/100))
        plt.axis('off')
        draw = ImageDraw.Draw(image)
        color = 'cyan'

        for detected_people in result.people.list:
            # Draw object bounding box
            r = detected_people.bounding_box
            bounding_box = ((r.x, r.y), (r.x + r.width, r.y + r.height))
            draw.rectangle(bounding_box, outline=color, width=3)

            # Return the confidence of the person detected
            #print(" {} (confidence: {:.2f}%)".format(detected_people.bounding_box, detected_people.confidence * 100))
            
        # Save annotated image
        plt.imshow(image)
        plt.tight_layout(pad=0)
        outputfile = 'people.jpg'
        fig.savefig(outputfile)
        print('  Results saved in', outputfile)    

def BackgroundForeground(endpoint, key, image_file):
    # Define the API version and mode
    api_version = "2023-02-01-preview"
    mode="backgroundRemoval" # Can be "foregroundMatting" or "backgroundRemoval"
    
    # Remove the background from the image or generate a foreground matte
    print('\nRemoving background from image...')
        
    url = "{}computervision/imageanalysis:segment?api-version={}&mode={}".format(endpoint, api_version, mode)

    headers= {
        "Ocp-Apim-Subscription-Key": key, 
        "Content-Type": "application/json" 
    }

    image_url="https://github.com/MicrosoftLearning/mslearn-ai-vision/blob/main/Labfiles/01-analyze-images/Python/image-analysis/{}?raw=true".format(image_file)  

    body = {
        "url": image_url,
    }
        
    response = requests.post(url, headers=headers, json=body)

    image=response.content
    with open("background.png", "wb") as file:
        file.write(image)
    print('  Results saved in background.png \n')   

if __name__ == "__main__":
    main()
```

### 练习2：使用 Azure AI 自定义视觉对图像进行分类
`https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/17-image-classification.html`

Azure AI自定义视觉服务使您能够创建基于自己图像训练的计算机视觉模型。您可以使用它来训练图像分类和物体检测模型，然后可以从应用程序中发布和使用。

在这个练习中，您将使用Custom Vision服务来训练一个图像分类模型，该模型可以识别三种水果类别（苹果、香蕉和橙子）。

`https://www.customvision.ai/`

### 练习3 (可选)：使用 Azure AI 自定义视觉检测图像中的对象
`https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/18-object-detection.html`

在这个练习中，您将使用自定义视觉服务来训练一个物体检测模型，该模型可以在图像中检测和定位三种水果类别（苹果、香蕉和橙子）。

### 练习4：检测、分析和识别人脸
`https://microsoftlearning.github.io/mslearn-ai-vision/Instructions/Exercises/04-face-service.html`

检测和分析人脸是核心的人工智能能力。在这个练习中，您将探索两个Azure AI服务，用于处理图像中的人脸：Azure AI Vision服务和Face服务。

> 注意：从2022年6月21日起，返回个人身份信息的Azure AI服务的功能仅限于获得有限访问权限的客户。此外，不再提供推断情绪状态的能力。这些限制可能会影响本实验。我们正在努力解决这个问题，但在此期间，在按照下面的步骤操作时可能会遇到一些错误；对此我们深表歉意。有关微软所做更改及原因的详细信息，请参阅《负责任AI投资和面部识别保护措施》。

功能申请链接：[A](https://customervoice.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR7en2Ais5pxKtso_Pz4b1_xUQjA5SkYzNDM4TkcwQzNEOE1NVEdKUUlRRCQlQCN0PWcu)zure AI Vision Face API Recognition Limited Access Review

### 练习5：读取图像中的文本
`https://microsoftlearning.github.io/mslearn-ai-vision/Instructions/Exercises/05-ocr.html`

光学字符识别（OCR）是计算机视觉的一个子集，处理图像和文档中的文字阅读。Azure AI Vision服务提供了一个用于读取文本的API，在这个练习中你将探索它。

### 练习6：分析视频
`https://microsoftlearning.github.io/mslearn-ai-vision/Instructions/Exercises/06-video-indexer.html`

今天创建和消费的数据中，很大一部分是以视频格式存在的。Azure AI 视频索引器是一个由人工智能驱动的服务，您可以使用它来对视频进行索引并从中提取见解。



> 注意：从2022年6月21日起，返回个人身份信息的Azure AI服务功能仅限于获得有限访问权限的客户使用。未经获得有限访问批准，本实验中无法使用Video Analyzer识别人物和名人。有关Microsoft所做更改的详细信息以及原因，请参阅面部识别的负责任AI投资和保障措施。

[Azure AI Video Indexer - Cognitive Video Indexing](https://www.videoindexer.ai/)

`https://aka.ms/responsible-ai-video`

# 使用 Azure AI 服务开发自然语言处理解决方案

自然语言处理 (NLP) 解决方案使用语言模型解释书面或口头语言的语义含义。可以使用语言理解服务为应用程序生成语言模型。

## 练习1：分析文本

`https://microsoftlearning.github.io/mslearn-ai-language/Instructions/Exercises/01-analyze-text.html`

Azure语言支持对文本进行分析，包括语言检测、情感分析、关键词提取和实体识别。

例如，假设一家旅行社想要处理已提交到公司网站的酒店评论。通过使用Azure AI Language，他们可以确定每个评论所写的语言，评论的情感（积极、中性或消极），可能指示评论讨论主题的关键短语以及在评论中提到的地点、地标或人物等命名实体。

## 练习2：创建问题解答解决方案

`https://microsoftlearning.github.io/mslearn-ai-language/Instructions/Exercises/02-qna.html`

最常见的对话场景之一是通过常见问题解答（FAQ）知识库提供支持。许多组织将FAQ发布为文档或网页，这对于一小部分问答配对来说效果很好，但大型文档可能难以搜索且耗时。

Azure AI Language包括一个问答功能，使您能够创建一个可以使用自然语言输入查询的问答配对知识库，并且通常用作机器人可用来查找用户提交的问题的资源。

> 遇到`MissingSubscriptionRegistration`错误通常意味着你尝试使用的服务（在这个案例中是Azure Search，现在称为Azure Cognitive Search）在你的Azure订阅中还没有注册。为了注册一个资源提供者，你可以使用Azure CLI（命令行接口）。

> 以下是注册Azure Cognitive Search服务的命令：

```
az provider register --namespace Microsoft.Search
```

> 执行这个命令之后，可能需要一些时间来完成注册。你可以使用以下命令来检查注册状态：

```
az provider show --namespace Microsoft.Search --query "registrationState"
```

> 当`registrationState`显示为`Registered`时，你就可以开始使用Azure Cognitive Search服务了。

> 请确保在执行这些命令之前，你已经登录到了Azure CLI，并且你的账户有权限注册新的资源提供者。如果你没有相应的权限，你可能需要联系你的Azure订阅管理员来为你注册服务。

Language Studio - Microsoft Azure

## 练习3：构建 Azure AI 服务对话语言理解模型

`https://microsoftlearning.github.io/mslearn-ai-language/Instructions/Exercises/03-language-understanding.html`

Azure AI 语言服务使您能够定义一个对话式语言理解模型，应用程序可以使用该模型来解释用户的自然语言输入、预测用户意图（他们想要实现什么）并识别任何应用于该意图的实体。

例如，一个针对时钟应用程序的对话式语言模型可能会处理以下输入：

伦敦现在几点？

这种类型的输入是一个示例性表述（用户可能说或键入的内容），其期望意图是获取特定位置（实体）中的时间；在本例中为伦敦。

> 注意：Azure AI 语言服务的对话式语言理解功能目前处于预览阶段，可能会有变化。在某些情况下，模型训练可能会失败 - 如果发生这种情况，请重试。

## 练习4：对文本进行分类

`https://microsoftlearning.github.io/mslearn-ai-language/Instructions/Exercises/04-text-classification.html`

Azure AI Language提供了几种NLP功能，包括关键短语识别、文本摘要和情感分析。该语言服务还提供自定义功能，如自定义问答和自定义文本分类。

为了测试Azure AI Language服务的自定义文本分类功能，我们将使用Language Studio配置模型，然后使用在Cloud Shell中运行的小型命令行应用程序进行测试。这里使用的相同模式和功能可以用于实际应用程序。

## 练习5：提取自定义实体

`https://microsoftlearning.github.io/mslearn-ai-language/Instructions/Exercises/05-extract-custom-entities.html`

除了其他自然语言处理功能外，Azure AI 语言服务还可以让您定义自定义实体，并从文本中提取它们的实例。

为了测试自定义实体提取功能，我们将通过 Azure AI 语言工作室创建一个模型并进行训练，然后使用命令行应用程序来测试。

## 练习6：使用 Azure AI 翻译服务翻译文本

`https://microsoftlearning.github.io/mslearn-ai-language/Instructions/Exercises/06-translate-text.html`

Azure AI Translator是一项服务，使您能够在不同语言之间进行文本翻译。例如，假设一个旅行社想要检查已提交到公司网站的酒店评论，并将英语作为分析所使用的语言。通过使用Azure AI Translator，他们可以确定每个评论所写的语言，并且如果它不是英语，则将其从原始语言翻译成英语。

## 练习7：创建启用了语音的应用

`https://microsoftlearning.github.io/mslearn-ai-language/Instructions/Exercises/07-speech.html`

Azure AI Speech是一个提供与语音相关功能的服务，包括：

+   语音转文本API，使您能够实现语音识别（将可听到的口头言辞转换为文本）。

+   文本转语音API，使您能够实现语音合成（将文本转换为可听到的言辞）。

在这个练习中，您将使用这两个API来实现一个说时钟应用程序。

## 练习8：翻译语音

`https://microsoftlearning.github.io/mslearn-ai-language/Instructions/Exercises/08-translate-speech.html`

Azure AI语音包括一个语音翻译API，您可以用它来翻译口头语言。例如，假设您想开发一个翻译应用程序，供人们在不懂当地语言的地方旅行时使用。他们可以用自己的语言说出像“车站在哪里？”或者“我需要找一家药店”这样的短语，并将其翻译成当地语言。

> 注意：此练习要求您使用带有扬声器/耳机的计算机。为了获得最佳体验，还需要麦克风。一些托管的虚拟环境可能能够捕获来自本地麦克风的音频，但如果这不起作用（或者您根本没有麦克风），您可以使用提供的音频文件进行语音输入。请仔细按照说明操作，因为根据您是使用麦克风还是音频文件，需要选择不同选项。

# 使用 Azure 认知搜索实现知识挖掘

你是否将信息锁定在结构化和非结构化数据源中？ 使用 Azure 认知搜索，可以从这些数据中提取关键见解，并使应用程序能够搜索和分析它们。

## 练习1：创建搜索解决方案

`https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Exercises/01-azure-search.html`

所有组织都依赖信息来做出决策、回答问题和高效运作。对于大多数组织来说，问题不在于缺乏信息，而是如何从庞大的文件、数据库和其他存储信息的来源中找到并提取所需的信息。

例如，假设玛吉旅行社是一家专门组织世界各地城市旅行的旅行社。随着时间推移，该公司已经积累了大量的信息，包括宣传册以及顾客提交的酒店评论。这些数据对于旅行代理商和顾客在计划旅行时具有重要洞察力，但庞大的数据量可能会使查找相关信息以回答特定顾客问题变得困难。

为了解决这个挑战，玛吉旅行社可以使用Azure AI Search实施一个方案，在其中通过使用AI技术对文档进行索引和增强处理，使其更容易搜索。

以下命令可以直接从Azure Cli里直接运行用于上传文件

```
# Set values for your storage account
$subscription_id = "6cc61414-c4d0-4179-a58e-1d47c9929894"
$azure_storage_account = "chengzhaistor"
$azure_storage_key = "bVRvBXB6unSwU4KE6iZ4v52nSO9fIRaDuvdMPsY78kb4yHBvdQgVD4DPdMw5s0BTPwQdxDIwCrsk+ASt2Wc24Q=="

Write-Host "Creating container..."
az storage container create --account-name $azure_storage_account --subscription $subscription_id --name margies --public-access blob --auth-mode key --account-key $azure_storage_key

Write-Host "Uploading files..."
az storage blob upload-batch -d margies -s data --account-name $azure_storage_account --auth-mode key --account-key $azure_storage_key --output none
```

## 练习2：实现自定义技能

`https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Exercises/02-search-skills.html`

Azure AI 搜索使用 AI 技能的丰富流程，从文档中提取 AI 生成的字段并将其包含在搜索索引中。有一套全面的内置技能可供使用，但如果这些技能无法满足您的特定要求，则可以创建自定义技能。

在此练习中，您将创建一个自定义技能，用于统计文档中各个单词的频率，并生成一个最常用的前五个单词列表，并将其添加到 Margie's Travel 的搜索解决方案 - 一个虚构旅行社。

## 练习3 (可选)：创建知识存储

`https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Exercises/03-knowledge-store.html`

Azure AI Search使用AI技能的丰富流程从文档中提取AI生成的字段，并将其包含在搜索索引中。尽管索引可能被认为是索引过程的主要输出，但它所包含的丰富数据也可能以其他方式有用。例如：

+   由于索引本质上是一组JSON对象，每个对象代表一个已索引记录，因此将这些对象导出为JSON文件并集成到数据编排流程中（使用诸如Azure Data Factory之类的工具）可能很有用。

+   您可能希望将索引记录规范化为关系模式表格，以便使用Microsoft Power BI等工具进行分析和报告。

+   在索引过程中提取嵌入式图像后，您可能希望将这些图像保存为文件。

在本练习中，您将为Margie's Travel实施一个知识存储库。Margie's Travel是一个虚构旅行社，利用宣传册和酒店评论中的信息来帮助客户计划旅行。

## 练习4 (可选)：使用自定义类扩充认知搜索索引

`https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Exercises/04-exercise-enrich-cognitive-custom-classes.html`

您已经构建了一个搜索解决方案，现在想要为索引添加Azure AI服务以进行语言增强。

在这个练习中，您将创建一个Azure AI搜索解决方案，并使用Language Studio自定义文本分类项目的结果来丰富索引。您将创建一个函数应用程序来连接搜索和分类模型。

## 练习5 (可选)：实现搜索结果增强

`https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Exercises/05-exercise-implement-enhancements-to-search-results.html`

您有一个现有的搜索服务，用于假日预订应用程序。您已经注意到搜索结果的相关性影响了您获得的预订数量。最近，您还添加了葡萄牙酒店，因此希望提供葡萄牙语作为支持的语言。

在这个练习中，您将添加一个评分配置文件来改善搜索结果的相关性。然后，您将使用Azure AI服务为所有酒店添加葡萄牙语描述。

## 练习6 (可选)：使用 Azure 机器学习模型扩充搜索索引

`https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Exercises/06-exercise-enrich-search-index-use-model.html`

您可以利用机器学习的力量来丰富搜索索引。为此，您将使用在Azure AI Machine Learning工作室中训练的模型，并从机器学习自定义技能集中调用它。

在这个练习中，您将创建一个Azure AI Machine Learning工作室模型，然后使用该模型进行训练、部署和测试端点。然后，您将创建一个Azure Cognitive Search服务，创建样本数据，并使用Azure AI Machine Learning工作室端点来丰富索引。

## 练习7 (可选)：使用推送 API 添加到索引

`https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Exercises/07-exercise-add-to-index-use-push-api.html`

您想探索如何使用C#代码创建Azure AI Search索引并将文档上传到该索引中。

在这个练习中，您将克隆一个现有的C#解决方案并运行它，以确定最佳批处理大小来上传文档。然后，您将使用此批处理大小，并采用多线程方法有效地上传文档。

## 练习8 (可选)：调试搜索问题

`https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Exercises/08-exercise-debug-search-issues.html`

您已经构建了搜索解决方案，但注意到索引器上有一些警告。

在这个练习中，您将创建一个Azure AI搜索解决方案，导入一些示例数据，然后解决索引器上的一个警告。

## 练习9 (可选)：对索引使用语义搜索

`https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Exercises/09-semantic-search-exercise.html`

在这个练习中，您将向索引添加语义搜索，并使用语义搜索进行查询。

## 练习10 (可选)：使用 REST API 运行矢量搜索查询

`https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Exercises/10-vector-search-exercise.html`

在这个练习中，您将设置项目、创建索引、上传文档并运行查询。

# 使用 Azure AI 文档智能开发解决方案

在本学习路径中，你将了解 Azure AI 文档智能解决方案如何使你能够从键入或手写的表单中捕获数据。 你还将了解如何为自定义表单类型生成解决方案，并将该解决方案集成到 Azure 认知搜索管道中。 将了解如何执行以下操作：

+   设计一个解决方案，利用 Azure AI 文档智能来分析业务表单。

+   创建使用文档智能分析常见文档的解决方案。

+   创建一个使用文档智能来分析不同自定义表单类型的解决方案。

+   将 Azure AI 文档智能服务作为自定义技能包含在 Azure 认知搜索管道中

## 练习1：使用 Azure AI 文档智能分析文档

`https://microsoftlearning.github.io/mslearn-ai-document-intelligence/Instructions/Exercises/01-use-prebuilt-models.html`

在这个练习中，您将在Azure订阅中设置一个Azure AI文档智能资源。您将使用Azure AI文档智能工作室和C#或Python向该资源提交表单进行分析。



# Azure AI 文档智能

## 使用 Azure 表单识别器分析发票（代码示例）

```csharp
using Azure;
using Azure.AI.FormRecognizer.DocumentAnalysis;

// Store connection information
string endpoint = "https://chengzhform1029.cognitiveservices.azure.com/";
string apiKey = "be4f96c4581d4976a997952565a37730";

Uri fileUri = new Uri("https://github.com/MicrosoftLearning/mslearn-ai-document-intelligence/blob/main/Labfiles/01-prebuild-models/sample-invoice/sample-invoice.pdf?raw=true");

Console.WriteLine("\nConnecting to Forms Recognizer at: {0}", endpoint);
Console.WriteLine("Analyzing invoice at: {0}\n", fileUri.ToString());

// Create the client
var cred = new AzureKeyCredential(apiKey);
var client = new DocumentAnalysisClient(new Uri(endpoint), cred);

// Analyze the invoice
AnalyzeDocumentOperation operation = await client.AnalyzeDocumentFromUriAsync(WaitUntil.Completed, "prebuilt-invoice", fileUri);
await operation.WaitForCompletionAsync();

// Display invoice information to the user
AnalyzeResult result = operation.Value;
    
foreach (AnalyzedDocument invoice in result.Documents)
{
    if (invoice.Fields.TryGetValue("VendorName", out DocumentField? vendorNameField))
    {
        if (vendorNameField.FieldType == DocumentFieldType.String)
        {
            string vendorName = vendorNameField.Value.AsString();
            Console.WriteLine($"Vendor Name: '{vendorName}', with confidence {vendorNameField.Confidence}.");
        }
    }

    if (invoice.Fields.TryGetValue("CustomerName", out DocumentField? customerNameField))
    {
        if (customerNameField.FieldType == DocumentFieldType.String)
        {
            string customerName = customerNameField.Value.AsString();
            Console.WriteLine($"Customer Name: '{customerName}', with confidence {customerNameField.Confidence}.");
        }
    }

    if (invoice.Fields.TryGetValue("InvoiceTotal", out DocumentField? invoiceTotalField))
    {
        if (invoiceTotalField.FieldType == DocumentFieldType.Currency)
        {
            CurrencyValue invoiceTotal = invoiceTotalField.Value.AsCurrency();
            Console.WriteLine($"Invoice Total: '{invoiceTotal.Symbol}{invoiceTotal.Amount}', with confidence {invoiceTotalField.Confidence}.");
        }
    }
}
Console.WriteLine("\nAnalysis complete.\n");
```

这段 C# 代码使用 Azure 的 Form Recognizer 服务来分析和提取 PDF 发票中的信息。

以下是代码的详细解释：

1. 首先，代码定义了连接到 Azure Form Recognizer 服务所需的终端 URL 和 API 密钥。

2. 然后，代码定义了一个 URI，指向要分析的 PDF 发票。

3. 使用这些信息，代码创建了一个 `DocumentAnalysisClient` 实例。

4. 使用 `AnalyzeDocumentFromUriAsync` 方法，代码开始异步分析指定 URI 中的发票。这个方法返回一个 `AnalyzeDocumentOperation` 对象，该对象表示正在进行的分析操作。

5. 使用 `WaitForCompletionAsync` 方法，代码等待分析操作完成。

6. 一旦分析完成，代码获取分析结果，这些结果包含了发票中识别出的各种字段。

7. 最后，代码遍历这些字段，尝试提取 `VendorName`（供应商名称）、`CustomerName`（客户名称）和 `InvoiceTotal`（发票总额）字段。如果这些字段存在并且类型正确，代码就会打印出它们的值和置信度。

这段代码的目标是从 PDF 发票中提取关键信息，并将这些信息打印到控制台。

```
Connecting to Forms Recognizer at: https://chengzhform1019.cognitiveservices.azure.com/
Analyzing invoice at: https://github.com/MicrosoftLearning/mslearn-ai-document-intelligence/blob/main/Labfiles/01-prebuild-models/sample-invoice/sample-invoice.pdf?raw=true

Vendor Name: 'CONTOSO LTD.', with confidence 0.93.
Customer Name: 'MICROSOFT CORPORATION', with confidence 0.915.
Invoice Total: '$110', with confidence 0.97.

Analysis complete.
```

<https://github.com/MicrosoftLearning/mslearn-ai-document-intelligence/blob/main/Labfiles/01-prebuild-models/sample-invoice/sample-invoice.pdf?raw=true>

## 练习2：从自定义表单提取数据

`https://microsoftlearning.github.io/mslearn-ai-document-intelligence/Instructions/Exercises/02-custom-document-intelligence.html`

假设一家公司目前要求员工手动购买订单表并将数据输入到数据库中。他们希望您利用人工智能服务来改进数据录入过程。您决定构建一个机器学习模型，该模型将读取表格并生成结构化数据，以便自动更新数据库。

Azure AI 文档智能是一项 Azure AI 服务，使用户能够构建自动化的数据处理软件。这个软件可以使用光学字符识别（OCR）从表单文档中提取文本、键值对和表格。Azure AI 文档智能具有预先构建的模型，用于识别发票、收据和名片。该服务还提供训练自定义模型的功能。在本练习中，我们将重点介绍如何构建自定义模型。

## 练习3（可选）：创建组合模型

`https://microsoftlearning.github.io/mslearn-ai-document-intelligence/Instructions/Exercises/03-composed-model.html`

在本练习中，你将创建并训练两个用于分析不同税务表的自定义模型。 然后，你将创建一个包含这两个自定义模型的组合模型。 你将通过提交一个表单来测试模型，并检查它是否正确识别文档类型和标记字段。

## 练习4（可选）：建立和部署 Azure AI 文档智能自定义技能

`https://learn.microsoft.com/zh-cn/training/modules/build-form-recognizer-custom-skill-for-azure-cognitive-search/4-exercise-build/deploy`

在本练习中，你将创建并部署一个 Web 服务，该服务充当认知搜索管道中的自定义技能。 Web 技能调用 Azure AI 文档智能来提取发票字段（例如客户名称），以便将其包含在索引中

# Azure Applied Skills for AI Topic

`https://learn.microsoft.com/zh-cn/credentials/browse/?credential_types=applied%20skills&roles=ai-engineer`

## 构建 Azure AI 视觉解决方案

## 使用 Azure AI 语言构建自然语言处理解决方案

## 使用 Azure AI 文档智能创建智能文档处理解决方案

## 使用 Azure OpenAI 服务开发生成式 AI 解决方案

## 扩展资料

[实现LLM应用基础架构](https://zsyhjtnsa5.feishu.cn/docx/EnLadsWgnoyJG1xvE08cTFcBnhg)

[使用 LangChain 开发 LLM 应用](https://zsyhjtnsa5.feishu.cn/docx/Jw4SdTFTXoIU9xx2GfEc2QSInYf)

[ChatGLM-2大语言模型的部署和使用](https://zsyhjtnsa5.feishu.cn/docx/X0bodh0v2ouBAfx15r8cZkM2ncg)

## 备注

![](img/ea3a37bfe84c5f7e445cf494fb747644.png)

[人工智能体系综述.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VpzpbCGK5okDIYxxv89cfk18ngb)

智能解决方案deck:

备用gitee代码库：

`https://gitee.com/cloudzun/mslearn-ai-services`

`https://gitee.com/cloudzun/mslearn-ai-vision`
