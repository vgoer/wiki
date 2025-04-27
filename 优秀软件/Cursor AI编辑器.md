<center>Cursor</center>









[toc]





## Cursor AI编辑器

> AI代码编辑器： [cursor](https://www.cursor.com/)
>
> 视频教程： [bilibili](https://space.bilibili.com/14097567/channel/collectiondetail?sid=3749940)









### 1. 安装

```shell
yay -S aur/cursor-bin
```











### 2.无限使用

> 无限使用： 使用无限邮箱登陆
>
> [无限邮箱](https://2925.com/)

```shell
goer123abc..@2925.com
goer123abc..+任意数字@2925.com
```







### 3. 脚本

> go-cursor-help: [github](https://github.com/yuaotian/go-cursor-help)

```shell
# linux 
curl -fsSL https://raw.githubusercontent.com/yuaotian/go-cursor-help/master/scripts/install.sh | sudo bash

# windows
irm https://raw.githubusercontent.com/yuaotian/go-cursor-help/master/scripts/install.ps1 | iex

# 重置
sudo cursor-id-modifier
```











### 4. 使用

> cursor 技巧： [blog](https://learn-cursor.com/blog/posts/cursor-quick-start.html)



掌握这些快捷键，让你的开发效率翻倍：

- `Command + K`：内联编辑
- `Command + L`：AI 对话
- `Command + I`：启动 Composer
- `Command + Shift + P`：命令面板
- `Command + Shift + F`：全局搜索







> Cursor Rules:
>
> 全局配置： 

1. 打开 Cursor 设置
2. 导航至 `General` > `Rules for AI`
3. 在文本区域输入自定义指令
4. 保存设置

```shell
# 全局 AI 规则示例
1. 代码风格：
   - 使用 TypeScript 类型注解
   - 遵循函数式编程范式
   - 保持代码简洁可读

2. 错误处理：
   - 添加完整的错误处理逻辑
   - 使用自定义错误类型
   - 包含错误日志和监控

3. 性能优化：
   - 优先考虑性能影响
   - 提供优化建议
   - 避免不必要的计算

4. 文档和注释：
   - 为复杂逻辑添加注释
   - 生成 JSDoc 文档
   - 包含使用示例
```

> 鲜蘑菇指定规则:

在项目根目录创建 `.cursorrules` 文件：

```yaml
// .cursorrules
You are an expert in TypeScript, Node.js, and React development.

Code Style:
- Use functional programming patterns
- Prefer immutability
- Follow SOLID principles

TypeScript Usage:
- Use strict type checking
- Prefer interfaces over types
- Implement proper error handling

React Patterns:
- Use functional components
- Implement proper state management
- Follow React best practices

Testing:
- Write unit tests for core functionality
- Include integration tests
- Follow TDD principles when possible

Performance:
- Optimize for runtime performance
- Consider memory usage
- Implement proper caching strategies
```

> 高级Rules: [blog](https://learn-cursor.com/blog/posts/cursor-rules-advanced.html)





### 5. MCP

> MCP 是一个开放协议，用于标准化应用程序向大语言模型提供上下文的方式。可以将 MCP 想象成 AI 应用程序的 USB-C 接口。就像 USB-C 为设备连接各种外设和配件提供了标准化方式一样，MCP 为 AI 模型连接不同的数据源和工具提供了标准化方式。
>
> [doc](https://modelcontextprotocol.io/introduction) [github](https://github.com/GLips/Figma-Context-MCP)
>
> [教程](https://www.framelink.ai/docs/quickstart?utm_source=github&utm_medium=referral&utm_campaign=readme) [awesome-mcp](https://github.com/punkpeye/awesome-mcp-servers)





[1. 获取 Figma 访问令牌](https://www.framelink.ai/docs/quickstart?utm_source=github&utm_medium=referral&utm_campaign=readme#figma-access-token)

在向 Framelink Figma MCP 服务器发出请求之前，您需要生成一个 Figma 访问令牌。

1. 在 Figma 主页上，单击左上角的个人资料图标，然后`Settings`从下拉菜单中进行选择。
2. 在设置菜单中，选择`Security`选项卡。
3. 向下滚动到该`Personal access tokens`部分并单击`Generate new token`。
4. 输入令牌的名称并确保您对*文件内容*和*开发资源*具有读取权限，然后单击`Generate token`。



[2. 将 Framelink Figma MCP 服务器添加到你的 IDE](https://www.framelink.ai/docs/quickstart?utm_source=github&utm_medium=referral&utm_campaign=readme#configure-ide)

大多数 IDE 都支持 MCP 服务器的 JSON 配置。这使得入门变得简单。在 IDE 中更新 MCP 配置文件后，MCP 服务器将自动下载并启用。

要添加 Framelink Figma MCP 服务器，请使用以下配置：

> mac/linux

```json
{
  "mcpServers": {
    "Framelink Figma MCP": {
      "command": "npx",
      "args": [
        "-y",
        "figma-developer-mcp",
        "--figma-api-key=YOUR-KEY",
        "--stdio"
      ]
    }
  }
}

```

> windows

```json
{
  "mcpServers": {
    "Framelink Figma MCP": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "figma-developer-mcp",
        "--figma-api-key=YOUR-KEY",
        "--stdio"
      ]
    }
  }
}

```



> 或者
>
> 本地运行figma服务

```shell
npx figma-developer-mcp --figma-api-key=YOUR-KEY
```

> cursor mcp

```json
{
  "mcpServers": {
    "Framelink Figma MCP": {
      "url": "http://localhost:3333/sse"
    }
  }
}
```







### 6. 浏览器mcp

> 浏览器 MCP 是一个模型上下文提供程序 (MCP) 服务器，允许 AI 应用程序控制您的浏览器
>
> [github](https://github.com/browsermcp/mcp) [doc](https://docs.browsermcp.io/welcome)







### 7. magic-mcp

> 它类似于 v0，但在你的 Cursor/WindSurf/Cline 中。21st dev Magic MCP 服务器用于与你的前端一起工作，就像 Magic
>
> [github](https://github.com/21st-dev/magic-mcp)

1. **生成 API 密钥**
   - 访问[21st.dev Magic Console](https://21st.dev/magic/console)
   - 生成新的 API 密钥



2. 手动配置mcp

```json
{
  "mcpServers": {
    "@21st-dev/magic": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@21st-dev/magic@latest",
        "API_KEY=\"xxxxx\""
      ]
    }
  }
}
```

> 自动： 

```shell
cmd /c npx -y @21st-dev/cli@latest install cursor --api-key "xxxx"
```



3. 使用

> 使用`cursor`

```shell
/ui create a modern navigation bar with responsive design
```

