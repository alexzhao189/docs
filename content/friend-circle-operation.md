# 朋友圈管理
---

## 打开朋友圈

方法定义
```
public void OpenMoments()
```

通过主窗口打开朋友圈窗口

## 判断朋友圈是否打开

方法定义:
```
public bool IsMomentsOpen()
```

判断朋友圈打开状态，如果朋友窗口存在，则返回```True```,否则返回```False```

## 获取朋友圈内容列表

方法定义:
```
public List<MomentItem> GetMomentsList(int count = 20)
```

其中:
  - count: 可选，鼠标滚动次数,一次滚动5行，由于朋友圈动态可能过长，所以做了此限制

返回:
一个```MomentItem```对象列表，```MomentItem```包含了朋友圈动态所需要的信息，具体请参考: [MomentItem类](../api/WxAutoCommon.Models.MomentItem.html)

## 静默模式获取朋友圈内容列表

静默模式意味着在朋友圈窗口不置顶的情况下获取内容

方法定义:
```
public List<MomentItem> GetMomentsListSilence()
```

返回:
一个```MomentItem```对象列表，```MomentItem```包含了朋友圈动态所需要的信息，具体请参考: [MomentItem类](../api/WxAutoCommon.Models.MomentItem.html)

## 刷新朋友圈

方法定义:
```
public void RefreshMomentsList()
```

点击朋友圈窗口的刷新按钮

## 朋友圈点赞

对设定的好友的朋友圈动态进行自动点赞

方法定义:
```
public void LikeMoments(OneOf<string, string[]> nickNames)
```

其中:
  - nickNames: 好友昵称或好友昵称列表

## 回复朋友圈动态

方法定义:
```
public void ReplyMoments(OneOf<string, string[]> nickNames, string replyContent)
```

其中:
  - nickNames: 好友昵称或好友昵称列表
  - replyContent: 回复内容
