# 监听管理
---

## 🚀 会话列表切换监听

如果注册了此事件，当会话列表进行切换时触发，可以在回调事件中触发自己的动作，如：读取用户数据库，更改自己的UI等.

> 不知道会话列表的位置？请参见 [POM对象](./glossary.md)的 ```13.ConversationList会话列表组件与Conversation组件```

### 添加会话列表切换监听器

请通过```WeChatMainWindow```调用AddConversationChangeListener()方法, 请参见[WeChatMainWindow](../api/WeChatAuto.Components.WeChatMainWindow.html)类

方法定义:

```
public void AddConversationChangeListener(Action<ChatContext, CancellationToken> callBack)

```

其中：
- callBack: 回调方法，当选择的会话项目(item)变化时触发，可以在回调方法中实现自己的业务逻辑，SDK提供一个ChatContext对象与一个CancellationToken对象供调用者使用，具体在下面介绍,ChatContext类参考: [ChatContext类](../api/WeChatAuto.Models.ChatContext.html)。


**注意事宜：**

1. 监听时间间隔默认为：5秒，如果需要修改这个间隔时间，请在WeAutomation初始化的时候修改，如下所示:

```
WeAutomation.Initialize(options =>
{
  ... 其他代码
  options.ConversationChangeListenerInterval=3,  //在这里修改时间间
  ... 其他代码
});
````

2. 建议不要在此事件中进行微信自动化操作，仅操作自己的UI及获取客户数据，当然进行自动化操作也是可以的，只是不建议

3. 一个winform的调用示例:

```
var window = client.WxMainWindow;

window.AddConversationChangeListener((context, token) =>
{
    Console.WriteLine(context.ToString());
    //这里调用自己的方法,通过context可以获取自己需要的信息,如：好友/群聊/企业微信的名字等.
    //如果这里运行的是长任务，可能任务没有运行完，用户又点击了其他的列表项，通过token可以收到外部终止执行的信息，可以运行```token.ThrowIfCancellationRequested();```终止任务
});
```


### 移除会话列表切换监听

移除会话列表切换监听，移除后，会话列表切换事件将不会触发

```
public void RemoveConversationChangeListener()
```

### 暂停会话列表监听

暂停会话列表监听，暂停后，会话列表切换事件将不会被触发，直到恢复会话列表监听

```
public void PauseConversationChangeListener()
```

### 恢复会话列表监听

恢复暂停的会话列表监听，恢复后，以前通过```AddConversationChangeListener```注册的会话列表切换事件将会被触发

```
public void ResumeConversationChangeListener()
```



## 🚀 消息监听

### 添加消息监听

> 适用于聊天（与好友或者在群聊中)时监听好友发来的消息,需要提供一个回调函数，当消息到达时，会自动调用这个回调函数

可以通过[WeChatClinet对象](../api/WeChatAuto.Components.WeChatClient.html)调用```AddMessageListener```方法,当然也可以通过[WeChatMainWindow](../api/WeChatAuto.Components.WeChatMainWindow.html)类来调用此方法

方法定义:

```
public async Task AddMessageListener(string nickName, Action<MessageContext> callBack)
```

其中：
  - nickName: 好友昵称
  - callBack: 回调函数,由用户提供,参数请参考[MessageContext类](../api/WeChatAuto.Models.MessageContext.html)

### 移除消息监听

> 添加消息监听会启动一个```System.Threading.Timer```来监听消息，所以如果不需要这个监听的时候，请移除监听

方法定义:

```
 public void StopMessageListener(string nickName)
```

其中:

  - nickName: 好友昵称

## 🚀 新好友申请监听

如果添加了新好友申请监听，当有新好友添加你的微信时，会自动监听到好友申请，并且执行你定义的逻辑

> 可以通过[WeChatClinet对象](../api/WeChatAuto.Components.WeChatClient.html)调用方法,当然也可以通过[WeChatMainWindow](../api/WeChatAuto.Components.WeChatMainWindow.html)类来调用。

### 自动通过新好友申请

用户只需提供一个回调函数，当检测到有新好友申请时，系统会自动通过申请，并在通过后调用用户的回调函数。

> 可通过[WeChatClient对象](../api/WeChatAuto.Components.WeChatClient.html)调用```AddFriendRequestAutoAcceptListener```方法。

方法定义:

```
public void AddFriendRequestAutoAcceptListener(Action<List<string>> callBack, string keyWord = null, string suffix = null, string label = null)
```

参数说明：
  - callBack：自定义回调函数，当有新好友添加成功时被调用
  - keyWord：可选, 设定关键词；仅当好友申请内容包含此关键词时才自动通过。若为```null```，则所有好友申请均会被自动通过
  - suffix：可选，自动通过时在新好友昵称后添加的后缀，便于管理分类
  - label：可选，自动通过时为新好友打上的微信标签，方便分类管理

### 自动通过新好友申请-监控好友消息-也可以发送首次聊天消息

用户只需提供一个回调函数，当自动通过好友申请后，如果好友有消息发送过来，会被监听到，可以通过```MessageContext```对象获取客户信息并回复等，具体请参看: [MessageContext](../api/WeChatAuto.Models.MessageContext.html)。

可选的，也可以在自动通过好友后，由我首先发起聊天（让我首先给客户发送文本、表情、图片、文件等），可以通过```Sender```对象发送首次消息，具体请参考: [Sender](../api/WeChatAuto.Components.Sender.html)。

> 可通过[WeChatClient对象](../api/WeChatAuto.Components.WeChatClient.html)调用```AddFriendRequestAutoAcceptAndOpenChatListener```方法。

方法定义:

```
    public void AddFriendRequestAutoAcceptAndOpenChatListener(Action<MessageContext> callBack, Action<Sender> firstMessageAction = null, string keyWord = null, string suffix = null, string label = null, bool isDelet = true)    
