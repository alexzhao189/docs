# 认识POM对象
---

WeChatAuto.SDK 采用页面对象模型（POM）设计理念，将微信的各类操作场景进行了模块化、清晰的组件封装。你只需要理解这些 POM 对象及其所对应的微信界面组件，就能便捷地调用其公开方法和属性，大幅简化功能开发和后期维护。如果你熟悉 WeChatAuto.SDK 的 POM 组件体系，基本上已经掌握了 60% 的核心用法😊。

## 🚀 WeChatAuto.SDK POM 组件结构图

![WeChatAuto.SDK 主要类关系示意](https://raw.githubusercontent.com/scottfly189/WeChatAuto.SDK/master/Images/class.png)

## 🎁 WeChatAuto.SDK的页面组件介绍

**🎈🎈 基础术语**

- 主窗口： 微信聊天的主窗口

- 子窗口： 双击会话打开的弹出窗口

### 1. WeChatClientFactory组件

<span style="color: #999;">本组件无UI</span>

WeChatClientFactory组件是非常重要的核心组件，负责多微信客户端的统一管理。SDK初始化后，可以通过依赖注入获取该组件。随后，可以通过WeChatClientFactory组件获取具体的微信客户端（[WeChatClient](../api/WeChatAuto.Components.WeChatClient.html)）实例，实现对不同微信账号的操作和管理。

```csharp
// 初始化WeAutomation服务
var serviceProvider = WeAutomation.Initialize(options =>
{
    options.DebugMode = true;   //开启调试模式，调试模式会在获得焦点时边框高亮，生产环境建议关闭
    //options.EnableRecordVideo = true;  //开启录制视频功能，录制的视频会保存在项目的运行目录下的Videos文件夹中
});
//获取到WeChatClientFactory
using var clientFactory = serviceProvider.GetRequiredService<WeChatClientFactory>();
```

> 由于WeChatClientFactory是以```Singletion```的方式加入依赖注入容器中，所以我们可以很方便的通过构造器注入等方式获取到它,具体请参考[WeChatClientFactory](../api/WeChatAuto.Components.WeChatClientFactory.html)

- WeChatClientFactory组件的Initialize()方法的参数WeChatConfig配置对象说明：

| 属性名 | 说明 |
|--------|-----|
| CaptureUIPath |进行截图操作时，捕获UI界面保存的路径 默认保存到当前目录下的Capture文件夹,可以修改为其他路径|
|ListenInterval|群或者好友消息监听的时间间隔,单位：秒，默认5秒监听一次|
|MomentsListenInterval|朋友圈监听的时间间隔,单位：秒，默认10秒监听一次|
|NewUserListenerInterval|监听新用户的时间间隔，单位：秒，默认5秒监听一次|
|MonitorSubWinInterval|监听子窗口时间间隔，单位秒,默认5秒监听一次|
|DebugMode|是否启用调试模式，启用了调试模式可以在运行时看到按钮高亮，默认false|
|EnableRecordVideo|是否启用视频录制，如果启用，可以录制整个自动化运行过程，默认false|
|TargetVideoPath|自定义视频录制文件保存位置，默认保存到当前目录下的Video文件夹,可以修改为其他路径|
|EnableMouseKeyboardSimulator|是否启用鼠标键盘模拟器,启用后，键盘鼠标操作会通过模拟器进行操作，而不是通过windows automation进行操作,注意：需要购买键鼠模拟器，并在此处启用|
|KMDeiviceVID|配置键鼠模拟器设备VID|
|KMDeivicePID|配置键鼠模拟器设备PID|
|KMVerifyUserData|配置键鼠模拟器用户校验数据|
|KMOutputStringType|配置键鼠模拟器输出字符串编码类型,默认使用剪贴板粘贴输出字符串。优点是输出字符多时速度更快且不受输入法影响|
|KMMouseMoveMode|配置键鼠模拟器鼠标移动模式|
|KMOffsetOfClick|点击偏移量,单位像素,为了避免每次点击都点击到同一个位置，可以设置一个偏移量，实际点击位置为点击位置减去偏移量的一个随机值|
|ProcessDpiAwareness|进程DPI感知值,如果使用库的应用已经设置DPI感知，此参数无效，可设置参数为:0: 不设置,进程对DPI完全不知晓，按逻辑像素绘制，可能会出现点击不准确的情况。1: PROCESS_SYSTEM_DPI_AWARE 默认值,进程只根据主显示器DPI绘制，DPI感知生效。 2: PROCESS_PER_MONITOR_DPI_AWARE，进程根据每个显示器DPI绘制,DPI感知生效。|

> 更具体的了解WeChatConfig配置对象，请参考: [WeChatConfig配置对象](../api/WeAutoCommon.Configs.WeChatConfig.html)

### 2. WeChatClient组件

**WeChatClient**组件代表一个微信客户端对象，这是整个POM组件体系的**入口**，只有**先**获取```WebChatClient```,再通过```WebChatClient```获取POM的其他组件

<span style="color: #999;">本组件无UI</span>

- 通过```WeChatClientFactory```获取到```WeChatClient```对象
- ```WeChatClient```对象使用委托模式转发调用其他各个组件的方法：如：消息管理，监听等，让客户端只需与 WeChatClient 交互，而不需要了解底层组件,当然更详细的API调用，还得了解```WeChatClient```包装的其他组件
- 具体请参见[WeChatClient](../api/WeChatAuto.Components.WeChatClient.html)类

### 3. WeChatNotifyIcon组件
本组件封装并抽象了微信在任务栏中的托盘图标（NotifyIcon），如下图所示：

<img src="../Images/notifyicon.png" alt="WeChatNotifyIcon" width="500"/>

> 可以通过WeChatClient.WxNotifyIcon属性获取到WeChatNotifyIcon对象，并执行方法;

更详细请参考[WeChatNotifyIcon组件](../api/WeChatAuto.Components.WeChatNotifyIcon.html)

### 4. WeChatMainWindow组件

本组件封装并抽象了微信窗口，封装的微信窗口，包含工具栏、导航栏、搜索、会话列表、通讯录、聊天窗口等

- 可以通过```WeChatClient```组件的WxMainWindow属性获取到微信客户端所属的WeChatMainWindow组件对象
- ```WeChatMainWindow组件```可以调用丰富的属性与方法,```WeChatMainWindow组件```也是各个其他组件的入口

对应微信部位如下图所示:

<img src="../Images/wechatwindow.png" alt="WeChatMainWindow" width="500"/>

<br/>

> 具体请参考[WeChatMainWindow组件](../api/WeChatAuto.Components.WeChatMainWindow.html)

### 4. Toolbar组件
本组件封装并抽象了微信右上角的工具栏，可以通过Toolbar组件设置置顶/取消置顶、最小化、最大化、关闭等操作,对应微信的位置如下:

<img src="../Images/toolbar.png" alt="Toolbar" width="500"/>

> 具体请参考[Toolbar组件](../api/WeChatAuto.Components.ToolBar.html)

### 5. Navigation组件
本组件封装并抽象了微信左侧的菜单，可以通过Navigation组件点击左侧的聊天、通讯录、收藏等按钮,对应微信位置如下:

<img src="../Images/Navigation.png" alt="Navigation" width="500"/>

> 具体请参考[Navigation组件](../api/WeChatAuto.Components.Navigation.html)

### 6. Moments组件
本组件封装并抽象了微信朋友圈，可以提供打开朋友圈、获取朋友圈内容列表、刷新朋友圈等操作，对应微信的位置如下：

<img src="../Images/monents.png" alt="Moments" width="700"/>

> 具体请参考[Moments组件](../api/WeChatAuto.Components.Moments.html)

### 7. ChatContent组件
ChatContent组件是很重要的一个组件，最主要通过它得到```ChatHeader```、```ChatBody```等组件，对应微信的位置如下：

> 主窗口与子窗口都有ChatContent组件

<img src="../Images/chatcontent.png" alt="ChatContent" width="500"/>

> 具体请参考[ChatContent组件](../api/WeChatAuto.Components.ChatContent.html)

### 8. ChatHeader组件
ChatHeader组件提供了获取主窗口与子窗口的聊天对象的标题，对应微信的位置如下：

> 主窗口与子窗口都有ChatHeader组件

<img src="../Images/chatheader.png" alt="ChatHeader" width="700"/>

> 具体请参考[ChatHeader组件](../api/WeChatAuto.Components.ChatHeader.html)

### 9. ChatBody组件
ChatBody组件封装了聊天体，可以用它来获取聊天列表```MessageBubbleList```与```Sender```组件,对应微信的位置如下所示：

> 主窗口与子窗口都有ChatBody组件

<img src="../Images/chatbody.png" alt="ChatBody" width="700"/>

> 具体请参考[ChatBody组件](../api/WeChatAuto.Components.ChatBody.html)

### 10. MessageBubbleList组件与MessageBubble组件
```MessageBubbleList```组件封装了一个聊天列表，一个```MessageBubbleList```包含多个```MessageBubble```

一个```MessageBubble```组件封装了一条消息，包括发送消息人，消息内容,时间等信息

对应的微信的位置如下：

> 主窗口与子窗口都有MessageBubbleList组件与MessageBubble组件

<img src="../Images/MessageBubbleList.png" alt="MessageBubbleList组件与MessageBubble组件" width="500"/>

> 具体请参考[MessageBubbleList组件与MessageBubble组件](../api/WeChatAuto.Components.MessageBubbleList.html)

### 11. Sender组件

Sender组件封装了一个发送器，可以用它来发送文本消息、Emoji字符串、文件等，对应的微信内容如下:

> 主窗口与子窗口都有Sender组件

<img src="../Images/Sender.png" alt="Sender组件" width="500"/>

> 具体请参考[Sender组件](../api/WeChatAuto.Components.Sender.html)

### 12. ConversationList会话列表组件与Conversation组件

ConversationList组件封装了一个会话列表，Conversation封装了一个会话，对应微信位置如下：

> 只有主窗口才有ConversationList会话列表组件与Conversation组件

<img src="../Images/Conversion.png" alt="SConversationList会话列表组件与Conversation组件" width="500"/>

> 具体请参考[ConversationList会话列表组件与Conversation组件](../api/WeChatAuto.Components.ConversationList.html)

### 13. Search组件
Search组件封装了微信搜索框,对应微信位置如下:

> 只有主窗口才有Search组件

<img src="../Images/Search.png" alt="Search组件" width="500"/>

> 具体请参考[Search组件](../api/WeChatAuto.Components.Search.html)

### 13. SubWinList组件
SubWinList组件封装了所有的子窗口的操作，如：启动子窗口，关闭子窗口，监听子窗口关闭再打开等，对应位置如下:

> 可以通过WeChatMainWindow组件得到SubWinList组件

<img src="../Images/subwinlist.png" alt="SubWinList组件" width="700"/>

> 具体请参考[SubWinList组件](../api/WeChatAuto.Components.SubWinList.html)

### 13. SubWin组件
SubWinList组件封装了一个子窗口,子窗口为双击会话弹出的窗口，可以通过SubWinList获取ChatContent,通过ChatContent获取到MessageBubbleList与Sender等子组件，对应位置如下:


<img src="../Images/SubWin.png" alt="SubWin组件" width="700"/>

> 具体请参考[SubWin组件](../api/WeChatAuto.Components.SubWin.html)






