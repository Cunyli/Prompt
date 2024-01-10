
## Sort

![image.png](https://raw.githubusercontent.com/Cunyli/dataAnalysis/master/202310301953977.png)

## principle 
> 1 write clear and specific instructions

- Tactic 1: **Use delimiters**
	- triple quotes ``"""``
	- triple backtics: `` ``` ``
	- triple dashes: ``---``
	- angle brackets: ``<>``
	- XML tags: `<tag></tag>`
- Tactic 2: **Ask for structured output**
	- HTML
	- JSON
- Tactic 3: **Check whether conditions are satisfied**
	- Check assumptions required to do the task
- Tactic 4: **Few-shot prompt**
	- Give successful examples of completing tasks
	- Then ask model to perform the task

## principle 2
> Give the model time to think

- Tactic 1: Specify the steps to complete a task
	- Step 1: ...
	- Step 2:...
- Tactic 2: Instruct the model to work out its own solution before rushing to a conclusion

## Model Limitation
> Hallucination
- Makes statements that sound plausible but are not true

**To reduce hallucinations:**
- First find relevant information, 
- Then answer the question based on the relevant information

----
## Iterative

简而言之就是一步一步的问，然后根据回答不断的修改问题，注意是将修改完的整个问题扔进去而只是加增加的部分

----
## Summarizing

带有重点的让他总结，比如以下prompt

```python
prompt = f"""
Your task is to generate a short summary of a product \
review from an ecommerce site to give feedback to the \
Shipping deparmtment. 

Summarize the review below, delimited by <>, in at most 30 words, and focusing on any aspects that mention shipping and delivery of the product. 

Review: <{prod_review}>
"""

response = get_completion(prompt)
print(response)```

或者让他专注在其他部分

```python
prompt = f"""
Your task is to generate a short summary of a product \
review from an ecommerce site to give feedback to the \
pricing deparmtment, responsible for determining the \
price of the product.  

Summarize the review below, delimited by triple 
backticks, in at most 30 words, and focusing on any aspects \
that are relevant to the price and perceived value. 

Review: {prod_review}
"""
```

其次，`extract`不同于`summarize`，总结出来的比提取出来的会更加的臃肿，多一些不是最重点的信息，所以善用extract会更好的完成特定的任务

----
## 补充

### 明确指示
尽可能地减少解释空间，限制 AI 的操作范围。比如明确列出你希望 AI 帮你总结的字段，明确需要注意的细节，明确输出的格式。比如：

> 请使用小红书流行的文案风格概括总结这篇文章。小红书的风格，特点是：
> 
> 1. 引人入胜的标题；  
> 2. 每个段落中包含表情符号；  
> 3. 在末尾添加相关标签；
> 
> 请确保原文的意思保持不变。

### 把任务分解
把一个大的问题拆解为细分步骤，每个逻辑步骤清晰，告诉 AI 第一步做什么，第二步接着做什么。比如：

> 选中的表格数据，是 A 公司近期的股票数据。请你根据选中的数据完成以下两个任务：
> 
> 1. 请对 A 公司近期的股票数据进行点评 。  
> 2. 根据收盘价计算每日收益率，并以表格的形式输出。

## 定义角色
告诉 AI 它现在需要以一个什么样的身份来回答问题。比如：

> 你现在是一名帮助我回答税收相关问题的助理。回答问题时，请遵循以下要求：
> 
> 1. 只回答跟税收相关的问题。  
> 2. 如果你不确定问题的答案，请回答「我不确定」。

### 明确输出的格式
你可以让 AI 输出表格、大纲。比如：

> 请使用大纲列表 outliner 的形式，依次告诉我这篇文章的引言、文献回顾、理论框架、创新点、研究方法、研究过程、主要观点和结论、进一步研究方向。
> 
> 生成的回答格式如下，以此类推。每个部分的回答，要细分为多个子内容。- 引言 - 此处为引言内容。- 子内容 - 子内容 - 文献回顾 - 此处为文献回顾内容 - 子内容 - 子内容。

也可以让它输出评分、解释。比如：

> 你是一名助手，旨在分析语音数据中的情绪。选中的内容是用户 A 的语音数据。评分范围为 1-10（10 为最高），请给出评分，并解释为什么给出这个评级。

### 把指令放到提示词开始
在最开始时，告诉 AI 你希望它执行的任务，然后再共享其他上下文信息或示例，可以帮助产生更高质量的输出。

### 最后一遍重复指令
模型容易受到最新偏差的影响，在这种情况下，末尾 Prompt 信息可能比开头 Prompt 信息对输出的影响更大。因此，当提示语比较长的时候，在 Prompt 末尾重复指令，是值得一试的方法。
