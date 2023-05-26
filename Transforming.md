### 1.翻译 Translation
~~~python
prompt = f"""
Translate the following English text to Spanish: \ 
```Hi, I would like to order a blender```
"""
response = get_completion(prompt)
print(response)
~~~
`prompt`：把 “Hi, I would like to order a blender” 翻译成西班牙语
执行结果：
<img width="864" alt="截屏2023-05-26 18 11 20" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/3ee1c8ef-2f87-418d-9210-0827f9226947">

~~~python 
prompt = f"""
Tell me which language this is: 
```Combien coûte le lampadaire?```
"""
response = get_completion(prompt)
print(response)
~~~
`prompt`：告诉我句子 “Combien coûte le lampadaire? ” 是什么语言
执行结果：
<img width="864" alt="截屏2023-05-26 18 12 11" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/b4f8858a-1ee3-498e-a3c9-256d6e3a1cea">

~~~python
user_messages = [
  "La performance du système est plus lente que d'habitude.",  # System performance is slower than normal         
  "Mi monitor tiene píxeles que no se iluminan.",              # My monitor has pixels that are not lighting
  "Il mio mouse non funziona",                                 # My mouse is not working
  "Mój klawisz Ctrl jest zepsuty",                             # My keyboard has a broken control key
  "我的屏幕在闪烁"                                               # My screen is flashing
] 

for issue in user_messages:
    prompt = f"Tell me what language this is: ```{issue}```"
    lang = get_completion(prompt)
    print(f"Original message ({lang}): {issue}")

    prompt = f"""
    Translate the following  text to English \
    and Korean: ```{issue}```
    """
    response = get_completion(prompt)
    print(response, "\n")
~~~
`prompt`：告诉我这句话是什么语言，并把它翻译成英文和韩文
执行结果：
<img width="864" alt="截屏2023-05-26 18 14 10" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/d5fc0d47-07bf-45ce-8547-fd3256cf3245">

### 2.语气转换 Tone Transformation
指定正式还是非正式的语气，指定语言使用的场合比如商务场合的邮件
~~~python
prompt = f"""
Translate the following from slang to a business letter: 
'Dude, This is Joe, check out this spec on this standing lamp.'
"""
response = get_completion(prompt)
print(response)
~~~
`prompt`：将以下俚语翻译成商务信函
执行结果：
<img width="864" alt="截屏2023-05-26 18 18 18" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/e5333758-8ac8-4811-ad27-28c46ebf21e6">

### 3.格式转换 Format Conversion
~~~python
data_json = { "resturant employees" :[ 
    {"name":"Shyam", "email":"shyamjaiswal@gmail.com"},
    {"name":"Bob", "email":"bob32@gmail.com"},
    {"name":"Jai", "email":"jai87@gmail.com"}
]}

prompt = f"""
Translate the following python dictionary from JSON to an HTML \
table with column headers and title: {data_json}
"""
response = get_completion(prompt)
print(response)
~~~
`prompt`：把JSON转换为HTML表格
执行结果：
<img width="864" alt="截屏2023-05-26 18 20 16" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/612205ca-5652-42a7-9230-384304d39075">
~~~python
from IPython.display import display, Markdown, Latex, HTML, JSON
display(HTML(response))
~~~
<img width="864" alt="截屏2023-05-26 18 21 09" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/a1fcb14e-d406-446f-bd78-033305f8b363">

### 4.拼写或语法检查 Spellcheck/Grammar check
~~~python
text = [ 
  "The girl with the black and white puppies have a ball.",  # The girl has a ball.
  "Yolanda has her notebook.", # ok
  "Its going to be a long day. Does the car need it’s oil changed?",  # Homonyms
  "Their goes my freedom. There going to bring they’re suitcases.",  # Homonyms
  "Your going to need you’re notebook.",  # Homonyms
  "That medicine effects my ability to sleep. Have you heard of the butterfly affect?", # Homonyms
  "This phrase is to cherck chatGPT for speling abilitty"  # spelling
]
for t in text:
    prompt = f"""Proofread and correct the following text
    and rewrite the corrected version. If you don't find
    and errors, just say "No errors found". Don't use 
    any punctuation around the text:
    ```{t}```"""
    response = get_completion(prompt)
    print(response)
~~~
`prompt`：校对并改正下面的课文，改写改正后的版本，如果找不到错误，只需说"No errors found"。不要在正文周围使用任何标点符号。
执行结果：
<img width="864" alt="截屏2023-05-26 18 22 43" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/9c6c959d-cf5b-4794-95c7-4da7d9ea7fbc">

~~~python
text = f"""
Got this for my daughter for her birthday cuz she keeps taking \
mine from my room.  Yes, adults also like pandas too.  She takes \
it everywhere with her, and it's super soft and cute.  One of the \
ears is a bit lower than the other, and I don't think that was \
designed to be asymmetrical. It's a bit small for what I paid for it \
though. I think there might be other options that are bigger for \
the same price.  It arrived a day earlier than expected, so I got \
to play with it myself before I gave it to my daughter.
"""
prompt = f"proofread and correct this review: ```{text}```"
response = get_completion(prompt)
print(response)
~~~
`prompt`：校对并更正这篇评论
执行结果：
<img width="864" alt="截屏2023-05-26 18 23 50" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/de7ca84d-43fc-4da5-96b9-ea5e6b274484">

在原文上标注修改的部分：
<img width="864" alt="截屏2023-05-26 18 24 32" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/67a5c531-693e-42b6-a04d-442ce0201430">

进一步改写：
~~~python
prompt = f"""
proofread and correct this review. Make it more compelling. 
Ensure it follows APA style guide and targets an advanced reader. 
Output in markdown format.
Text: ```{text}```
"""
response = get_completion(prompt)
display(Markdown(response))
~~~
`prompt`：校对并更正这篇评论。让它更有说服力。确保它遵循APA风格指南，并针对高级读者。以markdown格式输出。
执行结果：
<img width="864" alt="截屏2023-05-26 18 26 18" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/2756a60b-e707-47e8-8b2c-298b1da53d4b">


