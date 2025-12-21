# MCP Server介绍

MCP（Model Context Protocol，模型上下文协议）是专为大语言模型（LLM, Large Language Model）打造的服务端组件，其核心能力在于为LLM动态提供丰富的可调用工具、外部资源、结构化提示词等高阶能力，使模型不仅能处理自然语言对话，更可灵活组合、调用各类外接能力，显著扩展大模型的应用边界。

MCP Server 的主要优势体现在：

- **一站式工具与资源调度**：集中管理并安全开放各类内置或第三方API、插件、知识库，让模型可按需动态调用和组合。
- **结构化提示词与上下文封装**：支持先进的 Prompt 工程和多轮上下文封装，使LLM能灵活切换任务模式及对外部信息的引用。
- **多模态和可追踪交互**：不仅支持文本，还兼容图像、表格等多模态输入输出，同时自动记录与分析上下文流程，便于追踪和溯源。
- **标准化与易集成**：MCP基于开放协议设计，兼容各类LLM平台（如个人Agent、企业Bot、插件市场等），快速集成，一处接入全局赋能。

如果你在构建大语言模型驱动的应用或平台，MCP Server 能让你的LLM随时具备调用工具、获取知识、使用专业提示词等超强能力，是新一代智能体与大模型协作的基础设施。

## WeChatAuto.SDK MCP示例 - 以Vscode为例

> **本文主要演示vscode使用mcp server,你也可以在自己的应用加入mcp client代码，通过mcp client来调用LLM来操作微信桌面客户端.**

### 步骤一： 打开mcp.json
- 在项目的根目录运行```code .```打开vscode编辑器
- 在vscode中打开.vscode --> 打开mcp.json文件,如下图所示：

![vscode](../Images/mcp.png)

### 步骤二：修改mcp.json

```
{
	"servers": {
		"wechat_mcp_server": {
			"type": "stdio",
			"command": "dotnet",
			"args": [
				"run",
                "--project",
                "你的WeChatAuto.MCP.csproj路径，如：D:\\repo\\wxauto.net\\WeChatAuto.SDK\\src\\WeChatAuto.MCP\\WeChatAuto.MCP.csproj"
			]
		}
	}
}
```

### 步骤三：修改好mcp.json后，运行mcp server,如下图所示

![mcp.json](../Images/mcp1.png)

- 启动后，如下图所示，你会看到MCP Server公布出来的工具与资源：

![mcp.json](../Images/mcp2.png)

### 步骤四: 打开github copilot chat，如下图所示:

![mcp.json](../Images/mcp3.png)

### 步骤五：在 github copilot chat 对话框中直接输入你想自动化微信的指令，例如：

```
请帮我给微信好友:AI.net发送消息: Hello world!!
```

> 具体详细使用请参考微软相关文档

