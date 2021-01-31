---
layout: post
comments: true
title: 'AI채팅 오픈소스 공유'
author: 가위
date: 2021-01-31 19:30
tags: [개발,Opensource]
---

오늘은 Python의 [[ChatterBot](https://github.com/gunthercox/ChatterBot)]모듈을 Discord Bot에 적용한 소스를 공유해볼것이다.

ChatterBot이 뭔지 모른다면 [[이전일지](https://tales-blog.vercel.app/2021/01/29/AIchat1/)]를 참고하시라.

각설하고 이어가겠다.

먼저 사용하기전에 모듈을 설치해야한다. 참고로 필자는 Python 3.7.7 64bit를 사용하였다.

```cmd
pip install chatterbot
pip install --upgrade chatterbot_corpus korean
pip install --upgrade chatterbot
python -m spacy download en
```

그후 한글 Corpus를 적용해야한다.

https://github.com/gunthercox/chatterbot-corpus/tree/master/chatterbot_corpus/data
여기로 가서 Korean파일을 내려받는다.

그리고, C:\Users\계정이름\AppData\Local\Programs\Python\Python37\Lib\site-packages\chatterbot_corpus\data 여기에 Korean폴더째로 넣는다.

마지막으로 아무 Python파일을 생성하여 아래 소스를 복붙한다.

```py
from chatterbot import ChatBot
from chatterbot.trainers import ChatterBotCorpusTrainer
import discord
from discord.ext import commands
# Create a new chat bot named Charlie
app = discord.Client()
chatbot = ChatBot(
    'Charlie',
    logic_adapters=[
        "chatterbot.logic.BestMatch"
    ])
# Create a new Trainer
trainer = ChatterBotCorpusTrainer(chatbot)

trainer.train(
    "chatterbot.corpus.korean"
)
@app.event
async def on_ready():
    print("Ready.")
@app.event
async def on_message(message):
    if message.author.bot:
        return
    try:
        user_input = message.content

        bot_response = chatbot.get_response(user_input)

        await message.channel.send(bot_response)
    except:
        pass

app.run('Your Token')
```

마지막으로 실행하면 아마 잘 실행될것이다. 시도해보고 오류나 해결이 안된다면 댓글이나 Discord '가위#1111'로 DM보내시라.
