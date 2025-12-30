# 通讯录管理
---

## 获取通讯录所有好友

方法签名:

```
public List<string> GetAllFriends()
```

返回通讯中所有好友昵称

## 定位好友
在通讯录中定位好友，即使好友在UI界面中可见

方法签名:
```
public bool LocateFriend(string friendName)
```

其中:
  - friendName: 好友昵称
返回:

  如果找到好友，返回```True```,否则为```False```

## 获取所有公众号
在通讯录中获取所有公众号列表

方法签名:

```
public List<string> GetAllOfficialAccount()
```

## 获取所有待添加好友
当有其他人申请你为好友时，会在待添加好友列表中

方法签名:

```
public List<string> GetAllWillAddFriends(string keyWord = null)
```

其中:
  - keyWord: 关键词，待添加好友是否设定此关键词，如果设置了关键词，则仅返回添加 关键词 的好友

返回：
  返回待添加的好友列表

## 通过新好友

方法签名:

```
public List<string> PassedAllNewFriend(string keyWord = null, string suffix = null, string label = null)
```

其中:
  - keyWord: 可选，如果设置关键字，则通过包含关键字的新好友，如果没有设置，则通过所有新好友
  - suffix: 可选，如果设置后缀，则在添加好友时，会在好友昵称后添加后缀,方便分类管理
  - label: 可选，如果设置标签，则在添加好友时，会为此好友设置标签，方便分类管理

## 移除好友

方法签名:

```
public bool RemoveFriend(string nickName)
```

- nickName: 好友昵称

## 添加好友
在已知好友微信号或者电话号码的情况下添加好

> 注意：不能添加太频繁，否则可能会触发微信的风控机制，导致加好友失败

```
public List<(string friendName, bool isSuccess, string errMessage)> AddFriends(List<string> friendNames, string label = "")
```

其中:
  - friendNames: 微信号/手机号列表
  - label: 可选好友标签，如果设置了```label```,则在添加好友的时候设置标签,方便分类管理

## 添加好友二
在已知好友微信号或者电话号码的情况下添加好

> 注意：不能添加太频繁，否则可能会触发微信的风控机制，导致加好友失败

```
    public bool AddFriend(string friendName, string label = "")
```

其中:
  - friendNames: 微信号/手机号列表
  - label: 可选，好友标签，如果设置了```label```,则在添加好友的时候设置标签,方便分类管理

