群聊管理
---

群聊管理仅适用于聊天室进行管理,可以实现获取群聊成员对象，自动加好友等操作

> 可以通过[WeChatClinet对象](../api/WeChatAuto.Components.WeChatClient.html)调用下面方法,当然也可以通过[WeChatMainWindow](../api/WeChatAuto.Components.WeChatMainWindow.html)类来调用。

## 基础术语
- 自有群：自己担任群主的群聊
- 外部群：他人为群主且自己为成员的群聊

## 获取群聊成员列表

方法签名:
```
public async Task<List<string>> GetChatGroupMemberList(string groupName)
```

其中:
  - groupName: 聊天室昵称
  - 返回: ```List<string>```: 聊天室所有成员的昵称列表

## 是否是群聊成员
方法签名：

```
public async Task<bool> IsChatGroupMember(string groupName, string memberName)
```

其中：
  - groupName: 群聊昵称
  - memberName: 群聊成员

返回： 指定的memberName是否在群聊成员列表中

## 测试群是否是自有群
方法签名:

```
public async Task<bool> IsOwnerChatGroup(string groupName)
```

如果自己是群主，则返回:True

## 获取群主
方法签名:

```
public async Task<string> GetGroupOwner(string groupName)
```

返回群聊```groupName```的群主名称

## 清空群聊历史聊天记录
方法签名:
```
public async Task ClearChatGroupHistory(string groupName)
```

其中：
  - groupName: 群聊名称

## 退出群聊
方法签名:
```
public async Task QuitChatGroup(string groupName)
```

其中：
  - groupName: 群聊名称

## 设置消息免打扰
方法签名:
```
public ChatResponse SetMessageWithoutInterruption(string groupName, bool isMessageWithoutInterruption = true)
```

其中:
  - groupName: 群聊名称
  - isMessageWithoutInterruption: 是否消息免打扰,默认是True:消息免打扰,False:取消消息免打扰

## 保存到通讯录
方法签名:
```
public ChatResponse SetSaveToAddress(string groupName, bool isSaveToAddress = true)
```

其中：
  - groupName: 群聊名称
  - isSaveToAddress: 是否保存到通讯录,默认是True:保存,False:取消保存

## 设置聊天置顶
方法签名:
```
public ChatResponse SetChatTop(string groupName, bool isTop = true)
```
其中：
  - groupName: 群聊名称
  - isTop: 是否置顶,默认是True:置顶,False:取消置顶

## 改变自有群群备注

> 修改自有群备注（当然...我们也修改不了他有群的备注）

方法签名:
```
public ChatResponse ChageOwerChatGroupMemo(string groupName, string newMemo)
```

其中:
  - groupName: 群聊名称
  - newMemo: 新备注

返回: 请参考[ChatResponse类](../api/WxAutoCommon.Models.ChatResponse.html)

## 修改群名

修改群名，适用于自有群群名

方法签名:

```
public ChatResponse ChangeOwerChatGroupName(string oldGroupName, string newGroupName)
```

其中:
  - oldGroupName: 旧群名称
  - newGroupName: 新群名称

## 更新群聊公告
更新群聊公告，仅适用于自有群的群聊公告更新

方法签名:
```
public async Task<ChatResponse> UpdateGroupNotice(string groupName, string groupNotice)
```

其中:
  - groupName: 群聊名称
  - groupNotice: 群聊公告

## 创建群聊
创建群聊，如果存在，则打开它，如果群聊不存在，则创建一个新群聊

方法签名:
```
public ChatResponse CreateOrUpdateOwnerChatGroup(string groupName, OneOf<string, string[]> memberName)
```

其中：
  - groupName: 群聊名称
  - memberName: 要拉入群聊的成员，即好友昵称列表

## 检查群聊是否存在
检查群聊或者好友是否存在
方法签名:
```
public bool CheckFriendExist(string friendName, bool doubleClick = false)
```

其中：
  - friendName: 好友昵称，也可以是群聊昵称
  - doubleClick: 如果好友存在，是否在会话窗口中打开它，即：是否双击它打开子窗口

## 添加群聊成员
添加群聊成员,将好友拉到群中

方法签名:
```
public async Task<ChatResponse> AddOwnerChatGroupMember(string groupName, OneOf<string, string[]> memberName)
```

其中：
  - groupName: 群聊名称
  - memberName: 可以是单个好友，也可以是一个好友昵称列表

## 删除群聊

> 删除群聊，适用于自有群,与退出群聊不同，退出群聊是退出群聊，删除群聊会删除自有群的所有好友，然后退出群聊

```
public async Task<ChatResponse> DeleteOwnerChatGroup(string groupName)
```

- groupName: 群聊名称

## 移除群聊成员

移除群聊成员,适用于自有群

```
public async Task<ChatResponse> RemoveOwnerChatGroupMember(string groupName, OneOf<string, string[]> memberName)
```

其中:
  - groupName: 群聊名称
  - memberName: 成员名称

## 邀请群聊成员

适用在他有群邀请好友入群

方法定义:

```
public async Task<ChatResponse> InviteChatGroupMember(string groupName, OneOf<string, string[]> memberName, string helloText = "")
```

其中：

  - groupName: 群聊名称
  - memberName: 成员名称
  - helloText: 给群主打招呼的文本

## 群中加好友
添加群聊里面的指定好友为自己的好友,适用于从他有群中添加所有好友为自己的好友

> 注意：此方法容易引起微信风控退出，为了安全建议用下面分页添加好友的方法： [分页添加好友](#群里加好友二)

方法定义:

```
public async Task<ChatResponse> AddChatGroupMemberToFriends(string groupName, OneOf<string, string[]> memberName, int intervalSecond = 5, string helloText = "", string label = "")
```

其中：
  - groupName: 群聊名称
  - memberName: 要增加的群里的好友列表,可以单个好友，也可以设置多个
  - intervalSecond: 间隔时间，增加每个好友时的间隔时间
  - helloText: 给要增加的好友打招呼的方式
  - label: 好友标签，方便分类管理

## 群里加好友二
 添加群聊里面的所有好友为自己的好友,适用于从他有群中添加所有好友为自己的好友

 方法定义:
 ```
 public async Task<ChatResponse> AddAllChatGroupMemberToFriends(string groupName, List<string> exceptList = null, int intervalSecond = 3,
    string helloText = "", string label = "", int pageNo = 1, int pageSize = 15)
 ```
其中:
  - groupName: 群聊名称
  - exceptList: 可选，排除的好友列表，如：群主本来就在自己通讯录，可以设置排除
  - intervalSecond: 间隔时间，增加每个好友时的间隔时间
  - helloText: 给好友打招呼的方式
  - label: 为新增好友设置的标签，方便分类管理
  - pageNo: 起始页码,从1开始,如果从0开始，表示不使用分页，全部添加好友，但容易触发微信风控机制，建议使用分页添加
  - pageSize: 每页数量,为增加的每页好友的数量

## 群里加好友三

添加群聊里面的所有好友为自己的好友,适用于从他有群中添加所有好友为自己的好友

>  注意：此方法容易触发微信风控机制，建议使用分页添加，并使用键鼠模拟器的方式增加好友。

方法定义:
```
public async Task<ChatResponse> AddAllChatGroupMemberToFriends(string groupName, Action<AddGroupMemberOptions> options)
```

其中：
  - groupName: 群聊名称
  - options: 添加群聊成员为好友的选项,具体请参考:[AddGroupMemberOptions类](../api/WeChatAuto.Models.AddGroupMemberOptions.html)


