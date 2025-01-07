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

