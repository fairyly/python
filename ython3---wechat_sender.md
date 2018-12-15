# wechat_sender
>随时随地发送消息到微信


## 安装
```
pip install wechat_sender
```

## 运行环境

Python 2.7 及以上 Python 3 及以上

## 使用

- 1.登录微信并启动 wechat_sender 服务.
```
   from wxpy import *
   from wechat_sender import *
   bot = Bot()
   listen(bot)
   # 之后 wechat_sender 将持续运行等待接收外部消息
```
- 2.在外部向微信发送消息.
```
   from wechat_sender import Sender
   Sender().send('Hello From Wechat Sender')
   # Hello From Wechat Sender 这条消息将通过 1 中登录微信的文件助手发送给你
```
如果你是 wxpy 的使用者，只需更改一句即可使用 wechat_sender：

例如这是你本来的代码： 

```
# coding: utf-8
from __future__ import unicode_literals

from wxpy import *
bot = Bot('bot.pkl')

my_friend = bot.friends().search('xxx')[0]

my_friend.send('Hello WeChat!')

@bot.register(Friend)
def reply_test(msg):
    msg.reply('test')

bot.join()
```

- 使用 wechat_sender：
```
# coding: utf-8
from __future__ import unicode_literals

from wxpy import *
from wechat_sender import listen
bot = Bot('bot.pkl')

my_friend = bot.friends().search('xxx')[0]

my_friend.send('Hello WeChat!')

@bot.register(Friend)
def reply_test(msg):
    msg.reply('test')

listen(bot) # 只需改变最后一行代码
```

之后如果你想在其他程序或脚本中发送微信消息，只需要：
```
# coding: utf-8
from wechat_sender import Sender
Sender().send("test message")
```

## 文档
http://wechat-sender.readthedocs.io/zh_CN/latest/


## 参考
- [github---](https://github.com/bluedazzle/wechat_sender)
- [wxpy](https://github.com/youfou/wxpy)
- [如何将报警日志发到微信上](https://www.rapospectre.com/blog/send-alert-to-wechat)
