# 消息管理
---

> 注：教程中"好友名称"与"好友昵称"具有同样的语意

## 🚀 消息发送模型
WeChatAuto.SDK支持两种消息发送模型:

- **单独发送消息**

  适用于单发、群发、转发等简单聊天场景。

- **聊天过程消息回复**

  适用于在聊天过程中进行消息回复，特别适合接入大模型实现自动回复的场景。

## 😊 单独发送消息

> 具体请参考源码: ```项目根目录\Examples\demo03```

### 1. 发送文本消息
 - 可以通过```WeChatClient```类或者```WeChatMainWindow```类发送消息,或者通过窗口类```WeChatMainWindow```获得```Sender```对象来发送消息
 - 方法定义
 ```
 public async Task SendWho(string who, string message, OneOf<string, string[]> atUser = default, bool isOpenChat = true)
 ```
 
 其中：
   - who: 好友名称，可以是好友或者群昵昵称
   - message: 消息内容
   - atUser: 被@的好友昵称，可以单个好友，也可以是一个好友数组，适用群聊中@好友
   - isOpenChat: 是否打开子聊天窗口,默认是True:打开,False:不打开
 - 示例代码:
 ```
 /***
 * 示例三 - 消息管理演示
 */

using WeChatAuto.Services;
using WeChatAuto.Components;
using Microsoft.Extensions.DependencyInjection;

var serviceProvider = WeAutomation.Initialize(options =>
{
    options.DebugMode = true;
});

using var clientFactory = serviceProvider.GetRequiredService<WeChatClientFactory>();
var client = clientFactory.GetWeChatClient("Alex");
//发送消息给AI.Net好友昵称，请修改成自己的好友昵称
await client.SendWho("AI.Net", "你好，世界！");
//发送消息给群聊"测试11"，并好友:@123321
await client.SendWho("测试11", "你好，世界！", "123321");
//发送消息给群聊"测试11"，并好友:@123321,@Alex
await client.SendWho("测试11","你好，世界！",new string[]{"123321","Alex"});
 ```

### 2. 发送Emoji表情

