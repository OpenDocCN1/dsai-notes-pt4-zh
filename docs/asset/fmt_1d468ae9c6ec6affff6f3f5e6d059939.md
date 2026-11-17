
- Suitable for home or business settings.
- Qualified for contract use.

### CONSTRUCTION

- 5-wheel plastic coated aluminum base.
- Pneumatic chair adjust for easy raise/lower action.

### DIMENSIONS

- WIDTH 53 CM | 20.87”
- DEPTH 51 CM | 20.08”
- HEIGHT 80 CM | 31.50”
- SEAT HEIGHT 44 CM | 17.32”
- SEAT DEPTH 41 CM | 16.14”

### OPTIONS

- Soft or hard-floor caster options.
- Two choices of seat foam densities:  
  medium (1.8 lb/ft3) or high (2.8 lb/ft3)
- Armless or 8 position PU armrests

### MATERIALS

#### SHELL BASE GLIDER

- Cast Aluminum with modified nylon PA6/PA66 coating.
- Shell thickness: 10 mm.

#### SEAT

- HD36 foam

### COUNTRY OF ORIGIN

- Italy

"""

所以我的提示是：您的任务是帮助营销团队基于技术事实表为零售网站或产品创建描述，编写产品描述等。

这是我向大型语言模型解释任务的第一次尝试。让我按Shift + Enter，这需要几秒钟运行，我们得到了结果。

````
prompt = f"""
Your task is to help a marketing team create a 
description for a retail website of a product based 
on a technical fact sheet.

Write a product description based on the information 
provided in the technical specifications delimited by 
triple backticks.

Technical specifications: ```{fact_sheet_chair}```
"""
response = get_completion(prompt)
print(response)
"""
Introducing our stunning mid-century inspired office chair, the perfect addition to any home or business setting. Part of a beautiful family of office furniture, including filing cabinets, desks, bookcases, meeting tables, and more, this chair is available in several options of shell color and base finishes to suit your style. Choose from plastic back and front upholstery (SWC-100) or full upholstery (SWC-110) in 10 fabric and 6 leather options.

The chair is constructed with a 5-wheel plastic coated aluminum base and features a pneumatic chair adjust for easy raise/lower action. It is available with or without armrests and is qualified for contract use. The base finish options are stainless steel, matte black, gloss white, or chrome.

Measuring at a width of 53 cm, depth of 51 cm, and height of 80 cm, with a seat height of 44 cm and seat depth of 41 cm, this chair is designed for ultimate comfort. You can also choose between soft or hard-floor caster options and two choices of seat foam densities: medium (1.8 lb/ft3) or high (2.8 lb/ft3). The armrests are available in either an armless or 8 position PU option.

The materials used in the construction of this chair are of the highest quality. The shell base glider is made of cast aluminum with modified nylon PA6/PA66 coating and has a shell thickness of 10 mm. The seat is made of HD36 foam, ensuring maximum comfort and durability.

This chair is made in Italy and is the perfect combination of style and functionality. Upgrade your workspace with our mid-century inspired office chair today!
"""
````

看起来它已经很好地写了一个描述，介绍了一款令人惊叹的中世纪灵感办公椅，完美的版型等等。

### 问题1：生成的内容太长

但是当我看到这个结果时，我觉得，哇，这太长了。它确实做到了我要求它做的事情，也就是从技术事实表开始写产品描述。但是当我看到这个结果时，我觉得这有点长。也许我们希望它再短一点。

所以我想到了一个主意，我编写了一个提示并得到了结果。我对它不是很满意，因为它太长了，所以我会澄清我的提示，说使用最多50个单词来更好地指导所需长度，然后再次运行它。看起来这是一个更好的产品简短描述，介绍了一个中世纪灵感的办公椅，同时又时尚又实用。不错。

````
prompt = f"""
Your task is to help a marketing team create a 
description for a retail website of a product based 
on a technical fact sheet.

Write a product description based on the information 
provided in the technical specifications delimited by 
triple backticks.

Use at most 50 words.

Technical specifications: ```{fact_sheet_chair}```

