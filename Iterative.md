## 第一次写Prompt
例子：
~~~python
fact_sheet_chair = """
OVERVIEW
- Part of a beautiful family of mid-century inspired office furniture, 
including filing cabinets, desks, bookcases, meeting tables, and more.
- Several options of shell color and base finishes.
- Available with plastic back and front upholstery (SWC-100) 
or full upholstery (SWC-110) in 10 fabric and 6 leather options.
- Base finish options are: stainless steel, matte black, 
gloss white, or chrome.
- Chair is available with or without armrests.
- Suitable for home or business settings.
- Qualified for contract use.

CONSTRUCTION
- 5-wheel plastic coated aluminum base.
- Pneumatic chair adjust for easy raise/lower action.

DIMENSIONS
- WIDTH 53 CM | 20.87”
- DEPTH 51 CM | 20.08”
- HEIGHT 80 CM | 31.50”
- SEAT HEIGHT 44 CM | 17.32”
- SEAT DEPTH 41 CM | 16.14”

OPTIONS
- Soft or hard-floor caster options.
- Two choices of seat foam densities: 
 medium (1.8 lb/ft3) or high (2.8 lb/ft3)
- Armless or 8 position PU armrests 

MATERIALS
SHELL BASE GLIDER
- Cast Aluminum with modified nylon PA6/PA66 coating.
- Shell thickness: 10 mm.
SEAT
- HD36 foam

COUNTRY OF ORIGIN
- Italy
"""
~~~
~~~python
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
~~~
代码提供了一段椅子的描述文字，要求写一个文案放到网上，通过简单的Prompt描述，执行结果：
<img width="883" alt="截屏2023-05-25 21 45 25" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/86010608-f671-44c1-8358-3875992f37d3">

## 第二次写Prompt
第一次的执行结果，模型给出的文案描述不够简洁，下面再添加一个限制字数的条件：
~~~python
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
~~~
代码中添加了最多50个单词的条件，执行结果：
<img width="883" alt="截屏2023-05-25 21 48 14" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/c72fc0f7-a010-4f0a-a766-7f6253a2990c">

## 第三次写Prompt
要求专注于与目标受众相关的方面，本次添加的文案是面向经销商的，此群体更注重一些材料参数等细节。
~~~python
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
~~~
执行结果：
<img width="883" alt="截屏2023-05-25 21 50 20" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/9095a802-b0a5-41d4-a161-e00f841ade16">

## 第四次写Prompt
添加一些ID的说明：
~~~python
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
~~~
执行结果：
<img width="883" alt="截屏2023-05-25 21 52 05" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/08a6115e-3521-46d3-9293-dd9227ac9824">

## 第五次写Prompt
以表格的形式展示一些材料等数据，并输出为html格式：
~~~python
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
In the second column include the measurements in inches only.

Give the table the title 'Product Dimensions'.

Format everything as HTML that can be used in a website. 
Place the description in a <div> element.

Technical specifications: ```{fact_sheet_chair}```
"""

response = get_completion(prompt)
print(response)
~~~
执行结果：
![IMG_6435](https://github.com/Clair991/ChatGPT-note/assets/102845425/6b85f841-5e93-43ed-afa2-264f738a8186)
将输出的html渲染出的效果：
~~~python
from IPython.display import display, HTML
display(HTML(response))
~~~
![IMG_6436](https://github.com/Clair991/ChatGPT-note/assets/102845425/035bcbc2-3e91-4f84-8a63-0b83929cbfb0)

## 总结
<img width="600" alt="截屏2023-05-25 22 10 16" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/50a482c4-e5f6-4b1a-849e-9e5e7caaf006">

编写Prompt时，要对需要完成的任务有一个想法，然后尝试第一次编写一个Prompt，迭代过程时找出指令不清晰的原因，或为什么它没有给算法足够的时间来思考，允许完善想法和提示等。多次循环，知道得到合适的Prompt。