```

参数说明：
  - callBack：自定义回调函数，当有新好友添加成功时被调用,当自动通过好友申请后，如果好友有消息发送过来，则执行此回调方法,系统注入一个 ```MessageContext```对象支持获取所有聊天信息的上下文支持，具体请参考: [MessageContext类](../api/WeChatAuto.Models.MessageContext.html)
  - firstMessageAction: 可选, 当自动通过好友申请后，由我首次给好友发送消息,此Action由系统自动注入一个 ```Sender```对象可以由“我”来发起打招呼等消息
  - keyWord：可选, 设定关键词；仅当好友申请内容包含此关键词时才自动通过。若为```null```，则所有好友申请均会被自动通过
  - suffix：可选，自动通过时在新好友昵称后添加的后缀，便于管理分类
  - label：可选，自动通过时为新好友打上的微信标签，方便分类管理
  - isDelet: 添加好友成功后是否删除好友申请按钮，默认删除

### 移除好友申请监听

方法定义:
```
StopNewUserListener()
```

## 🚀 添加朋友圈监听
> 开启朋友圈监听后，可以自动获取到好友在朋友圈发布的动态，适合实现自动点赞、自动评论等功能。

可以通过[WeChatClinet对象](../api/WeChatAuto.Components.WeChatClient.html)调用方法,当然也可以通过[WeChatMainWindow](../api/WeChatAuto.Components.WeChatMainWindow.html)类来调用。


方法签名：

```
public void AddMomentsListener(OneOf<string, List<string>> nickNameOrNickNames, bool autoLike = true, Action<MomentsContext, IServiceProvider> action = null)
```

参数说明：
- nickNameOrNickNames：单个好友昵称或昵称列表，表示需要监听哪些好友的朋友圈动态。
- autoLike：是否自动点赞，默认为 true。如需关闭自动点赞可设置为 false。
- action：当监听到朋友圈新动态时自动调用的回调函数，用户可自定义处理逻辑。回调参数包括：
  - [MomentsContext](../api/WeChatAuto.Utils.MomentsContext.html)：包含点赞、评论等操作方法。
  - IServiceProvider：依赖注入服务提供器，可用于获取自定义 Service，方便扩展自己的业务逻辑。

### MomentsContext详解

> MomentsContext为处理朋友圈动态提供的上下文对象,方便与LLM集成

属性与方法如下：

- 获取历史回复列表

方法签名:

```
public List<ReplyItem> GetHistoryReplyItems()
```

返回```List<ReplyItem>```列表，具体```ReplyItem```类请参考：[ReplyItem](../api/WeAutoCommon.Models.ReplyItem.html)

- 获取朋友圈动态内容项

方法鉴名：

```
public MomentItem GetMomentItem() 
```

返回```MomentItem```对象，可以通过此对象获取发送朋友圈动态的好友昵称、内容、时间、评论列表、我是否点赞等信息,具体请参考:[MomentItem](../api/WxAutoCommon.Models.MomentItem.html)

- 获取朋友圈动态内容

方法鉴名:
```
public string GetMomentContent()
```

> 此内容可以做为大模型提供上下文信息。

- 获取朋友圈动态列表项唯一标识
方法定义：
```
public string GetMomentKey()
```

返回朋友圈动态列表的唯一标识,此标识可以做为大模型提供上下文信息,如：朋友圈动态记录唯一标识。

- 是否我是最后一个回复的人
方法定义：
```
public bool IsMyEndReply()
```

确认我是否是最后的回复人，供大模型自动回复朋友圈动态时参考

- 是否朋友动态的回复列表中包含我的回复
方法定义:
```
public bool IsIncludeMyReply()
```

回复列表中是否包含我的回复，供大模型自动回复时参考。

- 是否我点赞过朋友圈动态
方法定义:
```
public bool IsMyLiked()
```

此朋友圈动态是否我点赞过，供模型自动回复时参考

- 执行回复朋友圈动态

方法定义:
```
public void DoReply(string replyContent)
```
>为此条朋友圈动态进行回复

其中:
  - replyContent: 回复内容

- 执行朋友圈动态点赞

方法定义：

```
public void DoLike()
```

> 为此条朋友圈动态进行点赞，如果已经点赞，则不作操作.