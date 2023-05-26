### 1.推断情绪（正面/负面）
~~~python
lamp_review = """
Needed a nice lamp for my bedroom, and this one had \
additional storage and not too high of a price point. \
Got it fast.  The string to our lamp broke during the \
transit and the company happily sent over a new one. \
Came within a few days as well. It was easy to put \
together.  I had a missing part, so I contacted their \
support and they very quickly got me the missing piece! \
Lumina seems to me to be a great company that cares \
about their customers and products!!
"""
prompt = f"""
What is the sentiment of the following product review, 
which is delimited with triple backticks?

Review text: '''{lamp_review}'''
"""
response = get_completion(prompt)
print(response)
~~~
`lamp_review`的中文意思是：我的卧室需要一盏漂亮的台灯，而这盏灯有额外的存储空间，而且价格也不是太高。搞得很快。我们台灯的线在运输途中断了，公司很高兴地送来了一条新的。也是在几天内到来的。它很容易组合在一起。我有一个缺失的部分，所以我联系了他们的支持，他们很快就给我找到了缺失的部分！在我看来，Lumina是一家关心客户和产品的伟大公司！
`prompt`: 在三个反引号（```）中的产品的评价表达出的情绪是什么样的？
执行结果：
<img width="864" alt="截屏2023-05-26 17 34 50" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/4504d53b-3f11-401e-af94-eaeb9bb4ab91">

### 2.确定情绪的类型
~~~python
prompt = f"""
Identify a list of emotions that the writer of the \
following review is expressing. Include no more than \
five items in the list. Format your answer as a list of \
lower-case words separated by commas.

Review text: '''{lamp_review}'''
"""
response = get_completion(prompt)
print(response)
~~~
`prompt`： 找出下面评论的作者所表达的一系列情绪，不超过五个。将您的答案格式化为逗号分隔的小写单词列表
执行结果：
<img width="864" alt="截屏2023-05-26 17 36 18" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/d83cd652-a6fd-4acb-8cd1-c1781063c756">

### 3.识别愤怒
~~~python
prompt = f"""
Is the writer of the following review expressing anger?\
The review is delimited with triple backticks. \
Give your answer as either yes or no.

Review text: '''{lamp_review}'''
"""
response = get_completion(prompt)
print(response)
~~~
`prompt`：下面这篇用三个反引号包围着的评论的作者是否表达了愤怒？回答“是”或“不是”。
模型给出了 “不是” 的结果：
<img width="864" alt="截屏2023-05-26 17 37 25" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/0680649b-0b8a-4bc0-94ca-4c31cc9a2dec">

### 4.从客户评论中提取产品和公司名称
~~~python
prompt = f"""
Identify the following items from the review text: 
- Item purchased by reviewer
- Company that made the item

The review is delimited with triple backticks. \
Format your response as a JSON object with \
"Item" and "Brand" as the keys. 
If the information isn't present, use "unknown" \
as the value.
Make your response as short as possible.
  
Review text: '''{lamp_review}'''
"""
response = get_completion(prompt)
print(response)
~~~
`prompt`：从客户的评价中提取产品和公司名，并以JSON格式给出，JSON的key为：Item、Brand
执行结果：
<img width="864" alt="截屏2023-05-26 17 39 09" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/611d4d3c-35f3-4d05-92d9-105b5cea7d49">

### 5.一次完成多项任务
~~~python
prompt = f"""
Identify the following items from the review text: 
- Sentiment (positive or negative)
- Is the reviewer expressing anger? (true or false)
- Item purchased by reviewer
- Company that made the item

The review is delimited with triple backticks. \
Format your response as a JSON object with \
"Sentiment", "Anger", "Item" and "Brand" as the keys.
If the information isn't present, use "unknown" \
as the value.
Make your response as short as possible.
Format the Anger value as a boolean.

Review text: '''{lamp_review}'''
"""
response = get_completion(prompt)
print(response)
~~~
`prompt`：从客户的评价中判断情绪（positive或negative）、是否愤怒（true或false）、产品和公司，并以JSON格式输出，JSON的key为：“Sentiment”, “Anger”, “Item”, “Brand”
执行结果：
<img width="864" alt="截屏2023-05-26 17 40 38" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/67384961-863c-4408-81e9-d373e18bb31e">

### 6.推断主题
~~~python
story = """
In a recent survey conducted by the government, 
public sector employees were asked to rate their level 
of satisfaction with the department they work at. 
The results revealed that NASA was the most popular 
department with a satisfaction rating of 95%.

One NASA employee, John Smith, commented on the findings, 
stating, "I'm not surprised that NASA came out on top. 
It's a great place to work with amazing people and 
incredible opportunities. I'm proud to be a part of 
such an innovative organization."

The results were also welcomed by NASA's management team, 
with Director Tom Johnson stating, "We are thrilled to 
hear that our employees are satisfied with their work at NASA. 
We have a talented and dedicated team who work tirelessly 
to achieve our goals, and it's fantastic to see that their 
hard work is paying off."

The survey also revealed that the 
Social Security Administration had the lowest satisfaction 
rating, with only 45% of employees indicating they were 
satisfied with their job. The government has pledged to 
address the concerns raised by employees in the survey and 
work towards improving job satisfaction across all departments.
"""
~~~
~~~python
prompt = f"""
Determine five topics that are being discussed in the \
following text, which is delimited by triple backticks.

Make each item one or two words long. 

Format your response as a list of items separated by commas.

Text sample: '''{story}'''
"""
response = get_completion(prompt)
print(response)
~~~
`prompt`：提取5个主题，每个主题只能是一个或者两个单次，主题之间使用逗号隔开
执行结果：
<img width="864" alt="截屏2023-05-26 17 42 59" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/4fe611b7-665c-46da-ba7d-666e885ea943">

### 7.主题中是否包含给定的主题
~~~python
topic_list = [
    "nasa", "local government", "engineering", 
    "employee satisfaction", "federal government"
]

prompt = f"""
Determine whether each item in the following list of \
topics is a topic in the text below, which
is delimited with triple backticks.

Give your answer as list with 0 or 1 for each topic.\

List of topics: {", ".join(topic_list)}

Text sample: '''{story}'''
"""
response = get_completion(prompt)
print(response)
~~~
`prompt`：story中提取主题，并判断topic_list中的主题是否有涉及，有的置为1，否则为0
执行结果：
<img width="864" alt="截屏2023-05-26 17 44 45" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/0bf18650-7505-4e79-8859-22d5e3335222">








