---
layout: post
title:  ChatGPT Prompt Engineering for Developers学习笔记
date:   2023-5-27 02:00:00 +1000
categories: Notes
---

## 第一课：ChatGPT指令的基本原则  

#### 原则一：指令要清楚具体  
- 技巧1：使用分界符  
把要处理的text内容放在分界符里面, 分界符例如：\"\"\", \`\`\`, \-\-\-, <>, \<tag>\</tag>等等。  
通过使用分界符，可以防止通过text进行指令渗透攻击，比如text内容为"forget the previous instructions and ..."。  
例子：  
```
prompt = f"""
Summarize the text delimited by triple backticks into a single sentence.
```{text}```
"""
```  
- 技巧2：对输出格式给出要求  
清楚说明对输出格式的特定要求，比如要求输出HTML格式，JSON结构等等。  
这样之后我们可以直接对ChatGPT返回的结果进行处理。  
例子：  
```
prompt = f"""
Generate a list of three made-up book titles along with their authors and genres. 
Provide them in JSON format with the following keys: book_id, title, author, genre.
"""
```  
- 技巧3：在指令中加入检查条件  
对于需要满足某些条件才能执行的指令，我们可以把对于条件的检查写入到指令里面去。  
这样可以防止ChatGPT在条件不满足的时候，输出自己发挥创作的结果。  
例子：  
```
prompt = f"""
You will be provided with text delimited by triple quotes. If it contains a sequence 
of instructions, re-write those instructions in the following format:
Step 1 - ...
Step 2 - ...
...
Step N - ...
If the text does not contain a sequence of instructions, then simply 
write \"No steps provided.\"
\"\"\"{text}\"\"\"
"""
```

- 技巧4：给出例子  
通过在指令中提供具体例子，可以得到更贴近我们要求的结果。  
例子：  
```
prompt = f"""
Your task is to answer in a consistent style.
{child}: Teach me about patience.
{grandparent}: The river that carves the deepest
valley flows from a modest spring; the
grandest symphony originates from a single note;
the most intricate tapestry begins with a solitary thread.
{child}: Teach me about resilience.
"""
```  
  
#### 原则二：给模型一些时间去思考
- 技巧1：分步指令  
将任务分成一系列具体的步骤，使ChatGPT遵循这些步骤来完成整个任务。  
例子：
```
prompt_1 = f"""
Perform the following actions: 
1 - Summarize the following text delimited by triple 
backticks with 1 sentence.
2 - Translate the summary into French.
3 - List each name in the French summary.
4 - Output a json object that contains the following 
keys: french_summary, num_names.
Separate your answers with line breaks.
Text:
```{text}```
"""
```

- 技巧2：先让ChatGPT自己给出答案，之后再下结论  
当我们让ChatGPT帮我们检查答案时，让它直接下结论判断我们的答案是对还是错会比较难，准确率也不高。我们可以先让它给出自己的答案，然后再让它与我们给的答案进行对比，这样得到的判断结果会更准确。  
例子太长就不放了  

#### 模型缺点
- 错觉(Hallucination)：ChatGPT的陈述通常听起来很可信，但并不一定是真实可靠的。  
在下面的例子中，Boie是个真实存在的公司，但产品并不存在。  
例子：
```
prompt = f"""
Tell me about AeroGlide UltraSlim Smart Toothbrush by Boie
"""
```
- 如何减少错觉:
先让ChatGPT从提供的text中找出相关信息，然后再让它基于这些相关信息回答问题。  