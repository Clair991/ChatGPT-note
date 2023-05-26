### 1.自定义自动回复客户电子邮件
~~~python
# given the sentiment from the lesson on "inferring",
# and the original customer message, customize the email
sentiment = "negative"

# review for a blender
review = f"""
So, they still had the 17 piece system on seasonal \
sale for around $49 in the month of November, about \
half off, but for some reason (call it price gouging) \
around the second week of December the prices all went \
up to about anywhere from between $70-$89 for the same \
system. And the 11 piece system went up around $10 or \
so in price also from the earlier sale price of $29. \
So it looks okay, but if you look at the base, the part \
where the blade locks into place doesn’t look as good \
as in previous editions from a few years ago, but I \
plan to be very gentle with it (example, I crush \
very hard items like beans, ice, rice, etc. in the \ 
blender first then pulverize them in the serving size \
I want in the blender then switch to the whipping \
blade for a finer flour, and use the cross cutting blade \
first when making smoothies, then use the flat blade \
if I need them finer/less pulpy). Special tip when making \
smoothies, finely cut and freeze the fruits and \
vegetables (if using spinach-lightly stew soften the \ 
spinach then freeze until ready for use-and if making \
sorbet, use a small to medium sized food processor) \ 
that you plan to use that way you can avoid adding so \
much ice if at all-when making your smoothie. \
After about a year, the motor was making a funny noise. \
I called customer service but the warranty expired \
already, so I had to buy another one. FYI: The overall \
quality has gone done in these types of products, so \
they are kind of counting on brand recognition and \
consumer loyalty to maintain sales. Got it in about \
two days.
"""
~~~
~~~python
# given the sentiment from the lesson on "inferring",
# and the original customer message, customize the email
sentiment = "negative"

# review for a blender
review = f"""
So, they still had the 17 piece system on seasonal \
sale for around $49 in the month of November, about \
half off, but for some reason (call it price gouging) \
around the second week of December the prices all went \
up to about anywhere from between $70-$89 for the same \
system. And the 11 piece system went up around $10 or \
so in price also from the earlier sale price of $29. \
So it looks okay, but if you look at the base, the part \
where the blade locks into place doesn’t look as good \
as in previous editions from a few years ago, but I \
plan to be very gentle with it (example, I crush \
very hard items like beans, ice, rice, etc. in the \ 
blender first then pulverize them in the serving size \
I want in the blender then switch to the whipping \
blade for a finer flour, and use the cross cutting blade \
first when making smoothies, then use the flat blade \
if I need them finer/less pulpy). Special tip when making \
smoothies, finely cut and freeze the fruits and \
vegetables (if using spinach-lightly stew soften the \ 
spinach then freeze until ready for use-and if making \
sorbet, use a small to medium sized food processor) \ 
that you plan to use that way you can avoid adding so \
much ice if at all-when making your smoothie. \
After about a year, the motor was making a funny noise. \
I called customer service but the warranty expired \
already, so I had to buy another one. FYI: The overall \
quality has gone done in these types of products, so \
they are kind of counting on brand recognition and \
consumer loyalty to maintain sales. Got it in about \
two days.
"""
~~~
`prompt`：
你是一名客服人工智能助理。
你的任务是向重要客户发送电子邮件回复。
鉴于客户电子邮件由```分隔，
生成回复以感谢客户的评价。
如果情绪是积极的或中性的，感谢他们的评价。
如果情绪是负面的，道歉，并建议他们可以联系到客户服务。
确保使用评价中的具体细节。
用简洁和专业的语气写作。
用“AI customer agent”签名
执行结果：
<img width="864" alt="截屏2023-05-26 22 22 56" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/eba62cc0-565d-47ed-a0b9-9939fdaa9fce">

### 2.提醒模型使用客户电子邮件中的详细信息
~~~python
prompt = f"""
You are a customer service AI assistant.
Your task is to send an email reply to a valued customer.
Given the customer email delimited by ```, \
Generate a reply to thank the customer for their review.
If the sentiment is positive or neutral, thank them for \
their review.
If the sentiment is negative, apologize and suggest that \
they can reach out to customer service. 
Make sure to use specific details from the review.
Write in a concise and professional tone.
Sign the email as `AI customer agent`.
Customer review: ```{review}```
Review sentiment: {sentiment}
"""
response = get_completion(prompt, temperature=0.7)
print(response)
~~~
`prompt`：添加了一句 Make sure to use specific details from the review.
`get_completion`参数`temperature`允许你改变模型的探索和多样性的程度
执行结果：
<img width="864" alt="截屏2023-05-26 22 25 28" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/f310e35c-1bb5-47b1-872e-ec22cb7a2bbd">

### 3.参数 temperature
<img width="488" alt="截屏2023-05-26 22 33 04" src="https://github.com/Clair991/ChatGPT-note/assets/102845425/e3132e5b-251c-456f-9da6-783551db0ebc">

参数`temperature`能使我们改变模型响应的多样性。
比如，我最喜欢的食物，模型预测pizza可能性为53%，sushi的可能性为30%，tacos的可能性为5%
如果`temperature = 0`则模型总是给出pizza
如果`temperature > 0则模型给出的结果可能就会是 sushi 或者 tacos



