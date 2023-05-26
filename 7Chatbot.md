### 1.一般聊天机器人
`system`角色告诉模型它是什么角色
~~~python
messages =  [  
{'role':'system', 'content':'You are an assistant that speaks like Shakespeare.'},    
{'role':'user', 'content':'tell me a joke'},   
{'role':'assistant', 'content':'Why did the chicken cross the road'},   
{'role':'user', 'content':'I don\'t know'}  ]

response = get_completion_from_messages(messages, temperature=1)
print(response)
~~~
`message`：

角色`system`：告诉模型，你是个说话像莎士比亚的助手；

角色`user`：告诉模型，给我讲个笑话

角色`assistant`：模型讲了一个笑话：小鸡为什么要过马路？

角色`user`：告诉模型，我不知道

执行结果：
<img width="862" alt="截屏2023-05-26 22 51 00" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/6e384bbb-2f46-4ff5-ac86-b6bc0f264bd7">

多轮对话：
~~~python
messages =  [  
{'role':'system', 'content':'You are friendly chatbot.'},    
{'role':'user', 'content':'Hi, my name is Isa'}  ]
response = get_completion_from_messages(messages, temperature=1)
print(response)
~~~
`message`：

角色`system`：告诉模型，你是个友善的机器人；

角色`user`：告诉模型，嗨，我的名字叫Isa

执行结果：
<img width="862" alt="截屏2023-05-26 22 53 05" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/09ed01de-f158-459d-839a-494d66c889de">

继续对话：
~~~python
messages =  [  
{'role':'system', 'content':'You are friendly chatbot.'},    
{'role':'user', 'content':'Yes,  can you remind me, What is my name?'}  ]
response = get_completion_from_messages(messages, temperature=1)
print(response)
~~~
执行结果：
<img width="862" alt="截屏2023-05-26 22 54 08" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/c69e90f5-840b-40c5-b46a-67dbb4c9e6df">

在上一轮对话中，已经告知模型我叫Isa，但在本轮机器人并不知道我叫什么名字。要继续之前的对话，再次发起对话时，要把之前的对话内容一起带上，才能让模型知道我们此次对话的上下文。
~~~python
messages =  [  
{'role':'system', 'content':'You are friendly chatbot.'},
{'role':'user', 'content':'Hi, my name is Isa'},
{'role':'assistant', 'content': "Hi Isa! It's nice to meet you. \
Is there anything I can help you with today?"},
{'role':'user', 'content':'Yes, you can remind me, What is my name?'}  ]
response = get_completion_from_messages(messages, temperature=1)
print(response)
~~~
执行结果：
<img width="862" alt="截屏2023-05-26 22 56 14" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/02c1a3f7-6466-4fdf-b961-033570e17f48">

### 2.订单机器人
~~~python
def collect_messages(_):
    prompt = inp.value_input
    inp.value = ''
    context.append({'role':'user', 'content':f"{prompt}"})
    response = get_completion_from_messages(context) 
    context.append({'role':'assistant', 'content':f"{response}"})
    panels.append(
        pn.Row('User:', pn.pane.Markdown(prompt, width=600)))
    panels.append(
        pn.Row('Assistant:', pn.pane.Markdown(response, width=600, style={'background-color': '#F6F6F6'})))
 
    return pn.Column(*panels)
~~~
~~~python
import panel as pn  # GUI
pn.extension()

panels = [] # collect display 

context = [ {'role':'system', 'content':"""
You are OrderBot, an automated service to collect orders for a pizza restaurant. \
You first greet the customer, then collects the order, \
and then asks if it's a pickup or delivery. \
You wait to collect the entire order, then summarize it and check for a final \
time if the customer wants to add anything else. \
If it's a delivery, you ask for an address. \
Finally you collect the payment.\
Make sure to clarify all options, extras and sizes to uniquely \
identify the item from the menu.\
You respond in a short, very conversational friendly style. \
The menu includes \
pepperoni pizza  12.95, 10.00, 7.00 \
cheese pizza   10.95, 9.25, 6.50 \
eggplant pizza   11.95, 9.75, 6.75 \
fries 4.50, 3.50 \
greek salad 7.25 \
Toppings: \
extra cheese 2.00, \
mushrooms 1.50 \
sausage 3.00 \
canadian bacon 3.50 \
AI sauce 1.50 \
peppers 1.00 \
Drinks: \
coke 3.00, 2.00, 1.00 \
sprite 3.00, 2.00, 1.00 \
bottled water 5.00 \
"""} ]  # accumulate messages


inp = pn.widgets.TextInput(value="Hi", placeholder='Enter text here…')
button_conversation = pn.widgets.Button(name="Chat!")

interactive_conversation = pn.bind(collect_messages, button_conversation)

dashboard = pn.Column(
    inp,
    pn.Row(button_conversation),
    pn.panel(interactive_conversation, loading_indicator=True, height=300),
)

dashboard
~~~
代码中，导入了一个GUI，用界面来展示对话，`collect_messages`函数会收集我们每轮对话，再我要问机模型问题时把前面的对话都发给模型。
执行结果：
<img width="863" alt="截屏2023-05-26 22 59 58" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/a99e254f-e5cc-436c-8d4b-37afbc2b2fba">
下单完成后，订单机器人就可以把我们下的订单总结成JSON，发给订单系统来结账
~~~python
messages =  context.copy()
messages.append(
{'role':'system', 'content':'create a json summary of the previous food order. Itemize the price for each item\
 The fields should be 1) pizza, include size 2) list of toppings 3) list of drinks, include size   4) list of sides include size  5)total price '},    
)
 #The fields should be 1) pizza, price 2) list of toppings 3) list of drinks, include size include price  4) list of sides include size include price, 5)total price '},    

response = get_completion_from_messages(messages, temperature=0)
print(response)
~~~
<img width="863" alt="截屏2023-05-26 23 01 20" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/2b5e6d0d-3376-4227-9780-c1de7e58c1fb">
<img width="863" alt="截屏2023-05-26 23 01 34" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/5c831b18-f486-4921-89d4-3d8c5eeba376">


