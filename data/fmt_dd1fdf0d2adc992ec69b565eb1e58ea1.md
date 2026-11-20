
```
    Also, what TVs do you have?""",

     'ideal_answer':{

        'Smartphones and Accessories':set(

            ['SmartX ProPhone']),

        'Cameras and Camcorders':set(

            ['FotoSnap DSLR Camera']),

        'Televisions and Home Theater Systems':set(

            ['CineView 4K TV', 'SoundMax Home Theater','CineView 8K TV', 'SoundMax Soundbar', 'CineView OLED TV'])

        }

    }, 

    # eg 4

    {'customer_msg':"""tell me about the CineView TV, the 8K one, Gamesphere console, the X one.

I'm on a budget, what computers do you have?""",

     'ideal_answer':{

        'Televisions and Home Theater Systems':set(

            ['CineView 8K TV']),

        'Gaming Consoles and Accessories':set(

            ['GameSphere X']),

        'Computers and Laptops':set(

            ['TechPro Ultrabook', 'BlueWave Gaming Laptop', 'PowerLite Convertible', 'TechPro Desktop', 'BlueWave Chromebook'])

        }

    },

    # eg 5

    {'customer_msg':f"""What smartphones do you have?""",

     'ideal_answer':{

           'Smartphones and Accessories':set(

               ['SmartX ProPhone', 'MobiTech PowerCase', 'SmartX MiniPhone', 'MobiTech Wireless Charger', 'SmartX EarBuds'

               ])

                    }

    },

    # eg 6

    {'customer_msg':f"""I'm on a budget.  Can you recommend some smartphones to me?""",

     'ideal_answer':{

        'Smartphones and Accessories':set(

            ['SmartX EarBuds', 'SmartX MiniPhone', 'MobiTech PowerCase', 'SmartX ProPhone', 'MobiTech Wireless Charger']

        )}

    },

    # eg 7 # this will output a subset of the ideal answer

    {'customer_msg':f"""What Gaming consoles would be good for my friend who is into racing games?""",

     'ideal_answer':{

        'Gaming Consoles and Accessories':set([

            'GameSphere X',

            'ProGamer Controller',

            'GameSphere Y',

            'ProGamer Racing Wheel',

            'GameSphere VR Headset'

     ])}

    },

    # eg 8

    {'customer_msg':f"""What could be a good present for my videographer friend?""",

     'ideal_answer': {

        'Cameras and Camcorders':set([

        'FotoSnap DSLR Camera', 'ActionCam 4K', 'FotoSnap Mirrorless Camera', 'ZoomMaster Camcorder', 'FotoSnap Instant Camera'

        ])}

    },

    # eg 9

    {'customer_msg':f"""I would like a hot tub time machine.""",

     'ideal_answer': []

    }

]
```

可以打印出来中间结果

```
print(f'Customer message: {msg_ideal_pairs_set[7]["customer_msg"]}')
print(f'Ideal answer: {msg_ideal_pairs_set[7]["ideal_answer"]}')
"""
Customer message: What Gaming consoles would be good for my friend who is into racing games?
Ideal answer: {'Gaming Consoles and Accessories': {'ProGamer Controller', 'ProGamer Racing Wheel', 'GameSphere VR Headset', 'GameSphere Y', 'GameSphere X'}}
"""
```

接着，我们写一段自动化评估脚本，这段脚本会做三件事

1. 将模型返回的回复输出预处理，并解析成python对象
2. 比较ideal的和返回的输出是否一致，并统计正确率
3. 如果不正确，打印输出结果

```
import json

def eval_response_with_ideal(response,
                              ideal,
                              debug=False):

    if debug:
        print("response")
        print(response)

    # json.loads() expects double quotes, not single quotes
    json_like_str = response.replace("'", '"')
    # parse into a list of dictionaries
    l_of_d = json.loads(json_like_str)

    # special case when response is empty list
    if l_of_d == [] and ideal == []:
        return 1
    # otherwise, response is empty 
    # or ideal should be empty, there's a mismatch
    elif l_of_d == [] or ideal == []:
        return 0

    correct = 0    

    if debug:
        print("l_of_d is")
        print(l_of_d)

    for d in l_of_d:
        cat = d.get('category')
        prod_l = d.get('products')
        if cat and prod_l:
            # convert list to set for comparison
            prod_set = set(prod_l)
            # get ideal set of products
            ideal_cat = ideal.get(cat)
            if ideal_cat:
                prod_set_ideal = set(ideal.get(cat))
            else:
                if debug:
                    print(f"did not find category {cat} in ideal")
                    print(f"ideal: {ideal}")
                continue

            if debug:
                print("prod_set\n",prod_set)
                print()
                print("prod_set_ideal\n",prod_set_ideal)

            if prod_set == prod_set_ideal:
                if debug:
                    print("correct")
                correct +=1
            else:
                print("incorrect")
                print(f"prod_set: {prod_set}")
                print(f"prod_set_ideal: {prod_set_ideal}")
                if prod_set <= prod_set_ideal:
                    print("response is a subset of the ideal answer")
                elif prod_set >= prod_set_ideal:
                    print("response is a superset of the ideal answer")

    # count correct over total number of items in list
    pc_correct = correct / len(l_of_d)
    return pc_correct
```

接着，我们就可以批量地评估每个样本准确率

```
# Note, this will not work if any of the api calls time out
score_accum = 0
for i, pair in enumerate(msg_ideal_pairs_set):
    print(f"example {i}")
    customer_msg = pair['customer_msg']
    ideal = pair['ideal_answer']
    # print("Customer message",customer_msg)
    # print("ideal:",ideal)
    response = find_category_and_product_v2(customer_msg,
                                                      products_and_category)
    # print("products_by_category",products_by_category)
    score = eval_response_with_ideal(response,ideal,debug=False)
    print(f"{i}: {score}")
    score_accum += score

n_examples = len(msg_ideal_pairs_set)
fraction_correct = score_accum / n_examples
print(f"Fraction correct out of {n_examples}: {fraction_correct}")

"""
example 0
0: 1.0
example 1
1: 1.0
example 2
2: 1.0
example 3
3: 1.0
example 4
4: 1.0
example 5
5: 1.0
example 6
6: 1.0
example 7
incorrect
prod_set: {'GameSphere VR Headset', 'ProGamer Controller', 'ProGamer Racing Wheel'}
prod_set_ideal: {'ProGamer Racing Wheel', 'GameSphere VR Headset', 'ProGamer Controller', 'GameSphere Y', 'GameSphere X'}
response is a subset of the ideal answer
7: 0.0
example 8
8: 1.0
example 9
9: 1
Fraction correct out of 10: 0.9
"""
```

最终我们在10个测试用例上，得到了90%的效果，只用了2-shot examples，效果非常好了。

## 答案不唯一正确时评估

上面的例子是分类，有且只有一个正确类别和列表产品。

但当LLM被用来生成文本时，就不再只有一个正确的文本，而是有很多可能的好答案。那这时要如何评估这种类型的输出呢？

一种方法是编写一组评价指导原则或标准，也就是一套用来从不同维度评价这个答案的指导原则。根据这些原则对LLM的输出进行评估。这包括检查LLM的回答是否仅基于提供的上下文，答案是否包含在上下文中没有提供的信息，以及回答和上下文之间是否存在分歧等。

另一种方法是使用专家提供的理想答案。如果能获取专家认可的理想答案，可以让LLM对比其自身生成的回答和专家的理想答案的相似度，以此来评估LM的表现。

我们先让LLM针对一个case生成结果。