"""

response = get_completion(prompt)

print(response)

"""
Introducing our mid-century inspired office chair, part of a beautiful furniture family. Available in various shell colors and base finishes, with plastic or full upholstery options in fabric or leather. Suitable for home or business use, with a 5-wheel base and pneumatic chair adjust. Made in Italy.
"""
````

让我再确认一下这个长度。我将获取响应，根据空格进行分割，然后你将打印出长度。所以有52个单词。其实还不错。大型语言模型在遵循非常精确的单词计数指令方面表现还可以，但并不是特别出色。但这个结果实际上还不错。有时会输出60或65个单词，但这还算是合理范围内的。有些事情你可以这样做。让我再次运行一下。但这些都是告诉大型语言模型您想要的输出长度的不同方法。所以这是一个、两个、三个。我数了这些句子。看起来我做得不错。我也看到有些人有时会像这样做，我不知道，使用至多280个字符。由于大型语言模型解释文本的方式，使用一种称为分词器的工具，因此它们在计算字符数方面表现一般。但是，281个字符。它实际上非常接近。通常，大型语言模型无法达到这种程度。但这些都是可以尝试的不同方式，以控制输出的长度。但接着，我们只需将其切换回最多使用50个单词即可。这就是我们刚才得到的结果。

![](img/a610d6ac22be5c8e5c9cd23a6001c9c4.png)

### 问题2：面向受众不对

当我们继续为我们的网站修改这段文本时，我们可能会决定，哇，这个网站不是直接销售给消费者，而是旨在向家具零售商销售家具，他们更感兴趣的是椅子的技术细节和材料。在这种情况下，您可以采用这个提示，并说，我要修改这个提示，使其更精确地描述技术细节。

因此，让我继续修改这个提示。我要说，这个描述是为家具零售商而设计的，因此应该是技术性的，重点放在材料、产品和构造方面。好的，让我们运行它。

````
prompt = f"""
Your task is to help a marketing team create a 
description for a retail website of a product based 
on a technical fact sheet.

Write a product description based on the information 
provided in the technical specifications delimited by 
triple backticks.

The description is intended for furniture retailers, 
so should be technical in nature and focus on the 
materials the product is constructed from.

Use at most 50 words.

Technical specifications: ```{fact_sheet_chair}```
"""
response = get_completion(prompt)
print(response)
"""
Introducing our mid-century inspired office chair, perfect for both home and business settings. With a range of shell colors and base finishes, including stainless steel and matte black, this chair is available with or without armrests. The 5-wheel plastic coated aluminum base and pneumatic chair adjust make it easy to move and adjust to your desired height. Made with high-quality materials, including a cast aluminum shell and HD36 foam seat, this chair is built to last.
"""
````

看起来不错。它说，镀铝底座和气动椅子。高质量材料。通过修改提示，您可以让它更专注于您想要的特定特征。当我看到这个时，我可能会决定，在描述的结尾处，我还想包括产品ID。因此，我可以进一步改进这个提示。为了让它给我产品ID，我可以在描述的结尾处添加这个指令，在技术规格中包括每个7个字符的产品ID。让我们运行它，看看会发生什么。

````
prompt = f"""
Your task is to help a marketing team create a 
description for a retail website of a product based 
on a technical fact sheet.

Write a product description based on the information 
provided in the technical specifications delimited by 
triple backticks.

The description is intended for furniture retailers, 
so should be technical in nature and focus on the 
materials the product is constructed from.

At the end of the description, include every 7-character 
Product ID in the technical specification.

Use at most 50 words.

Technical specifications: ```{fact_sheet_chair}```

"""

response = get_completion(prompt)

print(response)

"""

chair, perfect for home or business settings. With a range of shell colors and base finishes, and the option of plastic or full upholstery, this chair is both stylish and comfortable. Constructed with a 5-wheel plastic coated aluminum base and pneumatic chair adjust, it's also practical. Available with or without armrests and suitable for contract use. Product ID: SWC-100, SWC-110.

"""
````

于是它说，介绍我们的中世纪办公椅，壳颜色，讲述塑料涂层铝底座，实用，一些选项，讲述了两个产品ID。看起来不错。

您刚才所看到的是许多开发者将要经历的迭代提示开发的简短示例。我认为一个指南是，在上一个视频中，您看到Yisa分享了许多最佳实践。因此，我通常会牢记这些最佳实践，清晰明确地表达指令，如果需要，给模型一些时间去思考。有了这些想法，通常值得尝试首次编写提示，观察发生了什么，然后从那里开始迭代优化提示，以逐渐接近所需的结果。因此，许多成功的提示，您可能会在各种程序中看到，都是通过这样的迭代过程获得的。

### 问题3：用表格描述

让我们来玩一下，我想给你展示一个更复杂的提示示例，让你感受一下ChatGPT的能力。我在这里添加了一些额外的指令。在描述之后，包括一个给出产品尺寸的表格，然后将所有内容格式化为HTML。那么让我们运行它。

```
prompt = f"""
Your task is to help a marketing team create a 
description for a retail website of a product based 
on a technical fact sheet.

Write a product description based on the information 
provided in the technical specifications delimited by 
triple backticks.

The description is intended for furniture retailers, 
so should be technical in nature and focus on the 
materials the product is constructed from.

At the end of the description, include every 7-character 
Product ID in the technical specification.

After the description, include a table that gives the 
product's dimensions. The table should have two columns.

In the first column include the name of the dimension. 
```
