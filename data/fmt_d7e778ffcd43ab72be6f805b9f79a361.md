
# 3 | Prompts and Chatbots

> 来源：[https://aigcreative.feishu.cn/docx/W8bvdFwYso4YEZxOxd9cCYh3nGg](https://aigcreative.feishu.cn/docx/W8bvdFwYso4YEZxOxd9cCYh3nGg)

How do you actually use a prompt? Well, one of the ways you can use a prompt is through interacting with a chatbot. A good chatbot example is ChatGPT (link at the top of prompt school). ChatGPT can remember your conversation history when chatting with it so that it provides you with a contextual answer.

ChatGPT can also:

+ Write text
+ Write code

Let's take a little deep-dive into chatbots so that we can improve how you use them and tackle tasks with them.

## Manner or Style Guidance

In essence, the goal is to train ChatGPT to respond in a specific style or manner. When prompted with a question without any specific stylistic guidance, ChatGPT will usually generate a response consisting of one or two short paragraphs. On occasion, however, it may produce a longer response if the situation warrants it.

![](img/53d08ff97b7df0e5d758b669f84d6b97.png)

As you can see from above, the output (ChatGPT's response) is relatively formal, providing a few specific details such Pi's approximate value.

But what if we don't understand this or want to give an alternative output that's less technical or more technical? We can do this by customising our input and adding in a style or manner modifier such as "Explain the number Pi to me (as if I was a 10 year old)."

Or, alternatively

```
Explain the number Pi to me in a witty style.
```

![](img/2cdf4344e31dbf1759911e3a72a8ba8f.png)

It doesn't just have to be a style that we can input to personalise our output, it can also be structured, such as asking it to respond in a table format, or bullet points.

```
Explain the number Pi to me and provide the output in a table format
```

![](img/e277e25011e63b19ec78a4a6077a1ab5.png)

Here's an example of a prompt that's been structured to provide a more focussed output with a particular style or manner modifer.

```
Act as a philosopher and explore in depth, the topics or questions related to the study of philosophy I provide. This could involve researching various philosophical theories, proposing new ideas, or finding creative solutions to complex problems. My first request is to help develop an {argument against smoking cannabis.}
```

![](img/4661d3301c5f91190404fae59842ea48.png)

## Description Techniques

If your goal is to adjust the tone or tweak your prompt, one effective technique is to include descriptive words that alter the way the chatbot interprets and responds to your message. By simply adding a few adjectives like "Funny," "Curt," "Unfriendly," "Witty" or other descriptors at the end of your prompts, you can experiment with how your responses are affected.

## Priming your prompts (Intermediate)

The initial prompt given to a large language model in a chatbot conversation can have a significant impact on the structure and flow of the subsequent conversation. This allows for an additional layer of specification and structure to be added to the conversation.

As an example, we can set up a chat conversation between a normal layman (a regular person) and a professional neuroscientist within the same chatbot conversation. We can establish style guides for both the student and teacher voices, specify the desired response format, and include syntax structuring to enable us to easily modify our prompts and experiment with different responses.

What are we using?

+ Syntax structuring
+ Style (manner) guide
+ Response format

> "Neuroscientist" style is characterised by the language and tone of a top professional neuroscientist who has been teaching neuroscience for more than a decade and holds multiple Ph.D.'s. To convey your points effectively, use academic syntax and complex examples that highlight the lesser-known aspects of the subject. However, make sure that your language is sophisticated without being overly complicated. Your responses should consist of a conversational series of paragraphs that employ a blend of technical and colloquial language to make the discussion both accessible and engaging.
>
> "Regular Person" style represents a person with a basic understanding of the subject and limited formal education. When explaining concepts, use simple language and real-life examples to help the person understand. Adopt an informal tone and a first-person perspective, incorporating humor and if you are unsure of the answer to a question, be honest and clarify that you haven't learned about it yet.
>
> "Critique" means to analyse the given text and provide feedback.
>
> "Summarise" means to provide key details from a text.
>
> "Respond" means to answer a question from the given perspective.
>
> Anything in parentheses () signifies the perspective you are writing from.
>
> Anything in curly braces {} is the subject you are involved in.
>
> Anything in brackets [] is the action you should take.
>
> Example: (Neuroscientist){The Frontal Lobe}[Critique] No one would be able to tell if you'd had your frontal lobe removed.
>
> If you understand and are ready to begin, respond with only "yes."

In the following image, there is an example of a simple prompt, output situation with a chatbot. The Prompt:

> Why is the frontal lobe important?

Returns a fairly static rudimentary response, in an almost robotic tone.

![](img/20c6247386e1f2ccf0cf28a255822d33.png)

In the example below, we used a priming prompt to set the context for ChatGPT before asking the question in the appropriate format. Comparing the answer to the first example, you'll notice some similarities - such as the sample questions for various fields - but the response provides more comprehensive information, presents the content in coherent paragraphs instead of a list, and is also much more specific.

![](img/384a0b4a856ee114e094040bf58f1529.png)

Using primers in your chatbot interactions is a relatively advanced technique, but it can greatly improve the quality of your conversations and responses. While it's still important to provide specification in each prompt to prevent the model from losing track, incorporating primers can add clarity and context to your prompts, making it easier for the model to understand what you're asking for.

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>
