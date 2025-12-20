# 认识POM对象
---

WeChatAuto.SDK 基于 POM（页面对象模型）设计理念，对微信的不同操作场景进行了清晰、模块化的对象封装。只需要了解各个 POM 对象及其对应的微信界面组件，即可调用这些对象的公开方法和属性，简化功能实现与维护。

## 🚀 WeChatAuto.SDK POM 组件结构图

![WeChatAuto.SDK 主要类关系示意](https://raw.githubusercontent.com/scottfly189/WeChatAuto.SDK/master/Images/class.png)

## 🎁 WeChatAuto.SDK的页面组件介绍

### 1. WeChatClientFactory组件

<span style="color: #999;">本组件无UI</span>

WeChatClientFactory组件是非常重要的核心组件，负责多微信客户端的统一管理。SDK初始化后，可以通过依赖注入获取该组件。随后，可以通过WeChatClientFactory组件获取具体的微信客户端（[WeChatClient](/api/WeChatAuto.Components.WeChatClient.html)）实例，实现对不同微信账号的操作和管理。

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

> 由于WeChatClientFactory是以```Singletion```的方式加入依赖注入容器中，所以我们可以很方便的通过构造器注入等方式获取到它,具体请参考[WeChatClientFactory](/api/WeChatAuto.Components.WeChatClientFactory.html)

- WeChatClientFactory组件的Initialize()方法的参数WeChatConfig配置对象说明：

| 属性名 | 说明 |
|--------|-----|
| CaptureUIPath |捕获UI保存路径 默认保存到当前目录下的Capture文件夹,可以修改为其他路径|
|ListenInterval|消息监听的时间间隔,单位：秒，默认5秒监听一次|
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
|OutputStringType|配置键鼠模拟器输出字符串编码类型,默认使用剪贴板粘贴输出字符串。优点是输出字符多时速度更快且不受输入法影响|
|MouseMoveMode|配置键鼠模拟器鼠标移动模式|
|OffsetOfClick|点击偏移量,单位像素,为了避免每次点击都点击到同一个位置，可以设置一个偏移量，实际点击位置为点击位置减去偏移量的一个随机值|
|ProcessDpiAwareness|进程DPI感知值,如果使用库的应用已经设置DPI感知，此参数无效，可设置参数为:0: 不设置,进程对DPI完全不知晓，按逻辑像素绘制，可能会出现点击不准确的情况。1: PROCESS_SYSTEM_DPI_AWARE 默认值,进程只根据主显示器DPI绘制，DPI感知生效。 2: PROCESS_PER_MONITOR_DPI_AWARE，进程根据每个显示器DPI绘制,DPI感知生效。|

> 更具体的了解WeChatConfig配置对象，请参考: [WeChatConfig配置对象](/api/WeAutoCommon.Configs.WeChatConfig.html)

### 2. WeChatClient组件

**WeChatClient**组件代表一个微信客户端对象

<span style="color: #999;">本组件无UI</span>

- 通过WeChatClientFactory获取到WeChatClient对象
- WeChatClient对象使用委托模式转发调用其他各个组件的方法：如：消息管理，监听等，让客户端只需与 WeChatClient 交互，而不需要了解底层组件
- 更具体请参见[WeChatClient](/api/WeChatAuto.Components.WeChatClient.html)类

### 3. WeChatNotifyIcon组件
本组件抽象了任务栏的微信图标,如下图所示:

<img src="../Images/notifyicon.png" alt="WeChatNotifyIcon" width="500"/>

> 可以通过WeChatClient.WxNotifyIcon属性获取到WeChatNotifyIcon对象，并执行方法;

### 4. WeChatMainWindow组件


