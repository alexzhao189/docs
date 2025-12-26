# 会话管理
---

## 获取所有可见会话列表

获取会话列表可见会话,会话信息包含：会话名称、会话类型、会话状态、会话时间、会话未读消息数、会话头像等信息

方法签名:
```
public List<Conversation> GetVisibleConversations()
```

返回: ```List<Conversation>```,具体```Conversation```类包含会话的一些信息，请参考: [Conversation类](../api/WeAutoCommon.Models.Conversation.html)

## 获取会话列表所有名称

获取会话列表所有会话的名称,考虑到效率，只返回名称列表

方法签名:
```
public List<string> GetAllConversations()
```

返回：所有会话列表的标题

## 定位会话

定位会话的作用：可以将会话列表滚动到指定会话的位置，使指定会话可见

方法签名:
```
public bool LocateConversation(string title)
```

其中:
  - title: 会话标题

## 点击会话

点击会话,使会话窗口获取焦点

方法签名:
```
public void ClickConversation(string title)
```

其中:
  - title: 会话标题

## 点击第一个会话

点击第一个会话,使第一个会话窗口获取焦点

方法签名:
```
public void ClickFirstConversation()
```

## 双击会话

双击会话,弹出子窗口,使会话窗口获取焦点

方法签名:
```
public void DoubleClickConversation(string title)
```

其中:
  - title: 会话标题

## 获取可见会话标题

方法签名:
```
public List<string> GetVisibleConversationTitles()
```

返回会话标题列表

## 查找并打开会话

查找并打开会话,如果找到，则打开好友或者群聊窗口

方法签名:
```
public WeAutoCommon.Models.Result FindAndOpenFriendOrGroup(string who)
```

其中:
  - who: 好友或者群聊昵称