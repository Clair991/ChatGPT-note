## 配置环境
### 1.下载openai
在`jupyter notebook`上安装
~~~python
!pip install openai 
~~~
### 2.导入openai，并配置openai API key
~~~python
import openai
import os

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv())

openai.api_key = 'sk- '
~~~
### 3.辅助函数
调用openai接口
~~~python
def get_completion(prompt, model="gpt-3.5-turbo"):
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=0, # this is the degree of randomness of the model's output
    )
    return response.choices[0].message["content"]
~~~

## Guidelines
### 准则一：写出明确而具体的说明
#### 方法1:使用分隔符清楚地表示输入的不同部分
分隔符可以是
~~~
 ```,  """,  < >,  <tag> </tag>,  :   
~~~
它可以防止prompt注入，以免给模型产生混乱的理解
例子：
~~~python
text = f"""
You should express what you want a model to do by \ 
providing instructions that are as clear and \ 
specific as you can possibly make them. \ 
This will guide the model towards the desired output, \ 
and reduce the chances of receiving irrelevant \ 
or incorrect responses. Don't confuse writing a \ 
clear prompt with writing a short prompt. \ 
In many cases, longer prompts provide more clarity \ 
and context for the model, which can lead to \ 
more detailed and relevant outputs.
"""
prompt = f"""
Summarize the text delimited by triple backticks \ 
into a single sentence.
```{text}```
"""
response = get_completion(prompt)
print(response)
~~~
结果为打印一句话总结的text的结果：
<img width="1120" alt="截屏2023-05-25 20 25 54" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/e58aaa96-0173-460c-90ef-287c4395b8fa">
#### 方法2:用结构化输出
如直接要求它以HTML或者JSON格式输出
例子：
~~~python
prompt = f"""
Generate a list of three made-up book titles along \ 
with their authors and genres. 
Provide them in JSON format with the following keys: 
book_id, title, author, genre.
"""
response = get_completion(prompt)
print(response)
~~~
代码要求生成三个虚拟的图书，以JSON格式输出，结果如下：
<img width="1120" alt="截屏2023-05-25 20 29 26" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/d424a441-74a1-4a8b-b4c5-aa21456037e8">
#### 方法3:请模型检查是否满足条件
要求模型先检查是否满足某个条件后，再进行输出，如果条件不满足可以直接告知。
例子：
~~~python
text_1 = f"""
Making a cup of tea is easy! First, you need to get some \ 
water boiling. While that's happening, \ 
grab a cup and put a tea bag in it. Once the water is \ 
hot enough, just pour it over the tea bag. \ 
Let it sit for a bit so the tea can steep. After a \ 
few minutes, take out the tea bag. If you \ 
like, you can add some sugar or milk to taste. \ 
And that's it! You've got yourself a delicious \ 
cup of tea to enjoy.
"""
prompt = f"""
You will be provided with text delimited by triple quotes. 
If it contains a sequence of instructions, \ 
re-write those instructions in the following format:

Step 1 - ...
Step 2 - …
…
Step N - …

If the text does not contain a sequence of instructions, \ 
then simply write \"No steps provided.\"

\"\"\"{text_1}\"\"\"
"""
response = get_completion(prompt)
print("Completion for Text 1:")
print(response)
~~~
代码中，text的内容是泡茶的步骤，prompt要求模型理解这段内容，告知是否能把它分解成一步一步的结构，如果能，则按照步骤重新描述，如果不能则返回`No steps provided.`。结果如下：
<img width="1120" alt="截屏2023-05-25 20 34 19" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/91aa314a-ff5a-496c-9266-4fd4c6d242ff">
不能分解步骤的例子：
~~~python
text_2 = f"""
The sun is shining brightly today, and the birds are \
singing. It's a beautiful day to go for a \ 
walk in the park. The flowers are blooming, and the \ 
trees are swaying gently in the breeze. People \ 
are out and about, enjoying the lovely weather. \ 
Some are having picnics, while others are playing \ 
games or simply relaxing on the grass. It's a \ 
perfect day to spend time outdoors and appreciate the \ 
beauty of nature.
"""
prompt = f"""
You will be provided with text delimited by triple quotes. 
If it contains a sequence of instructions, \ 
re-write those instructions in the following format:

Step 1 - ...
Step 2 - …
…
Step N - …

If the text does not contain a sequence of instructions, \ 
then simply write \"No steps provided.\"

\"\"\"{text_2}\"\"\"
"""
response = get_completion(prompt)
print("Completion for Text 2:")
print(response)
~~~
<img width="1120" alt="截屏2023-05-25 20 35 19" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/5c7a0f86-4b11-4995-9551-22e5e37877c6">

#### 方法4:Prompt中包含少量样本
例子：
~~~python
prompt = f"""
Your task is to answer in a consistent style.

<child>: Teach me about patience.

<grandparent>: The river that carves the deepest \ 
valley flows from a modest spring; the \ 
grandest symphony originates from a single note; \ 
the most intricate tapestry begins with a solitary thread.

<child>: Teach me about resilience.
"""
response = get_completion(prompt)
print(response)
~~~
代码中是一个child和grandparent对话的样本，要求再次按照这个样本给出grandparent地答复。结果如下：
<img width="1120" alt="截屏2023-05-25 20 40 55" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/7d450e4a-6a7a-45fe-8521-4f17ae46f27f">

### 准则二：给模型一些思考的时间
#### 方法1:指定完成任务所需要的步骤
例子：
~~~python
text = f"""
In a charming village, siblings Jack and Jill set out on \ 
a quest to fetch water from a hilltop \ 
well. As they climbed, singing joyfully, misfortune \ 
struck—Jack tripped on a stone and tumbled \ 
down the hill, with Jill following suit. \ 
Though slightly battered, the pair returned home to \ 
comforting embraces. Despite the mishap, \ 
their adventurous spirits remained undimmed, and they \ 
continued exploring with delight.
"""

prompt_1 = f"""
Perform the following actions: 
1 - Summarize the following text delimited by triple \
backticks with 1 sentence.
2 - Translate the summary into French.
3 - List each name in the French summary.
4 - Output a json object that contains the following \
keys: french_summary, num_names.

Separate your answers with line breaks.

Text:
```{text}```
"""

response = get_completion(prompt_1)
print("Completion for prompt 1:")
print(response)
~~~
代码中，prompt给出模型要执行任务的步骤：
步骤1，用一句话总结text内容
步骤2，翻译成法语
步骤3，列出名字
步骤4，以JSON格式输出
执行结果：
<img width="1120" alt="截屏2023-05-25 20 48 45" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/4d3a0276-8528-4a1d-bd68-308aab2f3f6a">

~~~python
prompt_2 = f"""
Your task is to perform the following actions: 
1 - Summarize the following text delimited by 
  <> with 1 sentence.
2 - Translate the summary into French.
3 - List each name in the French summary.
4 - Output a json object that contains the 
  following keys: french_summary, num_names.

Use the following format:
Text: <text to summarize>
Summary: <summary>
Translation: <summary translation>
Names: <list of names in Italian summary>
Output JSON: <json with summary and num_names>

Text: <{text}>
"""
response = get_completion(prompt_2)
print("\nCompletion for prompt 2:")
print(response)
~~~
<img width="1120" alt="截屏2023-05-25 20 51 16" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/5abfd6b5-1b9d-4a81-b3a3-fc6cb0cc069d">

#### 方法2:指示模型在匆忙得出结论之前制定出自己的解决方案
~~~python
prompt = f"""
Determine if the student's solution is correct or not.

Question:
I'm building a solar power installation and I need \
 help working out the financials. 
- Land costs $100 / square foot
- I can buy solar panels for $250 / square foot
- I negotiated a contract for maintenance that will cost \ 
me a flat $100k per year, and an additional $10 / square \
foot
What is the total cost for the first year of operations 
as a function of the number of square feet.

Student's Solution:
Let x be the size of the installation in square feet.
Costs:
1. Land cost: 100x
2. Solar panel cost: 250x
3. Maintenance cost: 100,000 + 100x
Total cost: 100x + 250x + 100,000 + 100x = 450x + 100,000
"""
response = get_completion(prompt)
print(response)
~~~
代码要求模型判断学生的解体是否正确，执行结果：
<img width="1120" alt="截屏2023-05-25 20 53 55" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/365a7cd0-9197-4e57-9450-44ccf513eccb">
第三步中结果明显错误，但模型中判断了`Total cost: 100x + 250x + 100,000 + 100x = 450x + 100,000`是正确的。
下面的例子要求模型先自己按照步骤给出解体的步骤，再判断学生的解体步骤是否正确：
~~~python
prompt = f"""
Your task is to determine if the student's solution \
is correct or not.
To solve the problem do the following:
- First, work out your own solution to the problem. 
- Then compare your solution to the student's solution \ 
and evaluate if the student's solution is correct or not. 
Don't decide if the student's solution is correct until 
you have done the problem yourself.

Use the following format:
Question:
```
question here
```
Student's solution:
```
student's solution here
```
Actual solution:
```
steps to work out the solution and your solution here
```
Is the student's solution the same as actual solution \
just calculated:
```
yes or no
```
Student grade:
```
correct or incorrect
```

Question:
```
I'm building a solar power installation and I need help \
working out the financials. 
- Land costs $100 / square foot
- I can buy solar panels for $250 / square foot
- I negotiated a contract for maintenance that will cost \
me a flat $100k per year, and an additional $10 / square \
foot
What is the total cost for the first year of operations \
as a function of the number of square feet.
```
Student's solution:
```
Let x be the size of the installation in square feet.
Costs:
1. Land cost: 100x
2. Solar panel cost: 250x
3. Maintenance cost: 100,000 + 100x
Total cost: 100x + 250x + 100,000 + 100x = 450x + 100,000
```
Actual solution:
"""
response = get_completion(prompt)
print(response)
~~~
执行结果：
<img width="1120" alt="截屏2023-05-25 20 56 52" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/6a2cf2b0-4ca6-4b90-8b4c-236ab45b8a7d">

## 模型的限制
~~~python
prompt = f"""
Tell me about AeroGlide UltraSlim Smart Toothbrush by Boie
"""
response = get_completion(prompt)
print(response)
~~~
要求模型介绍Boie公司的电动牙刷，公司和产品都不存在，但模型生成了介绍：
<img width="1120" alt="截屏2023-05-25 21 00 53" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/85e2a182-a831-4db0-a083-b4f59754bd4c">
这种模型的限制称为模型的幻觉。
要减少这种幻觉，需要模型先从文本中找到任何相关的引用，然后请它使用这些引用来回答问题，并且吧回答追溯到源文件。