> 注：Emoji 表情可以单独发送，也可以包含在文字消息中，其他用法同上面的[发送文本消息](#1-发送文本消息)

- 方法定义:
```
public async Task SendEmoji(string who, OneOf<int, string> emoji, OneOf<string, string[]> atUser = default, bool isOpenChat = false)
```
其中:
  - who: 好友名称，可以是好友或者群昵昵称
  - emoji: 可以是表情名称或者描述或者索引,具体索引或者描述等请参考下面的Emoji表
  - atUser: 被@的好友昵称，可以单个好友，也可以是一个好友数组，适用群聊中@好友
  - isOpenChat: 是否打开子聊天窗口,默认是True:打开,False:不打开

- Emoji表参考

> Emoji表按打开微信聊天窗口的“表情”从左到右，从上到下进行索引排序

| 索引 (index) | 描述 (desc)  | 表情名 (value)     |
|:---:|:-------------:|:----------:|
| 1 | 微笑 | [微笑] |
| 2 | 撇嘴 | [撇嘴] |
| 3 | 色色表情 | [色] |
| 4 | 发呆 | [发呆] |
| 5 | 得意 | [得意] |
| 6 | 流泪 | [流泪] |
| 7 | 害羞 | [害羞] |
| 8 | 闭嘴 | [闭嘴] |
| 9 | 睡 | [睡] |
| 10 | 大哭 | [大哭] |
| 11 | 尴尬 | [尴尬] |
| 12 | 发怒 | [发怒] |
| 13 | 调皮 | [调皮] |
| 14 | 呲牙 | [呲牙] |
| 15 | 惊讶 | [惊讶] |
| 16 | 难过 | [难过] |
| 17 | 囧 | [囧] |
| 18 | 抓狂 | [抓狂] |
| 19 | 吐 | [吐] |
| 20 | 偷笑 | [偷笑] |
| 21 | 愉快 | [愉快] |
| 22 | 白眼 | [白眼] |
| 23 | 傲慢 | [傲慢] |
| 24 | 困 | [困] |
| 25 | 惊恐 | [惊恐] |
| 26 | 憨笑 | [憨笑] |
| 27 | 悠闲 | [悠闲] |
| 28 | 咒骂 | [咒骂] |
| 29 | 疑问 | [疑问] |
| 30 | 嘘 | [嘘] |
| 31 | 晕 | [晕] |
| 32 | 衰 | [衰] |
| 33 | 骷髅 | [骷髅] |
| 34 | 敲打 | [敲打] |
| 35 | 再见 | [再见] |
| 36 | 擦汗 | [擦汗] |
| 37 | 抠鼻 | [抠鼻] |
| 38 | 鼓掌 | [鼓掌] |
| 39 | 坏笑 | [坏笑] |
| 40 | 右哼哼 | [右哼哼] |
| 41 | 鄙视 | [鄙视] |
| 42 | 委屈 | [委屈] |
| 43 | 快哭了 | [快哭了] |
| 44 | 阴险 | [阴险] |
| 45 | 亲亲 | [亲亲] |
| 46 | 可怜 | [可怜] |
| 47 | 笑脸 | [笑脸] |
| 48 | 生病 | [生病] |
| 49 | 脸红 | [脸红] |
| 50 | 破涕为笑 | [破涕为笑] |
| 51 | 恐惧 | [恐惧] |
| 52 | 失望 | [失望] |
| 53 | 无语 | [无语] |
| 54 | 嘿哈 | [嘿哈] |
| 55 | 捂脸 | [捂脸] |
| 56 | 奸笑 | [奸笑] |
| 57 | 机智 | [机智] |
| 58 | 皱眉 | [皱眉] |
| 59 | 耶 | [耶] |
| 60 | 吃瓜 | [吃瓜] |
| 61 | 加油 | [加油] |
| 62 | 汗 | [汗] |
| 63 | 天啊 | [天啊] |
| 64 | 嗯嗯嗯 | [Emm] |
| 65 | 社会社会 | [社会社会] |
| 66 | 旺柴 | [旺柴] |
| 67 | 好的 | [好的] |
| 68 | 打脸 | [打脸] |
| 69 | 哇 | [哇] |
| 70 | 翻白眼 | [翻白眼] |
| 71 | 666 | [666] |
| 72 | 让我看看 | [让我看看] |
| 73 | 叹气 | [叹气] |
| 74 | 苦涩 | [苦涩] |
| 75 | 裂开 | [裂开] |
| 76 | 嘴唇 | [嘴唇] |
| 77 | 爱心 | [爱心] |
| 78 | 心碎 | [心碎] |
| 79 | 拥抱 | [拥抱] |
| 80 | 强 | [强] |
| 81 | 弱 | [弱] |
| 82 | 握手 | [握手] |
| 83 | 胜利 | [胜利] |
| 84 | 抱拳 | [抱拳] |
| 85 | 勾引 | [勾引] |
| 86 | 拳头 | [拳头] |
| 87 | OK | [OK] |
| 88 | 合十 | [合十] |
| 89 | 啤酒 | [啤酒] |
| 90 | 咖啡 | [咖啡] |
| 91 | 蛋糕 | [蛋糕] |
| 92 | 玫瑰 | [玫瑰] |
| 93 | 凋谢 | [凋谢] |
| 94 | 菜刀 | [菜刀] |
| 95 | 炸弹 | [炸弹] |
| 96 | 便便 | [便便] |
| 97 | 月亮 | [月亮] |
| 98 | 太阳 | [太阳] |
| 99 | 庆祝 | [庆祝] |
| 100 | 礼物 | [礼物] |
| 101 | 红包 | [红包] |
| 102 | 發 | [發] |
| 103 | 福 | [福] |
| 104 | 烟花 | [烟花] |
| 105 | 爆竹 | [爆竹] |
| 106 | 猪头 | [猪头] |
| 107 | 跳跳 | [跳跳] |
| 108 | 发抖 | [发抖] |
| 109 | 转圈 | [转圈] |

### 3. 发起语音聊天,适用于单全好友

给**单个**好友发起语音聊天，如果是群聊，请使用下面的```SendVoiceChats```方法,参看[群聊中发起语音聊天](#4-群聊中发起语音聊天)

- 方法定义
```
public void SendVoiceChat(string who, bool isOpenChat = true)
```

其中:
  - who: 好友昵称，非群聊昵称
  - isOpenChat: 是否打开子聊天窗口,默认是True:打开,False:不打开

### 4. 群聊中发起语音聊天

在聊天室中发起语音聊天，需要指定**聊天人**

- 方法定义

```
public void SendVoiceChats(string groupName, string[] whos, bool isOpenChat = true) => WxMainWindow.SendVoiceChats(groupName, whos, isOpenChat);
```

其中:
  - groupName: 群聊名称
  - whos: 好友昵称列表,**好友昵称**必须在群聊中
  - isOpenChat: 是否打开子聊天窗口,默认是True:打开,False:不打开

### 5. 发起直播

群聊中发起直播，单个好友**没有**直播功能

- 方法定义

```
public void SendLiveStreaming(string groupName, bool isOpenChat = false)
```

其中:
  - groupName: 群聊名称
  - isOpenChat: 是否打开子聊天窗口,默认是True:打开,False:不打开

### 6. 获取当前聊天窗口的标题

获取主窗口当前聊天的窗口

- 方法定义:

```
public string GetCurrentChatTitle()
```

### 7. 给指定好友发送(多个)文件

- 方法定义

```
public async Task SendFile(string who, OneOf<string, string[]> files, bool isOpenChat = false)
```

其中：
  - who: 好友名称
  - files: 文件路径,可以是单个文件路径，也可以是多个文件路径，要求文件实际在磁盘中存在
  - isOpenChat: 是否打开子聊天窗口,默认是True:打开,False:不打开

### 8. 发送消息给多个好友(批量发送)

给多个好友（包括群聊）批量发送同样的消息

- 方法定义:

```
public async Task SendWhos(string[] whos, string message, OneOf<string, string[]> atUser = default, bool isOpenChat = true)
```

其中：
  - whos: 好友名称列表
  - message: 消息列表
  - atUser: 被@的用户,最主要用于群聊中@人,可以是一个用户，也可以是多个用户，如果是自有群，可以@所有人，也可以@单个用户，微信不支持他有群@所有人
  - isOpenChat: 是否打开子聊天窗口,默认是True:打开,False:不打开

### 9. 给多个好友发送文件

- 方法定义:

```
public async Task SendFiles(string[] whos, OneOf<string, string[]> files, bool isOpenChat = false)
```

其中：
  - whos: 好友名称列表
  - files: 文件路径,可以是单个文件路径，也可以是多个文件路径，要求文件实际在磁盘中存在
  - isOpenChat: 是否打开子聊天窗口,默认是True:打开,False:不打开

### 10. 获取用户所有消息列表

由于聊天内容比较长，需要指定聊天页数,如果指定为-1，则获取所有气泡,有可能被卡死

- 方法定义:

```
public List<ChatSimpleMessage> GetChatAllHistory(string who,int pageCount = 10)
```

其中：
  - who: 好友名称，可以是好友，也可以是群聊名称
  - pageCount: 获取的气泡数量，默认是10页,可以指定获取的页数，如果指定为-1，则获取所有气泡

### 11. 转发消息给好友

转发指定的消息数量给好友

- 方法定义:

```
public async Task<bool> ForwardMessage(string fromWho, string toWho, int rowCount = 5)
```

其中：
  - fromWho: 转发消息的来源,可以是好友名称，也可以是群聊名称
  - toWho: 转发消息的接收者,可以是好友名称，也可以是群聊名称
  - rowCount: 转发消息的行数


## 😁 聊天过程消息回复
